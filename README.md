# OpenSubdiv

OpenSubdiv is a set of open source libraries that implement high performance subdivision surface (subdiv) evaluation on massively parallel CPU and GPU architectures. This codepath is optimized for drawing deforming subdivs with static topology at interactive framerates. The resulting limit surface matches Pixar's Renderman to numerical precision.

OpenSubdiv is covered by the Apache license, and is free to use for commercial or non-commercial use. This is the same code that Pixar uses internally for animated film production. Our intent is to encourage high performance accurate subdiv drawing by giving away the "good stuff".

Feel free to use it and let us know what you think.

For more details about OpenSubdiv, see [Pixar Graphics Technologies](http://graphics.pixar.com).

|        |  Linux and OSX |  Windows  |
|:------:|:-------:|:---------:|
| master | [![Linux and OSX Build Status](https://travis-ci.org/PixarAnimationStudios/OpenSubdiv.svg?branch=master)](https://travis-ci.org/PixarAnimationStudios/OpenSubdiv) | [![Windows Build Status](https://ci.appveyor.com/api/projects/status/mcmwg4q9m8kgi9im/branch/master?svg=true)](https://ci.appveyor.com/project/c64kernal/opensubdiv-ddr8q) |
| dev | [![Linux Build Status](https://travis-ci.org/PixarAnimationStudios/OpenSubdiv.svg?branch=dev)](https://travis-ci.org/PixarAnimationStudios/OpenSubdiv) | [![Windows Build Status](https://ci.appveyor.com/api/projects/status/mcmwg4q9m8kgi9im/branch/dev?svg=true)](https://ci.appveyor.com/project/c64kernal/opensubdiv-ddr8q) |

## Documents
 * [User Documents](http://graphics.pixar.com/opensubdiv/docs/intro.html)
 * [Doxygen API Documents](http://graphics.pixar.com/opensubdiv/docs/doxy_html/index.html)
 * [Release Notes](http://graphics.pixar.com/opensubdiv/docs/release_notes.html)

## Forum
 * [OpenSubdiv Google Groups](https://groups.google.com/forum/embed/?place=forum/opensubdiv)

## Prerequisite
  For complete information, please refer OpenSubdiv documents:
  [Building with CMake](http://graphics.pixar.com/opensubdiv/docs/cmake_build.html)

 * General requirements:

| Lib                           | Min Version | Note       |
| ----------------------------- | ----------- | ---------- |
| [CMake](http://www.cmake.org) | 2.8.6       | *Required* |

 * Osd optional requirements:

| Lib                                                                | Min Version | Note                        |
| ------------------------------------------------------------------ | ----------- | ----------------------------|
| [GLEW](http://glew.sourceforge.net)                                | 1.9.0       | GL backend (Win/Linux only) |
| [CUDA](http://developer.nvidia.com/cuda-toolkit)                   | 4.0         | cuda backend                |
| [TBB](https://www.threadingbuildingblocks.org)                     | 4.0         | TBB backend                 |
| [OpenCL](http://www.khronos.org/opencl)                            | 1.1         | CL backend                  |
| [DX11 SDK](http://www.microsoft.com/download/details.aspx?id=6812) |             | DX backend                  |
| [Metal](https://developer.apple.com/metal/)                        | 1.2         | Metal backend               |

 * Requirements for building optional examples:

| Lib                                  | Min Version | Note                              |
| -------------------------------------| ----------- | --------------------------------- |
| [GLFW](http://www.glfw.org)          | 3.0.0       | GL examples                       |
| [Ptex](https://github.com/wdas/ptex) | 2.0         | ptex viewers                      |
| [Zlib](http://www.zlib.net)          |             | (required for Ptex under windows) |

 * Requirements for building documentation:

| Lib                                         |
| ------------------------------------------- |
| [Docutils](http://docutils.sourceforge.net) |
| [Doxygen](http://www.doxygen.org)           |
| [Graphviz](https://graphviz.gitlab.io/)     |


## Build example to run glViewer and other example programs with minimal dependency

### All platforms:

  * Install cmake, GLFW and GLEW (GLEW is not required on OSX)

   make sure GLEW and GLFW install directories configured as follows:

```
   ${GLEW_LOCATION}/include/GL/glew.h
   ${GLEW_LOCATION}/lib/libGLEW.a (linux)
   ${GLEW_LOCATION}/lib/glew32.lib (windows)

   ${GLFW_LOCATION}/include/GLFW/glfw3.h
   ${GLFW_LOCATION}/lib/libglfw3.a (linux)
   ${GLFW_LOCATION}/lib/glfw3.lib (windows)
```

  * Clone OpenSubdiv repository, and create a build directory.
```
   git clone https://github.com/PixarAnimationStudios/OpenSubdiv
   mkdir build
   cd build
```

### Windows (Visual Studio)

  * run cmake:
```
"c:/Program Files (x86)/CMake/bin/cmake.exe" ^
    -G "Visual Studio 12 Win64" ^
    -D NO_PTEX=1 -D NO_DOC=1 ^
    -D NO_OMP=1 -D NO_TBB=1 -D NO_CUDA=1 -D NO_OPENCL=1 -D NO_CLEW=1 ^
    -D "GLEW_LOCATION=*YOUR GLEW INSTALL LOCATION*" ^
    -D "GLFW_LOCATION=*YOUR GLFW INSTALL LOCATION*" ^
    ..
```
  * Open OpenSubdiv.sln in VisualStudio and build.

### Linux

```
cmake -D NO_PTEX=1 -D NO_DOC=1 \
      -D NO_OMP=1 -D NO_TBB=1 -D NO_CUDA=1 -D NO_OPENCL=1 -D NO_CLEW=1 \
      -D GLEW_LOCATION="*YOUR GLEW INSTALL LOCATION*" \
      -D GLFW_LOCATION="*YOUR GLFW INSTALL LOCATION*" \
      ..
make
```

### macOS

```
cmake -G Xcode -D NO_PTEX=1 -D NO_DOC=1 \
      -D NO_OMP=1 -D NO_TBB=1 -D NO_CUDA=1 -D NO_OPENCL=1 -D NO_CLEW=1 \
      -D GLFW_LOCATION="*YOUR GLFW INSTALL LOCATION*" \
      ..
make
```

### iOS

  * Because OpenSubdiv uses a self-built build tool (stringify) as part of the build process, you'll want to build for macOS and build the stringify target

```
SDKROOT=$(xcrun --sdk iphoneos --show-sdk-path) cmake -D NO_PTEX=1 -D NO_DOC=1 \
      -D NO_OMP=1 -D NO_TBB=1 -D NO_CUDA=1 -D NO_OPENCL=1 -D NO_CLEW=1 \
      -D STRINGIFY_LOCATION="*YOUR MACOS BUILD LOCATION*"/bin/stringify \
      -D CMAKE_TOOLCHAIN_FILE=../cmake/iOSToolchain.cmake -G Xcode \
      ..
```

  * This will produce an "OpenSubdiv.xcodeproj" that can be open and the targets 'mtlViewer' and 'mtlPtexViewer' (if NO_PTEX is ommitted and libPtex.a is installed in the iOS SDK) that can be run

### Useful cmake options and environment variables

````
-DCMAKE_BUILD_TYPE=[Debug|Release]

-DCMAKE_INSTALL_PREFIX=[base path to install OpenSubdiv]
-DCMAKE_LIBDIR_BASE=[library directory basename (default: lib)]
-DCMAKE_TOOLCHAIN_FILE=[toolchain file for crossplatform builds]

-DCUDA_TOOLKIT_ROOT_DIR=[path to CUDA Toolkit]
-DPTEX_LOCATION=[path to Ptex]
-DGLEW_LOCATION=[path to GLEW]
-DGLFW_LOCATION=[path to GLFW]
-DSTRINGIFY_LOCATION=[path to stringify utility]

-DNO_LIB=1        // disable the opensubdiv libs build (caveat emptor)
-DNO_EXAMPLES=1   // disable examples build
-DNO_TUTORIALS=1  // disable tutorials build
-DNO_REGRESSION=1 // disable regression tests build
-DNO_PTEX=1       // disable PTex support
-DNO_DOC=1        // disable documentation build
-DNO_OMP=1        // disable OpenMP
-DNO_TBB=1        // disable TBB
-DNO_CUDA=1       // disable CUDA
-DNO_OPENCL=1     // disable OpenCL
-DNO_OPENGL=1     // disable OpenGL
-DNO_CLEW=1       // disable CLEW wrapper library
-DNO_METAL=1      // disable Metal
````
