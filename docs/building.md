# Setup

## Downloading MCUT

MCUT is hosted on Github, which you can access [here](https://github.com/cutdigital/mcut.git). We recommend that you clone the project, for easier access to the latest updates (master branch). Alternatively, you can download a compressed release package from the GitHub repository itself. Be sure to get the most recent version.

## Building MCUT

This section describes how to build the code that you cloned/downloaded from GitHub.

*Requirements*

You need two things to build MCUT:

* [CMake](https://cmake.org/): to configure the build system. 
* A C++11 compiler: to compile/build the code. 

To compile the code (assuming you are using a terminal), execute the following commands:

*Linux/MacOS*
```sh
cd <path/to/mcut> # change directory into mcut dir
mkdir build # create directory called 'build'
cd build # enter build directory
cmake .. # generate build files
make # compile the code
```

*Windows*
```sh
cd <path/to/mcut> # change directory into mcut dir
mkdir build # create directory called 'build'
cd build # enter build directory
cmake .. # generate build files
# Open the generated solution file (.sln) in visual studio
```

You may also use the CMake GUI, which may be easier. The CMake website has information on how you can do that [here](https://cmake.org/runningcmake/).

### MPFR

The following CMake flag can also be used to enable compiling with arbitrary-precision numbers:

```cmake
-DMCUT_BUILD_WITH_ARBITRARY_PRECISION_NUMBERS=ON
```

Arbitrary-precision numbers depend on the Multiple Precision Floating-Point Reliable Library ([MPFR](https://www.mpfr.org/)), which you must install--version 4.0.2. 

We provide a brief guide on how to install MPFR here:

*Mac OS*

Download with [homebrew](https://brewinstall.org/Install-mpfr-on-Mac-with-Brew/) (follow instructions on the website). Alternatively, you may use some other preferred method of installation. 

*Linux*

MPFR may already be installed on your system. Otherwise, download MPFR with a package manager, or follow the instructions on the MPFR site [here](https://www.mpfr.org/mpfr-current/mpfr.html).

*Windows*

Several pre-built packages and instructions exist online. We recommend that you create your own binaries by e.g. compiling Brian Gladman's [MPIR](https://github.com/BrianGladman/mpir ) and [MPFR](https://github.com/BrianGladman/mpfr). Once compiled, update your PATH environment variable with the relevant paths so that CMake can find the necessary headers (`gmp.h` and `mpfr.h`) and binary files (`mpfr.dll`).

_Our pre-compiled Windows binaries for MPIR and MPFR can be found [here](https://github.com/cutdigital/mcut.github.io/blob/master/docs/media/gmp-vs2017.zip)._

----

*Minor cautions*

1. Enabling arbitrary-precision numbers may restrict the portability of MCUT.      
    - In general, portability constraints are determined by the availability of MPFR on your chosen platform. 
2. Enabling arbitrary-precision numbers will reduce the performance of your program compared to using standard machine-precision numbers.  
    - The magnitude of the effect is dependent on the size of the meshes used. 
    - This is normal and expected behaviour.

### MCUT integration 

You can add MCUT to an existing project's `CMakeLists.txt` using:

```cmake
...
add_subdirectory(<path/to/mcut>) # mcut directory on your system
target_link_libraries(myApp mcut) # link with the MCUT library target
target_include_directories(myApp PRIVATE ${MCUT_INCLUDE_DIR}) # set include dir of MCUT headers
```

Skim through `CMakeLists.txt` of MCUT for additional information about additional options that you may specify when configuring CMake.

## Building the Docs

Here we describe how you can configure CMake to generate configuration files that enable building the documentation.

First, you must install [Doxygen](https://www.doxygen.nl/index.html) by following instructions on the respective website.

The following CMake flag can then be used to enable documation generation:

```cmake
-DMCUT_BUILD_THE_DOCS=ON
```

This will enable the creation of a CMake target called ```doc_doxygen```.

When you invoke your build tool, like `make` (e.g. ```make doc_doxygen```), your documentation will be generated as html files. You can view the documentation using your browser by opening the file `build/html/index.html`. 

Feel free to reconfigure Doxygen to generate output in different formats. Refer to the Doxygen [website](https://www.doxygen.nl/index.html) for more information.