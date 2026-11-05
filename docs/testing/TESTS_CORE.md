# How to run Tests
This guide for running the tests.


We use Sanitizers and GoogleTest

## Example on Windows MSVC
```PowerShell
cmake --preset release-vcpkg-msvc -Dtests="on" -Dsanitizers="on"
cmake --build --preset release-vcpkg-msvc
ctest --preset release-vcpkg-msvc
```
## Example on Linux
### Ninja
```bash
cmake --preset release-ninja -Dtests="on" -Dsanitizers="on"
cmake --build --preset release-ninja
ctest --preset release-ninja
```
### Make
```bash
cmake --preset release-make -Dtests="on" -Dsanitizers="on"
cmake --build --preset release-make
ctest --preset release-make
```
