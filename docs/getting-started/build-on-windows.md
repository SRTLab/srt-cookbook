# Build on Windows

## Using NuGet

### 1. Download and install OpenSSL for Windows.

The 64-bit package can be downloaded from here: [Win64OpenSSL\_Light-1\_1\_1c.exe](http://slproweb.com/download/Win64OpenSSL_Light-1_1_1c.exe).

\(Note that the last letter or version number may be changed and older versions no longer available. If this isn't found, check here: [http://slproweb.com/products/Win32OpenSSL.html](http://slproweb.com/products/Win32OpenSSL.html) \)

It's expected to be installed in `C:\Program Files\OpenSSL-Win64`. Add this path to the user's or system's environment variable `PATH`.

### 2. Install Pthreads for Windows

nuget install cinegy.pthreads-win64 -version 2.9.1.17 -OutputDirectory C:\pthread-win32

### 3. Install cmake for Windows.

[CMake dowload page](https://cmake.org/download/).

The CMake GUI will help you configure the project.

### 4. Generate Visual Studio Solution.

Assuming you are currently in the cloned repo.

```text
mkdir _build & cd _build
cmake ../ -G"Visual Studio 16 2019" -A x64 -DPTHREAD_INCLUDE_DIR="C:\pthread-win32\cinegy.pthreads-win64.2.9.1.17\sources" -DPTHREAD_LIBRARY="C:\pthread-win32\cinegy.pthreads-win64.2.9.1.17\runtimes\win-x64\native\release\pthread_lib.lib"
cmake --build ./ --config Release
```

CMake will try to find OpenSSL and pthreads. If any of the is not found, you can define the following variables to help CMake find them:

```text
OPENSSL_ROOT_DIR=<path to OpenSSL installation>
OPENSSL_LIBRARIES=<path to all the openssl libraries to link>
OPENSSL_INCLUDE_DIR=<path to the OpenSSL include dir>

PTHREAD_INCLUDE_DIR=<path to where pthread.h lies>
PTHREAD_LIBRARY=<path to pthread.lib>
```

## Using vcpkg

