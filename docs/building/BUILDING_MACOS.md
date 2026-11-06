# macOS Building Guide
This guide covers building CaptureMoment on macOS. The recommended method uses an automated script to build core dependencies from source, ensuring you get the exact same versions and configuration as our Linux and CI pipelines, avoiding unpredictable Homebrew updates.

##  Setting up Dependencies

### 1. Run the automated setup script
This method uses an automated script that compiles Exiv2 and OpenImageIO from source to guarantee strict version consistency. (Note: The script uses Homebrew in the background to fetch the underlying base libraries required for the build).

From the root of the project, run:

```bash
./setup/setup-macos.sh
```
(Note: You can look inside this script if you prefer to run the installation steps manually).

### 2. Start the project
#### Configure the project

**Compilation**
```bash
# Use the optimized Homebrew preset and enable the Desktop UI
cmake --preset debug-homebrew -Dsanitizers=on -Dtests=on
```

##### Add Qt UI
* You can add -Ddesktop_ui=ON to compile with Qt

```bash
# Adapt $HOME/Qt to your installation path
cmake --preset release-homebrew -Ddesktop_ui=ON -DQt6_DIR="$HOME/Qt/6.10.2/macos/lib/cmake/Qt6"
```

#### Build the project
```bash
cmake --build --preset debug-homebrew
# Compile using all available CPU cores
cmake --build --preset debug-homebrew -j$(sysctl -n hw.ncpu)
```

---
## 🔍 Available macOS Presets (Homebrew)

|    Preset Name   |  Generator  | Build Type |     Notes                                  |
|:----------------:|:-----------:|:----------:|:------------------------------------------:|
| debug-homebrew   | Ninja       | Debug      |Recommended. Uses Ninja for a fast build.   |
| release-homebrew | Ninja       | Release    | Optimized final build.                     |
