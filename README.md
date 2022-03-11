1. ## 安装 MinGW-w64 并配置环境变量

   安装：x64-4.8.1-release-posix-seh-rev5.7z，直接解压    

   为用户变量 Path 添加 E:\mingw64\bin

2. ## 安装Clion配置C++编译器    

   设置——构建、执行、部署——工具链

   ![image-20220311085103567](https://gitee.com/zhangchenyu1023/Pictures/raw/master/img/image-20220311085103567.png)

3. ## 安装Cmake

4. ## [OpenCV源码](https://opencv.org/releases/)

   在Path中暂时移除Anaconda

   #### 使用 CMake 生成 OpenCV 的 Makefile

   打开 cmake-gui，设置源码和生成路径：

   - Where is the source code: `E:/opencv_341/opencv/sources`
   - Where to build the binaries: `E:/opencv_341/opencv_mingw64_build`

   点击 Configure，设置编译器

   - Specify the generator for this project: `MinGW Makefiles`

   - Specify native compilers

   - Next

   - Compilers C: `E:\mingw64\bin\gcc.exe`

   - Compilers C++: `E:\mingw64\bin\g++.exe`

   - Finish

     

   -  [opencv_ffmpeg_64.dll] [opencv_ffmpeg.dll]两个文件复制到E:\Opencv\opencv4.0.1\sources\3rdparty\ffmpeg中

     

   编译配置：

   - 勾选 `WITH_OPENGL`
   - 勾选 `ENABLE_CXX11`
   - 不勾选 `WITH_IPP`
   - 不勾选 `ENABLE_PRECOMPILED_HEADERS`

   点击 Configure，Generate 生成 Makefile

   #### 编译 OpenCV

   打开终端进行编译：（`-j` 是使用 `8` 个线程进行编译，请根据你的计算机配置合理设置线程数）

   ```
   E:cd 
   E:\opencv_341\opencv_mingw64_build
   mingw32-make -j 8
   mingw32-make install
   ```

   为用户变量 Path 添加 E:\Opencv\opencv4.0.1\mingw-build\bin

   #### 编译 OpenCV 常见错误

   #### 1. MinGW-w64 的 aviriff.h 文件注释错误

   表现

   ```
   [ 49%] Building CXX object modules/videoio/CMakeFiles/opencv_videoio.dir/src/cap_dshow.cpp.objIn file included from E:\opencv_341\opencv\sources\modules\videoio\src\cap_dshow.cpp:113:0:e:\mingw-w64\x64-4.8.1-release-posix-seh-rev5\mingw64\x86_64-w64-mingw32\include\aviriff.h:2:8: error: expected constructor, destructor, or type conversion before 'file' * This file is part of the mingw-w64 runtime package.        ^e:\mingw-w64\x64-4.8.1-release-posix-seh-rev5\mingw64\x86_64-w64-mingw32\include\aviriff.h:3:25: error: 'refer' does not name a type * No warranty is given; refer to the file DISCLAIMER within this package.                         ^In file included from e:\mingw-w64\x64-4.8.1-release-posix-seh-rev5\mingw64\x86_64-w64-mingw32\include\aviriff.h:19:0,                 from E:\opencv_341\opencv\sources\modules\videoio\src\cap_dshow.cpp:113:e:\mingw-w64\x64-4.8.1-release-posix-seh-rev5\mingw64\x86_64-w64-mingw32\include\pshpack2.h:7:21: error: expected declaration before end of line #pragma pack(push,2)                     ^modules\videoio\CMakeFiles\opencv_videoio.dir\build.make:146: recipe for target 'modules/videoio/CMakeFiles/opencv_videoio.dir/src/cap_dshow.cpp.obj' failedmingw32-make[2]: *** [modules/videoio/CMakeFiles/opencv_videoio.dir/src/cap_dshow.cpp.obj] Error 1CMakeFiles\Makefile2:3057: recipe for target 'modules/videoio/CMakeFiles/opencv_videoio.dir/all' failedmingw32-make[1]: *** [modules/videoio/CMakeFiles/opencv_videoio.dir/all] Error 2Makefile:161: recipe for target 'all' failedmingw32-make: *** [all] Error 2
   ```

   [![MinGW-w64_aviriff.h_file_annotation_error](http://huihut-img.oss-cn-shenzhen.aliyuncs.com/MinGW-w64_aviriff.h_file_annotation_error.png)](http://huihut-img.oss-cn-shenzhen.aliyuncs.com/MinGW-w64_aviriff.h_file_annotation_error.png)

   解决

   打开`E:\MinGW-w64\x64-4.8.1-release-posix-seh-rev5\mingw64\x86_64-w64-mingw32\include\aviriff.h`

   发现第一行的多行注释少了个`/`符号，加上保存，如下：

   ```
   /*** This file is part of the mingw-w64 runtime package.* No warranty is given; refer to the file DISCLAIMER within this package.*/
   ```

   然后重新 `Configure`-`Generate`-`mingw32-make` 就好了。

5. ## CMakeLists.txt

   ```
   cmake_minimum_required(VERSION 3.20)
   project(OpencvP)
   set(CMAKE_CXX_STANDARD 14)
   add_executable(OpencvP main.cpp)
   
   set(OpenCV_DIR E:\\Opencv\\opencv4.0.1\\mingw-build)
   find_package(OpenCV EQUIRED)
   include_directories(${OpenCV_INCLUDE_DIRS})
   target_link_libraries(OpencvP ${OpenCV_LIBS})
   ```