
# 🚀 Build Guide - CaptureMoment

Welcome! This guide will help you build CaptureMoment from source.

## 🧰 Global Prerequisites

Before selecting your platform, ensure you have these tools:

1.  **CMake 4.2+**
2.  **C++23 Compiler**:
    * **Windows**: MSVC 2026.
    * **Linux**: GCC 16+
    * **macOS**: Apple Clang 16+ (Xcode 15+).
3.  **Python 3.12+** — used to fetch the Halide wheel. The platform setup scripts install
    it into a project virtualenv (`<repo>/.venv`); 3.12 is the minimum because the wheels
    are built as `cp312-abi3`.
4.  **Ninja** — the generator used by every recommended preset.

---
## ⚙️ Global Build Configuration

### CMake Build
|  Component | Default Status |     CMake Variable (with Alias)    |                  Description                  |
|:----------:|:--------------:|:-----------------------------:|:---------------------------------------------:|
| Desktop UI | OFF            | BUILD_DESKTOP_UI (desktop_ui) | Builds the Qt Quick/QML desktop application.  |
| Mobile UI  | OFF            | BUILD_MOBILE_UI (mobile_ui)   | Builds the mobile application (future phase). |
| Tests      | OFF            | BUILD_TESTS (tests)           | Builds unit and integration tests.            |
| Sanitizers | OFF            | BUILD_SANITIZERS (sanitizers)           | Builds with sanitizer instrumentation (ASan, UBSan).            |
| Benchmarks | OFF            | BUILD_BENCHMARKS (benchmarks) | Builds performance benchmarks.                |

### Libraries

| Library     | Version      | Link                                                     |
|-------------|:------------------:|----------------------------------------------------------|
| OpenImageIO | 3.1.14.0             | https://github.com/AcademySoftwareFoundation/OpenImageIO |
| Halide      | >=22.1           | https://github.com/halide/Halide                         |
| spdlog      | 1.16.0      | https://github.com/gabime/spdlog                         |
| Exiv2         | 0.28.7| https://github.com/Exiv2/exiv2  |
| Magic Enum         |0.9.7| https://github.com/Neargye/magic_enum/ |
|  Qt6           | 6.10.x| https://doc.qt.io/qt-6/ |
|  GoogleTest           | 1.18.0 | https://github.com/google/googletest/ |

---
## 📦 How to build

### 1. Build Halide
➡️ Read the guide for Halide.

### 2. Setup Qt (Optional - Required for UI only)
➡️ Read the Building Qt Guide

### 3. Setup Dependencies & Compile
git clone the project
```bash
# Clone from develop branch
git clone --branch develop --single-branch https://github.com/Capture-Moment/CaptureMoment.git
cd CaptureMoment

# Or from your Fork
```

### 4. Setup Dependencies & Compile
Click the link for your OS for detailed instructions:

* [🟦 **Windows**](BUILDING_WINDOWS.md).
* [🐧 **Linux**](BUILDING_LINUX.md) (Ubuntu, Fedora, Arch).
* [🍎 **macOS**](BUILDING_MACOS.md) (Homebrew and Xcode).
