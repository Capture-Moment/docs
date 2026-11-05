# 🏗️ Halide Installation
This guide provides clear instructions for obtaining Halide with support for various GPU backends (CUDA, OpenCL, Vulkan, DirectX12) as well as CPU-only configurations.

---

# GPU Backend supported

| Backend | Platform | Requirements |
|---------|----------|--------------|
| CUDA | NVIDIA GPUs | NVIDIA drivers only, the CUDA Toolkit is NOT required |
| OpenCL | AMD/NVIDIA/Intel | OpenCL runtime (included with GPU drivers) |
| Vulkan | Cross-platform | Vulkan SDK and compatible driver |
| DirectX12 | Windows only | Windows 10/11, compatible GPU |

## How to Configure Halide

### 🥇 Method 1 : Pre-built Binary (Recommended & Fastest)
The official Halide binaries (CPU + all GPU backends) are distributed as a Python-independent wheel. **Python/pip** is only used as the downloader.

1. Install **Python** & **pip** on your machine (Python 3.12+ required).
#### Via PIP
```bash
sudo pip install --only-binary :all: halide==22.1.0
```

>💡 Linux note: on recent distributions, pip refuses to install outside a virtual environment, you can use venv or force install. You can see here [Official docs CMake Halide] (https://github.com/halide/Halide/blob/main/doc/HalideCMakePackage.md)

```bash
sudo pip install --break-system-packages --only-binary :all: halide==22.1.0
```
