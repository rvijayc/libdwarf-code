#USING MSYS2 (WINDOWS) CMAKE, MESON, CONFIGURE

Created 6 October 2023

msys2 provides an environment much like posix/unix/linux
with programs precompiled for use on windows
installed in its own set of locations.

Do not use the -DWALL option to cmake or
--enable-wall to configure or the meson
equivalent as it causes a minor warning
about <b>I64u , from gcc
(treated as an error).

When using configure note that building a static
library libdwarf.a is not supported. A static
libdwarf.a can be built with meson or cmake.

### Shared library builds
All libdwarf builds are automatically shared object (dll)
builds as of 0.8.0.

With

    --default-library static

on the meson command line
one can build libdwarf as an archive and dwarfdump and the
programs built will use the static library.

The default is shared and can be explicitly
chosen by:

    --default-library shared

Has the same meson setup reporting as on Linux (above).

configure will only allow generation of shared library
builds, while for cmake and meson one can choose
wither to build a shared library or an archive library
(libdwarf.a).


### NOTE on linking against libdwarf.a

If you are are linking code against a static 
library libdwarf.a you must arrange to  define the
macro LIBDWARF_STATIC in compiling your code that
does a #include "libdwarf.h".

To pass LIBDWARF_STATIC to the preprocessor with Visual Studio:

    right click on a project name
    In the contextual menu, click on Properties at the very bottom
    In the new window, double click on C/C++
    On the right, click on Preprocessor definitions
    The is a small down arrow on the right, click on it, then click on Modify
    Add LIBDWARF_STATIC to the values
    Click on OK to close the windows


## Setting up msys2 on Windows

We suggest you use meson for  msys2 builds.

Direct your browser to msys2.org

Download a recent .exe from the downloads page.
For example msys2-x86_64-20230718.exe
Execute it and follow the instructions on msys2.org

the following should get you sufficent files for
building and testing all the build mechanisms:

    basics
    pacman -Suy
    pacman -S base-devel git autoconf automake libtool
    pacnam -S mingw-w64-x86_64-python3 
    pacman -S mingw-w64-x86_64-toolchain
    pacman -S mingw-w64-x86_64-zlib 
    pacman -S mingw-w64-x86_64-zstd 
    pacman -S mingw-w64-x86_64-doxygen

    extras for meson/cmake
    pacman -S mingw-w64-x86_64-meson
    pacman -S mingw-w64-x86_64-cmake
    pacnam -S mingw-w64-x86_64-python3-pip

    To create a distribution
    pacman -S mingw-w64-x86_64-xz

    to list packages
    pacman -Q 
    to remove packages
    pacman -R  <packagename>

## cmake specific
cmake will generate ninja makefiles by default, add
'-G "Unix Makefiles"' to the cmake command line to
generate makefiles for gnu make, but we suggest you
use "-G Ninja" for speed and clarity..

Use
-DBUILD_SHARED:BOOL=TRUE  \
-DBUILD_NON_SHARED:BOOL=FALSE
on the cmake command
to be consistent with normal Windows use.

To get a usable set of executables
set a prefix (for cmake,
-DCMAKE_INSTALL_PREFIX=/c/msys64/usr),
presuming  the bin directory
is something in your $PATH in msys2.
Set an appropriate prefix whichever
build tool you use.

  ninja install
  cp src/bin/dwarfdump/dwarfdump.conf to $HOME
  # then
  dwarfdump.exe
  # which will give a short message  about
  # No object file provided. In which case 
  # dwarfdump is usable.
