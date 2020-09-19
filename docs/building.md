# Setup

## Installing MCUT

*Step 1*:
MCUT is hosted on [Github](https://github.com/cutdigital/mcut.git), which you can access upon obtaining a license. We recommend that you clone the project, for easier access to the latest updates. Alternatively, downloading a compressed release package from the project page is also sufficient.

*Step 2*:
Download the official MCUT headers from [here](https://github.com/cutdigital/mcut-headers.git). The referenced URL points to the C language headers for the MCUT API which you will need to build the MCUT library, and your application(s).

## Building MCUT

*Requirements*

* [CMake](https://cmake.org/): to configure the build system. 
* A standard C++11 compiler. 

To compile the code, run the following commands in your terminal:

```sh
cd mcut # change directory into mcut dir
mkdir build && cd build # create and enter directory called 'build'
cmake -DMCUT_INCLUDE_DIR=<path/to/mcut-headers/dir> .. # generate build files
```

The above commands will generate build files for compiling MCUT as a _shared library_ target, which is the default configuration. The CMake flag `-DMCUT_INCLUDE_DIR=` provides the path to the public headers. It must be specified as shown if you are building the library on its own. Alternatively, you may set the variable in the `CMakeLists.txt` file of your application project (see below). 

To build MCUT as a _static library_ target, run CMake with `-DMCUT_BUILD_AS_SHARED_LIB=OFF`.

### Arbitrary-precision numbers

The following CMake flag can also be used to enable compiling with arbitrary-precision numbers:

```cmake
-DMCUT_ARBITRARY_PRECISION_NUMBERS=ON
```

Arbitrary-precision numbers depend on the Multiple Precision Floating-Point Reliable Library ([MPFR](https://www.mpfr.org/)), which you must install--version 4.0.2. 

We provide a brief guide on how to install MPFR here:

#### Mac OS

Download with [homebrew](https://brewinstall.org/Install-mpfr-on-Mac-with-Brew/) (follow instructions on the website). Alternatively, you may use some other preferred method of installation. 

#### Linux ###

MPFR may already be installed on your system. Otherwise, download MPFR with a package manager, or follow the instructions on the MPFR site [here](https://www.mpfr.org/mpfr-current/mpfr.html).

#### Windows

Several pre-built packages and instructions exist online. We recommend that you create your own binaries by e.g. compiling Brian Gladman's [MPIR](https://github.com/BrianGladman/mpir ) and [MPFR](https://github.com/BrianGladman/mpfr). Once compiled, update your PATH environment variable with the relevant paths so that CMake can find the necessary headers (`gmp.h` and `mpfr.h`) and binary files (`mpfr.dll`).

----

*Minor cautions*

1. Enabling arbitrary-precision numbers may restrict the portability of MCUT.      
    - In general, portability constraints are determined by the availability of MPFR on your chosen platform. 
2. Enabling arbitrary-precision numbers will reduce the performance of your program compared to using standard machine-precision numbers.  
    - The magnitude of the effect is dependent on the size of the meshes used. 
    - This is normal and expected behaviour.

### MCUT in your CMake project 

You can add MCUT to an existing project's `CMakeLists.txt` using:

```cmake
...
# Required: this variable must be defined.
# set (MCUT_INCLUDE_DIR "<path/to/mcut-headers/dir>") # Uncomment and set here, or set via command-line.

add_subdirectory(<path/to/mcut>) # mcut directory on your system
target_link_libraries(myApp mcut) # link with the MCUT library target
target_include_directories(myApp PRIVATE ${MCUT_INCLUDE_DIR}) # set include dir of MCUT headers
```
