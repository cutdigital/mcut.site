# Setup

## Installing MCUT

MCUT is hosted on Github, which you can access upon obtaining a license. Using the standard `git clone` command or simply downloading the repository from github is sufficient.

If you wish to build with _arbitrary-precision_ numbers then you must install the Multiple Precision Floating-Point Reliable Library ([MPFR](https://www.mpfr.org/))--version 4.0.2. Be aware that enabling arbitrary-precision numbers may restrict the portability of MCUT on platforms other than Windows, Linux and Mac. 

### Installing MPFR (optional)

The following instructions describe how to install MPFR on three supported platforms if you wish to build with arbitrary-precision numbers.

#### Mac OS

Download with [homebrew](https://brewinstall.org/Install-mpfr-on-Mac-with-Brew/) (follow instructions on the website). Alternatively, you may use some other preferred method of installation. 

#### Linux ###

Download with a package manager or follow the instructions on the MPFR [site](https://www.mpfr.org/mpfr-current/mpfr.html).

#### Windows

TODO: 

## Building MCUT

MCUT is a a self-contained project using [CMake](https://cmake.org/) to configure the build system.

### Compilation

MCUT requires a C++11 enabled compiler to build successively (GCC, Clang, MSVC ...). To compile the code, run the following commands in your terminal:

```sh
cd mcut # change directory into mcut dir
mkdir build && cd build # create and enter directory called 'build'
cmake .. # generate build files
make mcut # compile the library
```

The above commands will generate and compile MCUT as a _shared library_ target, which is the default configuration.
To build MCUT as a _static library_ target, run CMake as follows:

```cmake
cmake -DMCUT_BUILD_AS_SHARED_LIB=OFF ..
```

The following CMake flag can also be used to enable compiling with arbitrary-precision numbers:

```cmake
-DMCUT_ARBITRARY_PRECISION_NUMBERS=ON
```

Be sure to have installed the necesary dependancy as described above

### MCUT in your CMake project 

You can add MCUT to an existing project's `CMakeLists.txt` using:

```cmake
add_subdirectory(<TODO:PATH-TO-MCUT-DIR>) 
target_link_libraries(<TODO:PROJECT-NAME> mcut)
target_include_directories(<TODO:PROJECT-NAME> PRIVATE ${mcut_include_dir})
```
