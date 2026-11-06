# 🟦 Windows Build Guide
This guide is optimized to handle the challenges of building on Windows.

## 🛠️ Environment Setup with vcpkg

**VCPKG_ROOT :** Ensure this environment variable points to your Vcpkg installation.

### 1. Install Vcpkg
```bash
git clone https://github.com/Microsoft/vcpkg.git C:\vcpkg
cd C:\vcpkg
.\bootstrap-vcpkg.bat
# Recommended:  Make VCPKG_ROOT permanent to system PATH
```

### 2. Enable Binary Caching (Crucial!)
To avoid recompiling LLVM every time you clean your project, enable local binary caching. Check if it's ON by default.

```bash
# Create a permanent cache folder
mkdir C:\vcpkg_cache
# Set the variable (Make this permanent in Windows Environment Variables)
$env:VCPKG_BINARY_SOURCES = "clear;files,C:\vcpkg_cache"
```
---
## 🚀 Building the Project
Use the **Native Tools Command Prompt for VS**

We recommend using CMake Presets which automatically configure the correct generators and Vcpkg triplets. Don't forget to enable the UI with **-Ddesktop_ui=ON** if you want to compile also the ui


```bash
cmake --preset release-vcpkg-msvc
cmake --build --preset release-vcpkg-msvc
```

For Full Development (Debug):
```bash
cmake --preset debug-vcpkg-msvc -Dsanitizers="on" -Dtests="on"
cmake --build --preset debug-vcpkg-msvc
```

### Option B: With Qt Desktop GUI

#### If you have Qt

```bash
cmake --preset release-vcpkg-msvc -Ddesktop_ui="ON"
cmake --build --preset release-vcpkg-msvc
```

---
## 🔍 Available Windows Presets
|     Preset Name     |       Generator       | Build Type |       VCPKG Triplet       |               Optimization / Notes              |
|:-------------------:|:---------------------:|:----------:|:-------------------------:|:-----------------------------------------------:|
| debug-vcpkg-msvc    | Auto-detected         | Debug      | x64-windows               | Standard Debug Build.                           |
| release-vcpkg-msvc  | Auto-detected         | Release    | x64-windows-release       | Optimized for Space/Time Savings                |
