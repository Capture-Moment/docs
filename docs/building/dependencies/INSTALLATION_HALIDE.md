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

> Note: CUDA is never probed on macOS. Apple has shipped no CUDA driver since macOS 10.13.

## How to Configure Halide

### 🥇 Method 1 : Automated (Recommended)

**You do not need to install Halide by hand.** The platform setup scripts do it for you:

```bash
./setup/setup-macos.sh     # macOS
./setup/setup-debian.sh    # Debian / Ubuntu
./setup/setup-dnf.sh       # Fedora / RPM
```

They create a virtual environment at the root of the repository (`.venv/`) and install
the official Halide wheel into it. CMake finds it automatically — no environment
variable, no `CMAKE_PREFIX_PATH` entry to set.

### 🥈 Method 2 : Manual (pip wheel in a virtualenv)

The official Halide binaries (CPU + all GPU backends) are distributed as a
Python-independent wheel. **Python/pip** is only used as the downloader.

From the root of the repository:

```bash
python3 -m venv .venv
.venv/bin/python -m pip install --only-binary :all: halide==22.1.0
```

> ⚠️ **Python 3.12+ is required.** The Halide 22.1.0 wheels are built as `cp312-abi3`.
> An older interpreter resolves to Halide 21 at best, and the build then fails with
> `Halide 22.1+ not found`. Check yours with `python3 --version`.

> 💡 Debian/Ubuntu also need the `python3-venv` package, otherwise `python3 -m venv`
> fails with *"ensurepip is not available"*.

Use a virtualenv rather than `sudo pip install`: a system-wide install fights with the
distribution's package manager (recent distributions refuse it outright with
*"externally-managed-environment"*), and on macOS it lands in whichever `python3`
happens to be first on your `PATH`.

### How CMake locates Halide

`find_halide_package()` searches, in order:

1. `$HALIDE_PREFIX`, if set
2. the project virtualenv — `<repo>/.venv`
3. an activated virtualenv — `$VIRTUAL_ENV`
4. the usual system paths / `CMAKE_PREFIX_PATH`

For 2 and 3 it asks the interpreter directly (`python -c "import halide; print(halide.install_dir())"`),
so the wheel's exact layout — which contains the Python minor version — does not matter.

### Using an existing Halide installation

If you built Halide from source, or installed it somewhere else, point `HALIDE_PREFIX`
at the directory containing `lib/cmake/Halide/HalideConfig.cmake`:

```bash
export HALIDE_PREFIX=/path/to/halide
```

This is also how the CI pipelines pass Halide to the build.

> Official reference: [Halide CMake package documentation](https://github.com/halide/Halide/blob/main/doc/HalideCMakePackage.md)
