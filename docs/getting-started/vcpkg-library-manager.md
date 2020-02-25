# Vcpkg Library Manager

[vcpkg](https://github.com/microsoft/vcpkg) is a C++ library manager for Windows, Linux and MacOS. Consider its [prerequisites](https://github.com/microsoft/vcpkg/blob/master/README.md#quick-start) before going further. 

The `vcpkg` library manager has preconfigured building procedures for `OpenSSL` and `pthreads` libraries. They can be easily built and further used as dependencies of SRT library.

**Note!** `vcpkg` does not support `LibreSSL` or `MbedTLS` libraries.

## 1. Configure vcpkg

Clone `vcpkg` using git, and build it.

```
git clone https://github.com/microsoft/vcpkg.git
cd vcpkg
.\bootstrap-vcpkg.bat
```

The current working directory will further be referenced as `VCPKG_ROOT`.

```
set VCPKG_ROOT=%cd%
```

## 2. Build SRT

There are two options to build SRT using `vcpkg`: by `vcpkg` itself (Section 2.1) or use `vcpkg` to build `openssl` and `pthreads`, and then build SRT from sources (Section 2.2). The second case makes it possible to build the latest SRT version, while the first case builds only the version included in `vcpkg`.

### 2.1. Build with vcpkg

SRT is included in `vcpkg` \([PR 8712](https://github.com/microsoft/vcpkg/pull/8712)\). Therefore, Vcpkg can build it with a single command.

```text
vcpkg install libsrt
```

### 2.2. Build SRT from sources

#### 2.2.1. Build openssl and pthreads

Then build `openssl` and `pthreads` libraries using **x64** toolset:

```
vcpkg install pthreads --triplet x64-windows
vcpkg install openssl --triplet x64-windows
```

The next step is to integrate `vcpkg` with the build system, so that `CMake` can locate `openssl` and `pthreads` libraries.

```
vcpkg integrate install
```

#### 2.2.2. Build SRT library

Clone the source code of `srt`.

```
git clone https://github.com/Haivision/srt.git srt
```

The current working directory will further be referenced as `SRT_ROOT`.

```
set SRT_ROOT=%cd%
```

From the `srt` cloned folder `SRT_ROOT` run `cmake` to generate build configuration files.

```
cd %SRT_ROOT%
md _build && cd _build
cmake ../ -G "Visual Studio 16 2019" -A x64
          -DCMAKE_TOOLCHAIN_FILE=%VCPKG_ROOT%\scripts\buildsystems\vcpkg.cmake
```

where `SRT_ROOT` and `VCPKG_ROOT` were defined above.

To build the project go to `%SRT_ROOT%\_build` and run:

```
cmake --build ./ --config Release
```

Alternatively Visual Studio IDE can be used by opening the generated solution file `SRT.sln` in the `_build` folder.