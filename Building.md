# Introduction
The libluksde source code can be build with different compilers:
* Using GNU Compiler Collection (GCC)
  * Using Cygwin
* Using Minimalist GNU for Windows (MinGW)
* Using Microsoft Visual Studio

Or directly packaged with different package managers:
* Using Debian package tools (DEB)
* Using RedHat package tools (RPM)
* Using Mac OS X pkgbuild

# Getting the source
## Source package
To retrieve the source package go to the [downloads](https://googledrive.com/host/0B3fBvzttpiiSSzlxcHFxRC0zYTA/) page and download the file named:
```
libluksde-alpha-<version>.tar.gz
```

To extract the source package run:
```
tar xfv libluksde-alpha-<version>.tar.gz
```

This will create the source directory:
```
libluksde-<version>
```

## Git
To retrieve the source from the git repository make sure to install: git, aclocal, autoconf, automake, autopoint, gettextize and libtoolize.

To download and prepare the source for building run:
```
git clone https://github.com/libyal/libluksde.git
cd libluksde/
./synclibs.sh
./autogen.sh
```

**Note that the source from the git repository will not work without synchronizing the library dependencies "./synclibs.sh" and having the autotools generate the necessary files "./autogen.sh".**

# Using GNU Compiler Collection (GCC)

Before you build the libluksde source code using the GNU Compiler Collection (GCC) you'll need to have compilation and build tools installed.
* On a Linux system make sure you have build-essential (Debian-based) or the Development Tools (RedHat-based) packages installed.
* On a Mac OS X system make sure you have XCode (with command line tools) or MacPorts (or equivalent) installed.

Also make sure to have the following dependencies including source headers installed:
* libcrypto (part of OpenSSL) (optional but recommended)

If you want to be able to use luksdemount, make sure that:
* on a Linux system you have libfuse-dev (Debian-based) or fuse-devel (RedHat-based) installed.
* on a Mac OS X system, you have OSXFuse (http://osxfuse.github.com/) installed.

To build the libluksde source code change into the source directory and run the following commands:
```
./configure
make
```

You can install the binaries that were build by running:
```
sudo make install
```

By default this will install the binaries in /usr/local. If you want to change this to e.g. /usr, add the configuration option --prefix=/usr, e.g.
```
./configure --prefix=/usr
```

On Linux make sure libluksde.so is in the library cache. Normally it suffices to run:
```
sudo ldconfig
```

## Verbose and debug output
To troubleshoot issues or for low-level format analysis libluksde supports verbose and debug output.

To enable verbose and debug output support add --enable-verbose-output and --enable-debug-output to configure, e.g.
```
./configure --enable-verbose-output --enable-debug-output
```

This will generate vast amounts of debug information on stderr when the tools are run with -v.

## Static library
To make a static library add --enable-shared=no to configure, .e.g:
```
./configure --enable-shared=no
```

## Static executables
Some distributions provide separate packages for static versions of libraries. Make sure you have a static versions of:
* glibc
* libcrypto (part of OpenSSL) (optional but recommended, can be disabled by --with-openssl=no)
* fuse (optional, can be disabled by --with-libfuse=no)

To make static executables add --enable-static-executables=yes to configure, .e.g:
```
./configure --enable-static-executables=yes
```

## Python-bindings
To compile the Python-bindings you'll need the Python development files.

Make sure that on:
* a Linux machine, you have python-dev (Debian-based) or python-devel (RedHat-based) installed.
* a Mac OS X machine, XCode if you're using a different Python source tree see Mac OS X notes below.

Adding --enable-python when running ./configure will enable the Python-bindings to be build.
```
./configure --enable-python
```

## Cygwin
If you want to use Cygwin to build libluksde make sure to have the following packages installed:
* autoconf
* automake
* binutils
* gcc-core
* gettext
* libiconv
* libtool
* make
* openssl-devel (optional but recommended)


After following the GNU Compiler Collection (GCC) build instructions you should end up with the following DLL:
```
libluksde/.libs/cygluksde-0.dll
```
And the following executables:
```
luksdetools/.libs/luksdeinfo.exe
luksdetools/.libs/luksdemount.exe
```


### Using the DLL
Make sure you use define LIBLUKSDE_DLL_IMPORT before including <libluksde.h>.
* **TODO describe dependencies**

## Mac OS X
### Universal binary
With XCode you can build a Mac OS X universal binary to run on multiple architectures. The supported architectures and exact command differs per version of Mac OS X.

### libtoolize
If you find that libtoolize is missing use glibtoolize instead.

#### Mac OS X 10.4
E.g. on Mac OS X 10.4 to build an PPC and Intel 32-bit multi binary, run the following commands:
```
CFLAGS="-isysroot /Developer/SDKs/MacOSX10.4u.sdk -arch ppc -arch i386" \
LDFLAGS="-Wl,-syslibroot,/Developer/SDKs/MacOSX10.4u.sdk -arch ppc -arch i386" \
./configure --disable-dependency-tracking
make
make install
```

#### Mac OS X 10.6
E.g. on Mac OS X 10.6 to build an Intel 32-bit and 64-bit multi binary, run the following commands:
```
CFLAGS="-isysroot /Developer/SDKs/MacOSX10.6.sdk -arch x86_64 -arch i386" \
LDFLAGS="-Wl,-syslibroot,/Developer/SDKs/MacOSX10.6.sdk -arch x86_64 -arch i386" \
./configure --disable-dependency-tracking
make
make install
```

#### Mac OS X 10.7
E.g. on Mac OS X 10.7 to build an Intel 32-bit and 64-bit multi binary, run the following commands:
```
CFLAGS="-isysroot /Applications/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX10.7.sdk -arch x86_64 -arch i386" \
LDFLAGS="-Wl,-syslibroot,/Applications/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX10.7.sdk -arch x86_64 -arch i386" \
./configure --disable-dependency-tracking
make
make install
```

### Python-bindings
Mac OS X comes with multiple platform specific build Python versions. If you install a different version of Python on Mac OS X make sure that when building and running the Python-bindings you're using the correct version otherwise it will likely result in one of the following errors:
```
Fatal Python error: Interpreter not initialized (version mismatch?)
```

```
Fatal Python error: PyThreadState_Get: no current thread
```

To build against a specific version you can tell configure where to look:
```
CFLAGS=-I/Library/Frameworks/Python.framework/Versions/2.7/include/ \
LDFLAGS=-L/Library/Frameworks/Python.framework/Versions/2.7/lib/ \
./configure --enable-python --with-pyprefix
```

Adding --with-pyprefix tells configure to use Python to determine where to install the Python-bindings.

## Sun Solaris
To build libluksde on Sun Solaris make sure that /usr/ccs/bin and /usr/sfw/bin are defined in the PATH environment variable.

# Using Minimalist GNU for Windows (MinGW)
To compile libluksde using MinGW you'll need:
* MinGW
* Windows Crypto API (libadvapi32) (optional but recommended)


To build use:
```
mingw32-configure --prefix=/opt/local/i386-mingw32 --enable-winapi=yes
mingw32-make
```

It is recommended that you use WINAPI support but it is possible to compile libluksde without it (--enable-winapi=no).
The default behavior is that configure will try to auto-detect MinGW and enable WINAPI support.

If mingw32-configure and mingw32-make are not available you can build it with:
```
./configure --host=i386-mingw32 --prefix=/opt/local/i386-mingw32 --enable-winapi=yes
make
```

If this does not work try a script similar to the following:
```
#!/bin/sh
CC=/opt/local/bin/i386-mingw32-gcc
CXX=/opt/local/bin/i386-mingw32-g++
AR=/opt/local/bin/i386-mingw32-ar
OBJDUMP=/opt/local/bin/i386-mingw32-objdump
RANLIB=/opt/local/bin/i386-mingw32-ranlib
STRIP=/opt/local/bin/i386-mingw32-strip
MINGWFLAGS="-mwin32 -mconsole -march=i586 "
CFLAGS="$MINGWFLAGS"
CXXFLAGS="$MINGWFLAGS"

CC=$CC CXX=$CXX AR=$AR OBJDUMP=$OBJDUMP RANLIB=$RANLIB STRIP=$STRIP ./configure --host=i586-mingw32msvc --prefix=/opt/local/i386-mingw32 --enable-winapi=yes
CC=$CC CXX=$CXX AR=$AR OBJDUMP=$OBJDUMP RANLIB=$RANLIB STRIP=$STRIP CFLAGS="$CFLAGS" CXXFLAGS="$CXXFLAGS" make
```

If you get compiler errors like:
```
#error WINAPI file open function for Windows 2000 or earlier NOT implemented yet
```

That means WINVER is not set or set to a version predating Windows XP (0x0501) and you'll have to set WINVER manually like:
```
CFLAGS=-DWINVER=0x0501 ./configure --host=i386-mingw32 --enable-winapi=yes
```

You should end up with the following DLL:
```
libluksde/.libs/libluksde-1.dll
```

And the following executables:
```
luksdetools/.libs/luksdeinfo.exe
luksdetools/.libs/luksdemount.exe
```


To install libluksde and tools in the MinGW build tree use:
```
sudo make install
```

## Using MSYS-MinGW
MSYS-MinGW provides means to run configure on Windows.

### Installing MSYS-MinGW
Download mingw-get from http://www.mingw.org/ 

Install mingw-get, you'll only need the command line interface.

More information can be found [here](http://www.mingw.org/wiki/InstallationHOWTOforMinGW).

Start a command prompt and change into the MinGW binaries directory:
```
cmd.exe
cd C:\mingw\bin\
```

To install the required MinGW and MSYS packages run:
```
mingw-get install binutils mingw-runtime w32api libgmp libmpc libiconv pthreads gettext libz gcc-core mingw32-make msys
```

### Building with MSYS-MinGW
Start the MSYS shell:
```
C:\MinGW\msys\1.0\msys.bat
```

Make sure the MinGW directory is mounted, otherwise run the following command to mount:
```
mkdir /mingw
mount C:\\MinGW /mingw
```

**Note: make sure to use the the double \\ and that /mingw has no trailing /**

To build use:
```
tar xfv libluksde-alpha-<version>.tar.gz
cd libluksde-<version>/
CPPFLAGS=-DWINVER=0x0501 ./configure --prefix=/mingw
make
```

## Using the DLL
Make sure you use define LIBLUKSDE_DLL_IMPORT before including <libluksde.h>.

* **TODO describe dependencies**

## Troubleshooting
While running make I get an error similar to the following:
```
libclocale_locale.c: In function 'libclocal_local_get_decimal_point':
libclocale_locale.c:357:2: warning implicit declaration of function 'GetLocaleInfoEx' [-Wimplicit-function-declaration]
libclocale_locale.c:358:7: error: 'LOCALE_NAME_USER_DEFAULT' undeclared (first use in this function)
libclocale_locale.c:358:7: note: each undeclared identifier is reported only once for every function it appears in
```

The version of MinGW does not support a WINAPI version of Vista or later (0x0600) try setting WINVER to 0x0501.

# Using Microsoft Visual Studio
Before you build libluksde using Microsoft Visual Studio you'll need to have it installed.
The libluksde packages comes with Microsoft Visual Studio files for version 2008.
Version 2010 is able to convert these files into its newer versions.

The Microsoft Visual Studio express version is sufficient.
Note that if you want to build 64-bit version with the express version you'll need at least 2010.
Also see the section: Microsoft Visual Studio 2010 express and 64-bit compilation.

Note that if you want to build libluksde from source checked out of git with Visual Studio make sure the autotools are able to make a distribution package of libluksde before trying to build it.
You can create distribution package by running: "make dist".

To compile libluksde using Microsoft Visual Studio you'll need:
* Windows Crypto API (libadvapi32) (optional but recommended)

If you want to be able to use luksdemount you'll need Dokan library see the corresponding section below.
Otherwise ignore or remove the dokan_dll and luksdemount Visual Studio project files.


## Verbose and debug output
To troubleshoot issues or for low-level format analysis libluksde supports verbose and debug output.

To enable verbose and debug output support edit:
```
common\config_winapi.h
```

Add the following definitions:
```
#define HAVE_VERBOSE_OUTPUT     1
#define HAVE_DEBUG_OUTPUT       1
```

## Dokan library
Get the Dokan installation from: http://dokan-dev.net/en/ and install the driver.

Get the source.
```
svn checkout http://dokan.googlecode.com/svn/trunk/ dokan
```

The Visual Studio project files expect the source directory to be named dokan and located in the directory containing the libluksde-```<version>``` directory.

Download [dokan.patch](https://googledrive.com/host/0B3fBvzttpiiSZDA2Y3hBeWRUUzg/dokan.patch).

Apply the patch:
```
cd dokan
patch -p1 < ../dokan.patch
```

The Dokan library is now ready to be build from the libluksde Visual Studio solution file and will be named dokan.dll.

## Python-bindings
To compile the pyluksde Python-bindings you'll need the Python development files which are part of the Python.org windows installer package.

The Microsoft Visual Studio files are configured to look in:
```
C:\Python2.7\
```

If you want to have the Python development files in a different location you'll have to change the pyluksde Microsoft Visual Studio settings.

Note that the VSDebug build of the Python-bindings will fail since the Python development files do not come with python2.7_d.lib.

If you do not intend to build the Python-bindings you can safely ignore build errors regarding: pyluksde.

## Building
Open the file:
```
msvscpp\libluksde.sln
```

Note that the project files contain a Release and VSDebug configuration.
The VSDebug builds the binaries with debug information.
Note that this is not the same as debug output.

Make sure to check if your build environment defines the correct WINVER for your build.
The code uses WINAPI version specific functions based on WINVER.
You can define a custom WINVER in the Microsoft Visual Studio C++ project files or in common\config_winapi.h

And build the solution. The build files will be places in:
```
msvscpp\Release\
```

### Using MSBuild
Another way to build libluksde with Visual Studio is to use MSBuild via the command line.
MSBuild can be installed as part of the Microsoft.NET Framework.

First set-up the Visual Studio variables:
```
C:\Program Files\Microsoft Visual Studio 9.0\VC\bin\vcvars32.bat
```

Next run MSBuild:
```
msbuild msvscpp\libluksde.sln /p:Configuration=Release;Platform=Win32
```

## Using the DLL
Make sure you use define LIBLUKSDE_DLL_IMPORT before including <libluksde.h>.

On other systems than the build system you'll also need to install the Visual Studio Redistributable package for the DLL to run.

## 64-bit with Microsoft Visual Studio express
To build a 64-bit version of libluksde with Microsoft Visual Studio
express you'll need at least the 2010 version.

### Microsoft Visual Studio 2010
First make sure to enabling 64-bit compilation support on Microsoft Visual
Studio 2010 express. Since this can be a tedious process, some relevant links:
* http://msdn.microsoft.com/en-us/library/vstudio/9yb4317s(v=vs.100).aspx
* http://support.microsoft.com/kb/2519277

If you have set it up correctly the following should work:

Go to:
```
Configuration manager -> Active solution platform
```

Select "```<New>```"
* Type or select the new platform: "x64"
* Copy settings from: "Win32"
* Create new project platforms: enabled


Additionally for every project change:
```
Configuration Properties -> General -> Platform Toolset
```

Into "Windows7.1SDK"

If you've Cygwin installed on your Visual Studio build machine you can try
running one of the following scripts to add the x64 settings for you:
```
sh msvscpp/scripts/vs2008_x64.sh
```

Make sure to convert the solution first from 2008 to 2010.
```
sh msvscpp/scripts/vs2010_x64.sh
```

# Using Debian package tools (DEB)

To build libluksde using the Debian package tools make sure you have the following packages installed:
```
sudo apt-get install build-essential debhelper fakeroot autotools-dev libssl-dev libfuse-dev python-dev
```

If you downloaded the source using git make sure to run ./configure at least once to generate the dpkg packaging files.

To build the Debian packages change into the source directory and run:
```
cp -rf dpkg debian
dpkg-buildpackage -rfakeroot
```

This will create the following files in the parent directory:
```
libluksde_<version>-1_<arch>.deb
libluksde-dev_<version>-1_<arch>.deb
libluksde-python_<version>-1_<arch>.deb
libluksde-tools_<version>-1_<arch>.deb
```

To install, e.g. the library:
```
sudo dpkg -i libluksde_<version>-1_<arch>.deb
```

# Using RedHat package tools (RPM)
To build libluksde using the RedHat package tools make sure you have the following packages installed:
```
yum install rpm-build openssl-devel fuse-devel python-devel
```

To build:
```
mv libluksde-alpha-<version>.tar.gz libluksde-<version>.tar.gz
rpmbuild -ta libluksde-<version>.tar.gz
```

This will create the following files in the rpmbuild directory:
```
~/rpmbuild/RPMS/<arch>/libluksde-<version>-1.<arch>.rpm
~/rpmbuild/RPMS/<arch>/libluksde-devel-<version>-1.<arch>.rpm
~/rpmbuild/RPMS/<arch>/libluksde-python-<version>-1.<arch>.rpm
~/rpmbuild/RPMS/<arch>/libluksde-tools-<version>-1.<arch>.rpm
~/rpmbuild/SRPMS/libluksde-<version>-1.src.rpm
```

To install, e.g. the library:
```
sudo rpm -ivh libluksde-<version>-1.<arch>.rpm
```

# Mac OS X package
pkgbuild can be used to create a Mac OS X package.

The following instructions show how to build libluksde.pkg and libluksde.dmg from the command line.

First build libluksde with Python-bindings:
```
./configure --prefix=/usr --enable-python --with-pyprefix
make
```

Next install the build files using DESTDIR
```
make install DESTDIR=$PWD/tmp
```

This will make sure that library paths in the dylib file is set correctly for distribution.
This is not the case when you use:
```
./configure --prefix=$PWD/tmp
```

You can check the library paths in the dylib by running:
```
otool -LT tmp/usr/lib/libluksde.1.dylib
```

After running "make install" the binaries are installed in:
```
$PWD/tmp/
```

If you are planning to distribute libluksde.pkg make sure it contains a copy of LGPL license:
```
mkdir -p $PWD/tmp/usr/share/doc/libluksde
cp AUTHORS COPYING NEWS README $PWD/tmp/usr/share/doc/libluksde
```

To create the package (directory):
```
pkgbuild --root $PWD/tmp --identifier com.github.libyal.libluksde --version <version> --ownership recommended ../libluksde-<version>.pkg
```

To create a distributable disk image:
```
hdiutil create ../libluksde-<version>.dmg -srcfolder ../libluksde-<version>.pkg -fs HFS+
```

