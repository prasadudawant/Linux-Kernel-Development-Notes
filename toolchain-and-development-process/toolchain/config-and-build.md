---
description: This page describes how to configure and build the Linux kernel.
---

# Config and Build

## Requirements for Building the Linux Kernel

1. consult the file Documentation/Changes in kernel source(which you want to build) to verify the specific version number you should have of each of tool mentioned below
2. Only three packages that are needed in order to successfully build a kernel: a compiler, a linker, and a make utility
   1.  **Compiler**

       To build the kernel, the gcc C compiler must be used. If you wish to download the compiler and build it yourself, you can find it at [http://gcc.gnu.org](http://gcc.gnu.org/) ( it’s interesting to know which compiler is used to compile the compiler). Note that getting the most recent gcc version is not always a good idea. Some of the newest gcc releases don’t build the kernel properly. So use what is mentioned in Documentation.

       To determine which version of gcc you have on your system, run the following command:\
       `gcc --version`
   2.  **Linker**

       Additional set of tools known as binutils needed to do the linking and assembling of source files. The binutils package also contains useful utilities that can manipulate object files in lots of useful ways, such as to view the contents of a library.

       If you wish to download and install the package yourself, you can find it at [http://www.gnu.org/software/binutils](http://www.gnu.org/software/binutils). \
       To determine which version of binutils you have on your system, run the following command:\
       `ld -v`
   3.  **make**

       make is a tool that walks the kernel source tree to determine which files need to be compiled, and then calls the compiler and other build tools to do the work in building the kernel. The kernel requires the GNU version of make, which can usually be found in a package called make for your distribution. If you wish to download and install make youself, you can find it at [http://www.gnu.org/software/make](http://www.gnu.org/software/make). It is recommended that you install the latest stable version of make, because newer versions are known to work faster at processing the build files. \
       To determine which version of make you have on your system, run the following command: \
       `make --version`

## Configure Your Kernel

## Build Your Kernel
