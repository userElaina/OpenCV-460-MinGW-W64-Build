# How to build

部分国家用户建议使用代理, 非正常网络环境可能导致错误.

#### 准备

获得 `MinGW-W64` 从 https://www.mingw-w64.org/downloads/#mingw-builds. 
选择你需要的版本, 解压并将 `mingw64\bin\` 添加到 PATH.

获得 `CMake` 从 https://cmake.org/download/. 

#### 克隆并编辑

`git clone` OpenCV 并回滚到发布的 commit.

```sh
git clone https://github.com/opencv/opencv.git
cd opencv
git reset --hard 725e440d278aca07d35a5e8963ef990572b07316
```

#### 配置并生成

打开 `cmake-gui.exe`, 选择 OpenCV 的文件夹.

点击 `Configure`.

选择 `MinGW Makefiles`, `Use default native compilers`.

点击 `Finish` 并等一会儿.

选择

```sh
BUILD_opencv_world
```

不选

```sh
ENABLE_PRECOMPILED_HEADERS
WITH_MSMF
WITH_OBSENSOR
```

如果存在上述选项. 
这是原因: https://forum.opencv.org/t/opencv-cmake-opencv-building-error/11566/11.

如果你只想要 C/C++ 依赖, 不选

```sh
BUILD_JAVA
BUILD_PERF_TESTS
BUILD_TESTS
BUILD_opencv_apps
BUILD_opencv_java_bindings_generator
BUILD_opencv_js
BUILD_opencv_js_bindings_generator
BUILD_opencv_python3
BUILD_opencv_python_bindings_generator
BUILD_opencv_python_tests
BUILD_opencv_ts
```

来加快速度.

点击 `Generate` 并等一会儿.

#### 建立

```sh
mkdir -p build
cd build
mingw32-make -j 32
mingw32-make install
```

根据你的计算机的性能选择线程数 (`-j` 后的数字).

然后所有你想要的文件都在 `build\install` 文件夹里.

#### More

编译命令将会像这样:

```sh
g++ "a.cpp" -o "a.exe" -I "$Env:OPENCV470\include" -I "$Env:OPENCV470\include\opencv2" -L "$Env:OPENCV470\x64\mingw\lib" -llibopencv_world470
```

其中 `$Env:OPENCV470` 是 `build\install\` 文件夹.

然后当你运行 `a.exe`, `libopencv_world470.dll` (在 `build\install\x64\mingw\bin` 文件夹中) 应当在 PATH 或 `.\` 中.
