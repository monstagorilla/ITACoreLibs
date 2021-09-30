# How to build

## Introduction

This tutorial explains how to build (most of) the C++ libraries of the *Institute for Hearing Technology and Acoustics* at *RWTH Aachen University*.

Our projects use CMake as a build system generator. To ease the build process, most external libraries are downloaded during the configuration and build with the main project. This is done using the [CMake Package Manager](https://github.com/cpm-cmake/CPM.cmake).

## Requirements
### General requirements

- CMake Version 3.20.6 or higher
- C++11 compatible compiler (only tested with Visual C++ 19)

In case you are using Visual Studio, you can use the integrated CMake version. See [Windows with Visual Studio](#windows-with-visual-studio) section for further information.

External libraries are automatically downloaded and configured for you!
For exceptions, see [Special requirements](#special-requirements).

### Special requirements

For certain modules, additional external libraries have to be manually included:
- For ITAGeometricalAcoustics/ITAGeo: SketchUp SDK, minimum version 2019
  Cannot be disabled if dependency towards ITAGeo exists
- For pigeon application: Qt, version 5.9.2 - 5.15.2  
  CMake option to disable module: `ITA_GEOMETRICAL_ACOUSTICS_WITH_APPS_QT` Default: `OFF`

These dependencies have to be downloaded separately and made known to CMake. This is done via the CMake variables `SketchUpAPI_DIR` and `Qt5_DIR`. Note, that these variables can also be environment variables to work. For Qt, `Qt5_DIR` should point to `*Qt Version*\*Compiler*\lib\cmake\Qt5`. For SketchUp, `SketchUpAPI_DIR` should point to the `sketchup-yyyy` directory, e.g. `D:/ExternalLibs/SketchUp/sketchup-2021`.

In case you are working at our institute, these libraries can be found on our server.

## Known issues

CMake has a character limit for its build paths. This means, that the path to some source files can be too long. In these cases CMake will issue a warning. If this happens, try to move the top-level source folder up in your directory tree. A safe place should be the main drive folder, e.g. `D:\`.


## General CMake

### Build via CMake GUI

1. Open the CMake GUI
2. Point to the project directory for the source code (e.g. via `Browse Source...`)
3. As build directory, specify a direct subfolder of the project directory. The folder name should start with or being called `build`, e.g.  `D:/Development/ITACoreLibs/build`.
4. Press `Configure` and specify your generator (e.g. `Visual Studio 16 2019`)
5. Press `Generate`
6. Press `Open Project`
7. Done!

You can use the CMake variables to configure the project. For example, it is possible to de-/activate certain modules.

### Build via command line

```bash
mkdir build
cd build
cmake ..
cmake --build
cmake --install
```


## Windows with Visual Studio

Another way to build this project on windows. Does not require a separate installation of CMake. However, CMake configuration can be tricky sometimes.

### Visual Studio version

- Visual Studio 2019, Version 16.10.4 and above; lower versions will also work.  
  The limiting factor is the bundled CMake version, which should match the [general requirements](#general-requirements).  
  Note, that the **C++ CMake Tools for Windows** have to be installed with Visual Studio ([reference](https://docs.microsoft.com/en-us/cpp/build/cmake-projects-in-visual-studio?view=msvc-160)).

### Build

1. Start Visual Studio.
2. Click on `Continue without code`/`Ohne Code fortfahren`.
3. `File`/`Datei` -> `Open`/`Öffnen` -> `CMake...`.
4. Choose the top-level `CMakeLists.txt` file.
5. Wait till the configuration is finished.
6. `Build`/`Erstellen` -> `Build all`/`Alle erstellen` or `Install "PROJECT NAME"`/`"PROJECT NAME" installieren`.
7. And you are done.
   The build results can be found in `*project_folder*/out/build/*build_configuration*/[Debug|Release|...]`
   While the install results can be found in `*project_folder*/out/install/*build_configuration*`



## Linux

For now, see [General CMake](#general-cmake).

## Apple OSX

For now, see [General CMake](#general-cmake).
