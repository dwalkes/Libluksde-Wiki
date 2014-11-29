# Troubleshooting issues
Some common tips for troubleshooting issue:

1. Check if the documentation already describes your issue;
2. Check the [issues](https://github.com/libyal/libluksde/issues), to see if you are experiencing a known issue;
3. Make sure to use a recent version. The latest version could have introduced new bugs so try the one before that as well;
4. Report the issue via the [issues](https://github.com/libyal/libluksde/issues) or via email. If you don't report it, it is not likely to get fixed.

## Configure errors
These are errors when running ./configure.

If you want to report one of these please include config.log.

### GNU Compiler Collection (GCC)
```
error: command 'gcc' failed with exit status 1
```

This means the compilation failed. To determine the cause look for lines containing ": error: " in config.log e.g.
```
libclocale_locale.c:358:7: error: 'LOCALE_NAME_USER_DEFAULT' undeclared (first use in this function)
```

## Compiler and linker errors
These are errors when running make.

If you want to report one of these please include the last part of the command line output.

### MSYS-MinGW
```
libtool: link: cannot find the library `/home/keith/staged/mingw32/lib/libiconv. la' or unhandled argument `/home/keith/staged/mingw32/lib/libiconv.la'
```

This is a bug in gettext-18.3.2-1 revert to a previous version, e.g.:
```
mingw-get remove gettext
mingw-get install gettext=18.3.1-1
```

## Run-time errors
### Microsoft Visual Studio
On Windows if an executable built with Visual Studio will not run and shows you an error dialog that a certain function is not implemented:
* check that you've installed the Visual Studio run-time DLLs for the version of Visual Studio;
* check if WINVER is set correctly;
* check if your version of Visual Studio is compatible with your version of Windows, e.g. a Visual Studio 2008 created binary will not run on Windows 2000.

You can use Visual Studio's bindump.exe to determine which versions of Windows your executable has been built for.

Some examples of common run-time errors:
```
The procedure entry point DecodePointer could not be located in the dynamic link library KERNEL32.DLL
```

You are likely trying to run an executable created by Visual Studio 2010 or later on a version of Windows that is no longer supported. 

## Format or behavioral errors
### Verbose and debug output
libluksde comes with a build feature named "verbose and debug output".
This enables the library to output a lot of format debug information when enabled.

Also see: [Building](https://github.com/libyal/libluksde/wiki/Building)

## Crashes
These are errors when running one of the luksdetools.

If you want to report one of these please include the back trace.

### GNU Compiler Collection (GCC)
Compile libluksde with debug symbols:
```
CPPCFLAGS=-g ./configure --enable-shared=no
make
```

Run the crashing tool with a debugger, e.g.:
```
gdb --ex r --args luksdeinfo image.raw
```

Generate a back trace:
```
bt
```

### Microsoft Visual Studio
Build the executables using the VSDebug configuration and run the command via the Visual Studio debugger.

