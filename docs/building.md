# Setup

## Install

MCUT is hosted on Github, which you can access upon obtaining a license. Using the standard `git clone` command or simply downloading the repository from github is sufficient.

If you wish to build with _arbitrary-precision_ numbers then you must install the Multiple Precision Floating-Point Reliable Library ([MPFR](https://www.mpfr.org/)). Version 4.0.2.

### Installing MPFR 

The following is how to install MPFR on three supported platforms.

#### Mac OS

Download with [homebrew](https://brewinstall.org/Install-mpfr-on-Mac-with-Brew/) (follow instructions on the website). Alternatively, you may use some other preferred method of installation. 

#### Linux ###

Download with a package manager or follow the instructions on the MPFR [site](https://www.mpfr.org/mpfr-current/mpfr.html).

#### Windows

TODO: 

## Build

MCUT is a project which relies on [CMake](https://cmake.org/) to configure the build system. The project is distributed as a self-contained library. 

The following instructions assume that MCUT has been downloaded into a suitable directory as described [here](build/download.md)

### Compilation

MCUT requires a C++11 compiler to build successively. To compile the MCUT library, use:

```sh
cd mcut # change directory into mcut dir
mkdir build && cd build # create and enter directory called 'build'
cmake -DCMAKE_BUILD_TYPE=Release .. # generate build files
make mcut -j2 # compile the library
```

The above commands will generate and compile MCUT as a _shared library_ target, which is the default configuration.
To build MCUT as a _static library_ target, use the following CMake flag:

```cmake
-DMCUT_BUILD_AS_SHARED_LIB=OFF
```

The following CMake flag can also be used to enable compiling with arbitrary-precision numbers:

```cmake
-DMCUT_ARBITRARY_PRECISION_NUMBERS=ON
```

Be sure to have installed the necesary dependancy as described [here](build/download.md)

### MCUT in your CMake project 

You can add MCUT to an existing project's `CMakeLists.txt` using:

```cmake
add_subdirectory(<TODO:PATH-TO-MCUT-DIR>) 
target_link_libraries(<TODO:PROJECT-NAME> mcut)
target_include_directories(<TODO:PROJECT-NAME> PRIVATE ${mcut_include_dir})
```
