# How to build

部分国家用户建议使用代理, 非正常网络环境可能导致错误. [中文](./README-zh_cn.md)

#### Preparation

Get `MinGW-W64` from https://www.mingw-w64.org/downloads/#mingw-builds. 
Select the version you need. Download, unzip and add `mingw64/bin/` to PATH.

Get `CMake` from https://cmake.org/download/. 

#### Clone and Modify

`git clone` OpenCV and allback to the commit of the release.

```sh
git clone https://github.com/opencv/opencv.git
cd opencv
git reset --hard 725e440d278aca07d35a5e8963ef990572b07316
```

#### Configure and Generate

Open `cmake-gui.exe`, choose where your source code is and where to build the binaries.

Click `Configure`.

Choose `MinGW Makefiles`, `Use default native compilers`.

Click `Finish` and wait a while.

Check

```sh
BUILD_opencv_world
```

and uncheck

```sh
ENABLE_PRECOMPILED_HEADERS
WITH_MSMF
WITH_OBSENSOR
```

if exists. 
That's why: https://forum.opencv.org/t/opencv-cmake-opencv-building-error/11566/11.

If you only what the C/C++ dependency library, uncheck

```sh
BUILD_JAVA
BUILD_PERF_TESTS
BUILD_TESTS
BUILD_opencv_apps
BUILD_opencv_java_bindings_generator
BUILD_opencv_js
BUILD_opencv_js_bindings_generator
BUILD_opencv_objc_bindings_generator
BUILD_opencv_python3
BUILD_opencv_python_bindings_generator
BUILD_opencv_python_tests
BUILD_opencv_ts
```

to speed things up.

Click `Generate` and wait a while.

#### Build

```sh
mkdir -p build
cd build
mingw32-make -j 32
mingw32-make install
```

Choose the number of threads (after `-j`) according to your computer's performance.

Then all the files you want are in the `build\install` folder.

#### More

The compile command will look like this:

```sh
g++ "a.cpp" -o "a.exe" -I "$Env:OPENCV470\include" -I "$Env:OPENCV470\include\opencv2" -L "$Env:OPENCV470\x64\mingw\lib" -llibopencv_world470
```

where `$Env:OPENCV470` is the `build\install` folder.

Then when you run `a.exe`, the `libopencv_world470.dll` (in `build\install\x64\mingw\bin`) should in the PATH or `.\`.
