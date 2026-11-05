# How to run Tests
This guide for running the tests.


We use Sanitizers and GoogleTest

## Example on Windows MSVC
```bash
cmake --preset release-vcpkg-msvc -Dtests="on" -Dsanitizers="on"
cmake --build --preset release-vcpkg-msvc
ctest --preset release-vcpkg-msvc
```
## Example on Linux with Ninja
```bash
cmake --preset release-ninja -Dtests="on" -Dsanitizers="on"
cmake --build --preset release-ninja
ctest --preset release-ninja
```

