# Building ScanTailor Advanced and its dependencies
=================================================

ScanTailor Advanced: https://github.com/ScanTailor-Advanced/scantailor-advanced

This is an instruction for building ScanTailor Advanced and its dependencies for **Windows** and **Linux**. 

## <u>Contents</u>:
* [ScanTailor building options](#scantailor-building-options)
* [Build instructions for Qt 5](#build-instructions-for-qt-5)
  * [Dependencies](#dependencies)
  * [Tools](#tools)
  * [**Windows**](#windows)
    * [Preparing to building](#windows-preparing-to-build)
    * [Building with MinGW](#windows-building-with-mingw)
      * [Tools](#windows-mingw-tools)
      * [Preparing tools](#windows-mingw-preparing-tools)
      * [Building dependencies](#windows-mingw-building-dependencies)
      * [Building ScanTailor](#windows-mingw-building-scantailor)
    * [Building with MSVC](#windows-building-with-msvc)
      * [Tools](#windows-msvc-tools)
      * [Preparing tools](#windows-msvc-preparing-tools)
      * [Building dependencies](#windows-msvc-building-dependencies)
      * [Building ScanTailor](#windows-msvc-building-scantailor)
  * [**Linux**](#linux)
    * [Getting dependencies](#linux-getting-dependencies)
    * [Building ScanTailor](#linux-building-scantailor)
  * [**macOS**](#macos)
 
* [Build instructions for Qt 6](#build-instructions-for-qt-6)
  * [Dependencies](#qt6-dependencies)
  * [Tools](#qt6-tools)
  * [**Windows**](#qt6-windows)
    * [Using libraries form Strawberry Perl (MinGW only)](#qt6-strawberry)
    * [Setting up the toolchain CLI](#qt6-cli-setup)
    * [Building Boost](#qt6-boost)
    * [Building other libraries](#qt6-other-libs)
    * [Building Scantailor](#qt6-scantailor)
  * [**Linux**](#qt6-linux)
* [Packaging](#packaging)

## ScanTailor building options
-----
Usage: `cmake -D <option>=<value> ...`

Useful options:  
* `CMAKE_INSTALL_PREFIX=<path>` - install ScanTailor Advanced into the `path` after building.  
* `PORTABLE_VERSION=[ON|OFF]` - whether to build portable version. If enabled, ScanTailor tries to store its settings 
    and files inside the application folder if possible, else ScanTailor stores those in the system specific paths. 
    If disabled, the settings and files are always stored in the system specific paths.  

## Build instructions for Qt 5
----------

### Dependencies
1. [Boost](http://www.boost.org/) (>= 1.60)
2. [libpng](https://sourceforge.net/projects/libpng/files/)
3. [zlib](https://sourceforge.net/projects/libpng/files/zlib/)
4. [jpeg](http://www.ijg.org/files/)
5. [libtiff](http://www.simplesystems.org/libtiff/) (<= 4.2)
6. [Qt](https://download.qt.io/archive/qt/) (>= 5.6)

### Tools
1. [CMake](https://cmake.org/download/) (<= 3.19.8)

### Windows
Supported toolchains for Windows are MinGW and MSVC.

#### <a name="windows-preparing-to-build"></a> <u>Preparing to build</u>
1. Download the sources of [ScanTailor Advanced](https://github.com/ScanTailor-Advanced/scantailor-advanced/releases/)
2. Clone [this project](https://github.com/ScanTailor-Advanced/scantailor-libs-build). (Push the 'Clone or download' button above)
3. Unpack ScanTailor Advanced sources into a folder
4. Create the empty folder named `libs` near the ScanTailor sources folder (but not inside)
5. Download and unpack the sources of all [the dependencies](#dependencies) listed into the libs folder
6. Unpack the folder of this project there.  
   You should have gotten a folder structure similar to this:

   ~~~~
   ..\
    |
    |-- libs
    |   |-- boost_1_xx_x
    |   |-- jpeg-9
    |   |-- libpng-1.x.xx
    |   |-- scantailor-libs-build-master
    |   |-- jpeg-9x
    |   |-- qt-everywhere-opensource-src-5.x.x
    |   |-- tiff-4.2.x
    |   |-- zlib-1.x.x
    |   
    |-- scantailor-advanced-x.x.x
    |
   ~~~~

7. Install [CMake](#tools)

#### <a name="windows-building-with-mingw"></a> <u>Building with MinGW</u>
#### <a name="windows-mingw-tools"></a> Tools
1. [MinGW-w64](https://sourceforge.net/projects/mingw-w64/files/Toolchains%20targetting%20Win64/Personal%20Builds/mingw-builds/)
   (for 32 and 64 bit Windows; `posix` and `dwarf` / `seh` versions recommended)
2. [MSYS2](http://www.msys2.org/)

#### <a name="windows-mingw-preparing-tools"></a> Preparing tools
1. Install MinGW64 into the path `C:\mingw64` / `C:\mingw32`
2. Install MSYS2 into the path `C:\msys64` / `C:\msys32`
3. Run script `configure_msys.bat` with administrative permissions
4. Add MinGW into the `PATH` environment variable:  
   1. Go to: My Computer -> Right click -> Properties -> Advanced system settings  
   2. Click Environment variables  
   3. Find `Path` variable in the system variables tab and paste
      `;C:\mingw64\bin` / `;C:\mingw32\bin` at the end of its value

#### <a name="windows-mingw-building-dependencies"></a> Building dependencies 
1. Run script `configure_libs_x32.bat` / `configure_libs_x64.bat`
2. Open the command prompt (`cmd`), navigate to the `scantailor-libs-build`
   directory (`cd /d "<dir>"`) and enter:

   ~~~~
   mkdir build & cd build
   cmake -G "MinGW Makefiles" --build ..
   mingw32-make -j %NUMBER_OF_PROCESSORS%
   ~~~~

>***Note**: when building successfully done the `qt-everywhere-opensource-src-5.x.x` and `boost_1_xx_x` folders can be removed to free up disk space.*

#### <a name="windows-mingw-building-scantailor"></a> Building ScanTailor
1. Open the command prompt (`cmd`), navigate to the `scantailor-advanced-x.x.x`
   directory (`cd /d "<dir>"`) and enter:

   ~~~~
   mkdir build & cd build
   cmake -G "MinGW Makefiles" --build ..
   mingw32-make -j %NUMBER_OF_PROCESSORS%
   ~~~~

#### <a name="windows-building-with-msvc"></a> <u>Building with MSVC</u>
#### <a name="windows-msvc-tools"></a> Tools
1. [Visual Studio](https://www.visualstudio.com/) (version 2017 or higher \[with the Win XP compability package\] required)
2. [Jom](http://wiki.qt.io/Jom)

#### <a name="windows-msvc-preparing-tools"></a> Preparing tools
1. Place `jom.exe` into the `%ProgramFiles(x86)%\Jom\` folder
2. Add Jom into the `PATH` environment variable:  
   1. go to: My Computer -> Right click -> Properties -> Advanced system settings  
   2. click Environment variables  
   3. Find `Path` variable in the system variables tab and paste
      `;%ProgramFiles(x86)%\Jom\` at the end of its value

#### <a name="windows-msvc-building-dependencies"></a> Building dependencies
1. Open the Native Tools Command Prompt for VS, navigate to the `scantailor-libs-build-master`
   directory (`cd /d "<dir>"`) and enter:

   ~~~~
   mkdir build & cd build
   cmake -G "NMake Makefiles JOM" -D CMAKE_BUILD_TYPE=Release --build ..
   jom -j %NUMBER_OF_PROCESSORS%
   ~~~~

>***Note**: when building successfully done the `qt-everywhere-opensource-src-5.x.x` and `boost_1_xx_x` folders can be removed to free up disk space.*

#### <a name="windows-msvc-building-scantailor"></a> Building ScanTailor
1. Open the Native Tools Command Prompt for VS, navigate to the `scantailor-advanced-x.x.x`
   directory (`cd /d "<dir>"`) and enter:

   ~~~~
   mkdir build & cd build
   cmake -G "NMake Makefiles JOM" -D CMAKE_BUILD_TYPE=Release --build ..
   jom -j %NUMBER_OF_PROCESSORS%
   ~~~~

### Linux
#### <a name="linux-getting-dependencies"></a> Getting dependencies

1. Download [CMake](#tools) and the [dependencies](#dependencies) from the repositories.

   Examples:  
   <details><summary><i>Debian/Ubuntu</i></summary>

   ```
   sudo apt install gcc g++ cmake libjpeg-dev libpng-dev libtiff5 libtiff5-dev libboost-test-dev qtbase5-dev libqt5svg5-dev qttools5-dev qttools5-dev-tools libqt5opengl5-dev libpthread-stubs0-dev
   ```

   </details>

   <details><summary><i>OpenSUSE</i></summary>

   ```
   sudo zypper install gcc7 gcc7-c++ cmake libjpeg8-devel libpng16-devel libtiff5 libtiff-devel libboost_test1_66_0 libboost_test1_66_0-devel libqt5-qtbase-devel libqt5-qtsvg-devel libqt5-qttools-devel pthread-stubs-devel
   ```

   </details>

   If you are using another Linux distribution, refer to its documentation and the [dependencies](#dependencies) section to install the needed packages.

#### <a name="linux-building-scantailor"></a> Building ScanTailor
1. Navigate to the `scantailor-advanced-x.x.x` directory (`cd "<dir>"`) and enter:

   ~~~~
   mkdir build
   cd build
   cmake ..
   cmake --build .
   ~~~~

2. Optionally, install ScanTailor Advanced with:

   ~~~~
   sudo make install
   ~~~~

   >***Note***: you can use `sudo make uninstall` from the build dir to uninstall ScanTailor Advanced later.

### macOS
Use this repository: https://github.com/yb85/scantailor-advanced-osx

Build instructions for Qt 6
----------

It is possible to build scantailor-advanced with Qt6. However, scantailor-libs-build does not include tools for this yet.
The following instructions assume a precompiled version of Qt.

Please note that Qt 6 doesn't support Windows 7 or older.

<h3 id="qt6-dependencies">Dependencies</h3>

1. [Boost](http://www.boost.org/) (>= 1.60)
2. [libpng](https://sourceforge.net/projects/libpng/files/)
3. [zlib](https://sourceforge.net/projects/libpng/files/zlib/)
4. [jpeg](http://www.ijg.org/files/) or [libjpeg-turbo](https://sourceforge.net/projects/libjpeg-turbo/files/)
5. [libtiff](http://www.simplesystems.org/libtiff/)
6. [Qt](https://download.qt.io/official_releases/online_installers/) (>= 6, in binary form), with the corresponding MinGW toolchain if one plans to use MinGW. The toolchain can be installed alongside Qt in the `Tools` section.
7. If using libjpeg-turbo under Windows, [NASM](https://nasm.us/). This is not strictly required but enables to produce faster code.

<h3 id="qt6-tools">Tools</h3>

1. [CMake](https://cmake.org/download/) (>= 3.9)

<h3 id="qt6-windows">Windows</h3>

Supported toolchains for Windows are MinGW and MSVC. A precompiled version of Qt must be installed, with a compiler matching the selected toolchain. In the MinGW case, it is advised to use the MinGW GCC compiler shipped with Qt.

<h4 id="qt6-strawberry">Using libraries form Strawberry Perl (MinGW only)</h4>

When using the MinGW toolchain, `libpng`, `zlib`, `libjeg` and `libtiff` can be obtained in compiled form from an installation of [Strawberry Perl](https://strawberryperl.com/). If Strawberry Perl binaries are in your PATH, CMake will be able to locate the libraries with no additional effort.

<h4 id="qt6-cli-setup">Setting up the toolchain CLI</h4>

In a CMD command prompt, set up the following variables:
  * set `QT_BASE` to the base directory of installed Qt versions. e.g. `set QT_BASE=C:\Qt`.
  * set `QT_DIR` to the Qt binary tree you wish to use. For instance, `set QT_DIR=%QT_BASE%\6.2.3\mingw_64` if using QT 6.2.3 with the MinGW toolchain; or `set QT_DIR=%QT_BASE%\6.2.3\msvc2019_64` for the MSVC version.

Set up the toolchain:
  * for MSVC, locate the `vcvars64.bat` script relevant to your installation and run it: e.g. run `"C:\Program Files\Microsoft Visual Studio\2022\Community\VC\Auxiliary\Build\vcvars64.bat"`
  * for MinGW, add the MinGW binary directory to your PATH:  e.g. `set PATH=%QT_BASE%\Tools\mingw900_64\bin;%PATH%`

Ensure that the `Ninja` build tool is also in your `PATH`. It is provided with Qt, and can be added to the `PATH` by `set PATH=%QT_BASE%\Tools\Ninja;%PATH%`.

<h4 id="qt6-boost">Building Boost</h4>

set `BOOST_SRC_DIR` to the location Boost sources; e.g. `set BOOST_SRC_DIR=C:\boost_1_78_0`

Depending on the toochain selected, build Boost with MSVC,
  ~~~~
cd %BOOST_SRC_DIR%
boostrap.bat msvc
b2 toolset=msvc
  ~~~~

or build boost with MinGW gcc:
  ~~~~
cd %BOOST_SRC_DIR%
boostrap.bat gcc
b2 toolset=gcc
  ~~~~

<h4 id="qt6-other-libs">Building other libraries</h4>

This works both with MinGW and MSVC, but can be avoided using libraries of Strawberry Perl in the MinGW case (see above)

  * Set variables defining `zlib`, `libtiff`, `libjpeg`, `libpng` source directories:
  ~~~~~~
set ZLIB_SRC_DIR=...
set LIBTIFF_SRC_DIR=...
set LIBJPEG_SRC_DIR=...
set LIBPNG_SRC_DIR=...
  ~~~~~~

  * Set up a temporary directory `TMP_DIR` where libraries will be built: e.g. `TMP_DIR=%TEMP%`
  * Set up a directory where libraries will be installed for use by the `scantailor-advanced` build process: e.g. `LIB_DIR=C:\lib_install`.

  * Build and install `zlib`
  ~~~~~~
cmake -GNinja -S "%ZLIB_SRC_DIR%" -B "%TMP_DIR%\build_zlib_release" -D CMAKE_INSTALL_PREFIX="%LIB_DIR%" -D CMAKE_BUILD_TYPE=Release
cmake --build "%TMP_DIR%\build_zlib_release" 
cmake --install "%TMP_DIR%\build_zlib_release"
  ~~~~~~

  * Build and install `libpng`
  ~~~~~~
cmake -GNinja -S "%LIBPNG_SRC_DIR%" -B "%TMP_DIR%\build_libpng_release" -D CMAKE_PREFIX_PATH="%LIB_DIR%" -D CMAKE_BUILD_TYPE=Release
cmake --build "%TMP_DIR%\build_libpng_release" 
cmake --install "%TMP_DIR%\build_libpng_release" --prefix "%LIB_DIR%"
  ~~~~~~

  * Build and install `libtiff`
  ~~~~~~
cmake -GNinja -S "%LIBTIFF_SRC_DIR%" -B "%TMP_DIR%\build_libtiff_release" -D CMAKE_PREFIX_PATH="%LIB_DIR%"  -D CMAKE_INSTALL_PREFIX="%LIB_DIR%" -D CMAKE_BUILD_TYPE=Release
cmake --build "%TMP_DIR%\build_libtiff_release" 
cmake --install "%TMP_DIR%\build_libtiff_release" 
  ~~~~~~

  * Build and install `libjpeg` or `libjpeg-turbo`. Here commands are given for `libjpeg-turbo`, which has a `CMakeFile.txt`. `libjpeg` build instructions can be found as part of the scantailor Qt5 build instructions above.
  ~~~~~~
cmake -GNinja -S "%LIBJPEG_SRC_DIR%" -B "%TMP_DIR%\build_libjpeg_release" -D CMAKE_PREFIX_PATH="%LIB_DIR%" -D CMAKE_BUILD_TYPE=Release
cmake --build "%TMP_DIR%\build_libjpeg_release"
cmake --install "%TMP_DIR%\build_libjpeg_release" --prefix="%LIB_DIR%"
  ~~~~~~

<h4 id="qt6-scantailor">Building Scantailor</h4>

  Set `SCANTAILOR_SOURCE_DIR` and `SCANTAILOR_INSTALL_DIR` to the source and installation directory for scantailor, respectively:
  ~~~~~~
set SCANTAILOR_SOURCE_DIR=...
set SCANTAILOR_INSTALL_DIR=...
  ~~~~~~
  Perform the build with:
  ~~~~~~
    cmake -GNinja -S "%SCANTAILOR_SOURCE_DIR%" -B "%TMP_DIR%\build_scantailor-advanced_release" -DCMAKE_PREFIX_PATH="%QT_DIR%";"%BOOST_SRC_DIR%";"%LIB_DIR%" -DCMAKE_BUILD_TYPE=Release
    cmake --build "%TMP_DIR%\build_scantailor-advanced_release"
    cmake --install "%TMP_DIR%\build_scantailor-advanced_release" --prefix "%SCANTAILOR_INSTALL_DIR%"
  ~~~~~~

  * All directories used can be removed, except `SCANTAILOR_INSTALL_DIR`.

<h3 id="qt6-linux">Linux</h3>

The installation of `zlib`, `libtiff`, `libjpeg`, `libpng` and `Boost` are performed as in the Qt5 case, either from your distribution packages or from source. If a version of Qt6 is shipped with your distribution, it is enough to install it (both the binary and development packages) and perform the build with

  ~~~~~~
    cmake -GNinja -S "$SCANTAILOR_SOURCE_DIR" -B "$TMP_DIR\build_scantailor-advanced_release" -DCMAKE_BUILD_TYPE=Release
    cmake --build "$TMP_DIR\build_scantailor-advanced_release"
    cmake --install "$TMP_DIR\build_scantailor-advanced_release" --prefix "$SCANTAILOR_INSTALL_DIR"
  ~~~~~~

  With `SCANTAILOR_SOURCE_DIR` and `SCANTAILOR_INSTALL_DIR` set to the source and installation directory for scantailor, respectively; and `TMP_DIR` set to some working directory.

  If the compiled version of Qt6 that one wishes to use sits in a non-standard location, it should be given to CMake as in the example below: 

  ~~~~~~
    QT_DIR=/mnt/data/Qt/6.2.3/gcc_64
    cmake -GNinja -S "$SCANTAILOR_SOURCE_DIR" -B "$TMP_DIR\build_scantailor-advanced_release" -DCMAKE_PREFIX_PATH="$QT_DIR" -DCMAKE_BUILD_TYPE=Release
  ~~~~~~

  The build and install steps are unchanged.


Packaging
---------
1. Build ScanTailor Advanced following the instructions above  
2. Use `cpack -G <generator> [-D <var>=<value> ...]` to create the package for your platform.  

   Enter `cpack --help` to see the options and generators available.  
   See the list of available variables for each generator [here](https://cmake.org/cmake/help/latest/manual/cpack-generators.7.html).

   Examples:
     1. To create a Linux DEB package use: `cpack -G "DEB"`  
     2. To create a Linux RPM package use: `cpack -G "RPM"`   
          *Note:* `rpm-build` package has to be installed. 
     3. To create an installer for Windows use: `cpack -G "NSIS"`   
          *Note:* [NSIS](https://sourceforge.net/projects/nsis/files/) has to be installed.
