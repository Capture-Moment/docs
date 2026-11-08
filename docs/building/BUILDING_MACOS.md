# macOS Building Guide
This guide covers building CaptureMoment on macOS. The recommended method uses an automated script to build core dependencies from source, ensuring you get the exact same versions and configuration as our Linux and CI pipelines, avoiding unpredictable Homebrew updates.

##  Setting up Dependencies

### 1. Run the automated setup script
This method uses an automated script that compiles Exiv2 and OpenImageIO from source to guarantee strict version consistency, and installs Halide from the official wheel. (Note: The script uses Homebrew in the background to fetch the underlying base libraries required for the build).

From the root of the project, run:

```bash
./setup/setup-macos.sh
```
(Note: You can look inside this script if you prefer to run the installation steps manually).

The script performs four steps:

| Step | What it does | Where it lands |
|:----:|--------------|----------------|
| 1 | Installs base libraries via Homebrew | Homebrew prefix |
| 2 | Builds and installs Exiv2 from source | `/opt/exiv2` |
| 3 | Builds and installs OpenImageIO from source | `/opt/oiio` |
| 4 | Installs the Halide wheel in a virtualenv | `<repo>/.venv` |

Steps 2 and 3 use `sudo` to write to `/opt`, and are skipped if the directory already exists.

> 💡 Step 4 needs **Python 3.12+** (the Halide wheels are `cp312-abi3`). The script picks the
> newest suitable interpreter it can find and installs `python@3.13` via Homebrew if there
> is none. Beware that a version manager such as asdf may put an older `python3` first on
> your `PATH` — the script looks past it.
>
> ➡️ See the [Halide installation guide](dependencies/INSTALLATION_HALIDE.md) for details.

### 2. Install Qt (only for the Desktop UI)

Qt is **not** installed by the setup script. Two options:

**Homebrew** — note that Qt is split per module here, and `qtshadertools` is required:

```bash
brew install qtbase qtdeclarative qtshadertools qtsvg
```

Missing `qtshadertools` is the most common failure, and the error names the *parent* package rather than the missing module:

```
Failed to find required Qt component "ShaderTools".
```

**Official installer** — see the [Qt installation guide](dependencies/INSTALLATION_QT.md). This is the way to get the exact 6.10.2 the CI uses.

### 3. Start the project
#### Configure the project

**Core and tests only**
```bash
cmake --preset debug-homebrew -Dsanitizers=on -Dtests=on
```

##### Add Qt UI
* You can add -Ddesktop_ui=ON to compile with Qt

```bash
cmake --preset release-homebrew -Ddesktop_ui=ON
```

The `*-homebrew` presets already search `/opt/exiv2`, `/opt/oiio`, `/opt/homebrew` and
`/usr/local`, so a Homebrew Qt is found with no extra flag.

Only pass `Qt6_DIR` if you installed Qt through the official installer, and point it at a
path that actually exists:

```bash
# Adapt to your installation path - check that it exists first
cmake --preset release-homebrew -Ddesktop_ui=ON -DQt6_DIR="$HOME/Qt/6.10.2/macos/lib/cmake/Qt6"
```

> ⚠️ A `Qt6_DIR` pointing at a non-existent directory is **silently ignored** — CMake falls
> back to whatever Qt it finds on `CMAKE_PREFIX_PATH`. Check the configure output
> (`Qt Version: ...`) to confirm which one you are really building against.

#### Build the project
```bash
cmake --build --preset release-homebrew
# Compile using all available CPU cores
cmake --build --preset release-homebrew -j$(sysctl -n hw.ncpu)
```

#### Run the Desktop app
```bash
open build/release-homebrew/qt/desktop/CaptureMoment.app
# Or, to see the logs in the terminal:
./build/release-homebrew/qt/desktop/CaptureMoment.app/Contents/MacOS/CaptureMoment
```

This bundle is a **development** bundle: it links against your Homebrew prefix, `/opt` and
the Halide virtualenv, so it only runs on the machine that built it.

---
## 📦 Producing a distributable app bundle

To get a `.app` that runs on another Mac, run the install step. It deploys Qt (frameworks,
plugins and QML modules), copies Halide / OpenImageIO / Exiv2 and all their transitive
dependencies into `Contents/Frameworks`, rewrites every install name, and signs the result:

```bash
cmake --install build/release-homebrew --prefix build/release-homebrew/dist
```

The bundle lands at `build/release-homebrew/dist/CaptureMoment.app` (~300 MB — most of it
is `libHalide.dylib`).

Verify that nothing points outside the bundle:

```bash
otool -L build/release-homebrew/dist/CaptureMoment.app/Contents/MacOS/CaptureMoment
codesign --verify --deep --strict build/release-homebrew/dist/CaptureMoment.app
```

Two limitations to keep in mind:

* **Single architecture.** The build targets the host CPU, so a bundle built on Apple
  Silicon will not run on an Intel Mac (Rosetta only translates the other way). A universal
  binary would require the whole dependency stack to be built universal.
* **Ad-hoc signature.** Good enough for your own machines; distributing outside them needs
  a Developer ID signature and notarization, otherwise Gatekeeper blocks the download.

---
## 🔍 Available macOS Presets (Homebrew)

|    Preset Name   |  Generator  | Build Type |     Notes                                  |
|:----------------:|:-----------:|:----------:|:------------------------------------------:|
| debug-homebrew   | Ninja       | Debug      |Recommended. Uses Ninja for a fast build.   |
| release-homebrew | Ninja       | Release    | Optimized final build.                     |

---
## 🩺 Troubleshooting

| Symptom | Cause | Fix |
|---------|-------|-----|
| `Halide 22.1+ not found` | No virtualenv, or its Python is older than 3.12 | Re-run `./setup/setup-macos.sh`, or see the [Halide guide](dependencies/INSTALLATION_HALIDE.md) |
| `Failed to find required Qt component "ShaderTools"` | `qtshadertools` not installed | `brew install qtshadertools` |
| Wrong Qt version in the configure output | `Qt6_DIR` points nowhere and was ignored | Check the path exists, or drop the flag |
