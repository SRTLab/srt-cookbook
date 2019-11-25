# Vcpkg Library Manager

SRT is included in `vcpkg` \([PR 8712](https://github.com/microsoft/vcpkg/pull/8712)\). Vcpkg is C++ Library Manager for Windows, Linux, and MacOS. There are two options to build SRT using `vcpkg`.

### 1. Build SRT with vcpkg

Once SRT is added to the list of supported ports of vcpkg, the buld will be:

```text
vcpkg install libsrt
```

### 2. Build SRT with openssl and pthreads built with vcpkg

At the moment it can already be used to simplify SRT build. Vcpkg already has ports for `openssl` and `pthreads`, so the build is as easy as

```text
vcpkg install openssl
vcpkg install pthreads
```

Integrate vcpkg with the build system \(CMake will know about packaged installed by vcpkg\)

```text
vcpkg integrate install
```

Then build SRT:

```text
cmake ../ -G "Visual Studio 16 2019" -A x64
             -DCMAKE_TOOLCHAIN_FILE=[vcpkg root]\scripts\buildsystems\vcpkg.cmake
```

