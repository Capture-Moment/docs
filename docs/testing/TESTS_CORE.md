# How to run Tests
This guide for running the tests.


We use Sanitizers and GoogleTest

## Example on Windows MSVC
```bash
cmake --preset debug-vcpkg-msvc -Dtests="on" -Dsanitizers="on"
cmake --build --preset debug-vcpkg-msvc
ctest --preset debug-vcpkg-msvc
```
## Example on Linux with Ninja
```bash
cmake --preset debug-ninja -Dtests="on" -Dsanitizers="on"
cmake --build --preset debug-ninja
ctest --preset debug-ninja
```

## Example on macOS with Ninja
```bash
cmake --preset debug-homebrew -Dtests="on" -Dsanitizers="on"
cmake --build --preset debug-homebrew
ctest --preset debug-homebrew
```
