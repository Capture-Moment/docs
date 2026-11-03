# 🏗️ Architecture & Technical Stack - CaptureMoment

This document outlines the technical architecture, dependencies, and design principles of the project.

## 🎯 Core Principles

Capture Moment is built on three pillars:
1.  **Non-Destructive Workflow**: The source file (RAW/TIFF) is never modified. Adjustments are stored as a metadata "recipe".
2.  **High-Performance Pipeline**: Uses **Halide** to fuse all processing operations into a single computation pass, avoiding intermediate memory reads/writes.
3.  **Modularity**: Strict separation between I/O (Loading), Compute (Math), and Display (UI).

---

## 📦 Technical Stack

| Library | vcpkg (Windows) | Homebrew (macOS) | apt (Linux) | Git Clone / CPM (Source) | Notes / Prerequisites |
| :--- | :---: | :---: | :---: | :---: | :--- |
| **OpenImageIO** | ✅ | ❌ | ❌ | ✅ *(Git Clone on macOS/Linux)* | Built from source on macOS and Linux |
| **Halide** | ❌ | ❌ | ❌ | ✅ *(Git Clone)* | 🐍 **Python 3** recommended |
| **magic_enum** | ❌ | ❌ | ❌ | ✅ *(CPM.cmake)* | Header-only (fetched via CMake) |
| **Exiv2** | ✅ | ✅ | ✅ | ❌ | System package manager preferred |
| **Qt6** | ❌ | ❌ | ❌ | ❌ | 📦 **Official Site (Qt Online Installer)** |
| **GoogleTest** | ❌ | ❌ | ❌ | ✅ *(CPM.cmake)* | Fetched automatically for unit testing |


### 1. Data Management & I/O (Source Manager)
* **Library:** [OpenImageIO (OIIO)](https://openimageio.readthedocs.io/)
* **Role:**
    * **Input/Output:** Reads RAW files (via integrated LibRaw), TIFF, EXR, JPG, etc..
    * **Image Cache:** Handles tiling and lazy loading to manage 50MP+ files with limited RAM.
    * **Metadata:** Manages EXIF, XMP, and IPTC data.
    * **Color:** Bridges with OpenColorIO.

### 2. Compute Engine (Pipeline Engine)
* **Library:** [Halide](https://halide-lang.org/)
* **Role:**
    * **Processing:** Executes algorithms (Exposure, Contrast, Sharpening, Denoise).
    * **AOT Compilation:** Filters are compiled Ahead-Of-Time into optimized machine code (AVX2/NEON/GPU).
    * **Optimization:** Automatic vectorization, parallelism, and loop fusion.
  
### 3. Metadata & Serialization
* **Libraries:**
    * [**Exiv2**](https://www.exiv2.org/): For robust reading and writing of XMP metadata packets within image files or sidecar files.
    * [**magic_enum**](https://github.com/Neargye/magic-enum): For safe and efficient conversion of enums to/from strings during XMP serialization.
* **Role:**
    * **Persistence:** Saving and loading the list of applied image adjustments as XMP metadata associated with the source image file.
    * **Type Safety:** Ensuring correct serialization/deserialization of operation parameters and types using `magic_enum`.

### 4. User Interface & Display
* **Framework:** [Qt6 (Quick/QML)](https://doc.qt.io/)
* **Role:**
    * **UI:** Modern, fluid, declarative interface.
    * **Rendering:** Uses **QRhi** (Qt Rendering Hardware Interface) to display Halide buffers directly on the GPU (Vulkan/Metal/D3D12).

### 5. Testing
* **Libraries:**
    * [**GoogleTest**](https://github.com/google/googletest): For test the project
* **Role:**
   * **Unit and Validation Testing:** Validates core components (Source Manager, Pipeline, etc..).

### 6. Dependency Management

* **Role:** Ensures all libraries (OIIO, OCIO, Halide, Qt, Exiv2, magic_enum) are compiled with the same ABI across all platforms.

#### General
* **Tool:** [CMakePackageManager](https://github.com/cpm-cmake/CPM.cmake/blob/master/README.md)
#### Windows
* **Tool:** [Vcpkg](https://vcpkg.io/)
#### Windows
* **Tool:** [Homebrew](https://brew.sh/)
#### UBuntu
* **Tool:** [APT](https://launchpad.net/ubuntu)

## Architectures

* [**Core Design**](./core/CORE_DESIGN.md).
* [**Qt Core Design**](./qt/core/QT_CORE_ARCHITECTURE.md).
* [**Qt Desktop Design**](./qt/desktop/QT_DESKTOP_ARCHITECTURE.md).

## 📚 Additional Resources

- **CMake Documentation:** [cmake.org/documentation](https://cmake.org/documentation/)
- **Qt Documentation:** [doc.qt.io](https://doc.qt.io/)
- **OpenImageIO:** [openimageio.readthedocs.io](https://openimageio.readthedocs.io/)
- **vcpkg:** [github.com/Microsoft/vcpkg](https://github.com/Microsoft/vcpkg)
- **Exiv2:** [exiv2.org](https://www.exiv2.org/)
- **magic_enum:** [github.com/Neargye/magic-enum](https://github.com/Neargye/magic-enum)
- **GoogleTest:** [github.com/google/googletest](https://github.com/google/googletest)
