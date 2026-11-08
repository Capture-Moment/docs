# 🐧 Linux Building Guide
This guide covers building CaptureMoment on Linux. The recommended method uses an automated script to build dependencies from source, ensuring you get the exact same versions and configuration as our CI pipeline.


## 📦 Setting up Dependencies (Recommended) on Ubuntu/Debian
>Make sure you have GCC 16+

### 1. Run the automated setup script
This script compiles Exiv2 and OpenImageIO from source to guarantee version consistency,
and installs Halide from the official wheel.

>Note: You can look inside this script if you prefer to run the installation steps manually.
From the root of the project, run:

#### Debian / Ubuntu (apt)
Tested on Ubuntu 26.04 LTS

```bash
chmod +x setup/setup-debian.sh
./setup/setup-debian.sh
```
#### Fedora / RPM (dnf)
```bash
chmod +x setup/setup-dnf.sh
./setup/setup-dnf.sh
```

>🚨 Important: After the script, run in the terminal: `source ~/.bashrc`

The script performs five steps:

| Step | What it does | Where it lands |
|:----:|--------------|----------------|
| 1 | Installs development packages (apt / dnf) | System |
| 2 | Builds and installs Exiv2 from source | `/opt/exiv2` |
| 3 | Builds and installs OpenImageIO from source | `/opt/oiio` |
| 4 | Installs the Halide wheel in a virtualenv | `<repo>/.venv` |
| 5 | Persists `CMAKE_PREFIX_PATH` / `LD_LIBRARY_PATH` | `~/.bashrc` |

> 💡 Step 4 needs **Python 3.12+** (the Halide wheels are `cp312-abi3`). The script picks the
> newest suitable interpreter and tries to install one if there is none — on Ubuntu 22.04,
> where `python3` is 3.10 and `python3.12` is not in the default repositories, you will have
> to provide one yourself (deadsnakes PPA, pyenv, ...).
>
> ➡️ See the [Halide installation guide](dependencies/INSTALLATION_HALIDE.md) for details.

>Note:  If a system package is outdated on your distribution, please open an issue to let us know, and adapt the installation accordingly.

### 2. Start the project

#### Configure the project
* You can swap release-ninja for debug-ninja (or any other preset from the table below).

```bash
# Configure the project using a generic preset
cmake --preset debug-ninja -DBUILD_SANITIZERS=ON -DBUILD_TESTS=ON

# Or, for a release build
cmake --preset release-ninja -DBUILD_DESKTOP_UI=ON
```

##### Add Qt UI
* You can add -DBUILD_DESKTOP_UI=ON to compile with Qt
```bash
# Adapt $HOME/Qt to your installation path
cmake --preset release-ninja -DBUILD_DESKTOP_UI=ON -DQt6_DIR="$HOME/Qt/6.10.2/gcc_64/lib/cmake/Qt6"
```

> ⚠️ A `Qt6_DIR` pointing at a non-existent directory is **silently ignored** — CMake falls
> back to whatever Qt it finds on `CMAKE_PREFIX_PATH`. Check the configure output
> (`Qt Version: ...`) to confirm which one you are really building against.

#### Build the project
```bash
cmake --build --preset debug-ninja
# Compile using all available CPU cores
cmake --build --preset debug-ninja -j$(nproc)
```

#### Run the Qt Desktop
```bash
ldd build/release-ninja/CaptureMoment | grep "not found"
./build/release-ninja/CaptureMoment
```

---
## 🔍 Available Linux Presets

|  Preset Name |    Generator   | Build Type |                       Notes                      |
|:------------:|:--------------:|:----------:|:------------------------------------------------:|
| debug-ninja  | Ninja          | Debug      | Ninja generator for fast build (recommended).    |
| release-ninja| Ninja          | Release    | Ninja generator for fast build (recommended).    |
| release-make | Unix Makefiles | Release    | Classic Makefiles generator.                     |
| debug-make   | Unix Makefiles | Debug      | Classic Makefiles generator.                     |
