# simple-binary

This is an example of how to create a simple binary [yotta](http://yottadocs.mbed.com) module for [mbed OS](https://mbed/com).

**NOTE: yotta modules are normally distributed as source code, to maximise compatibility across platforms. Binary modules lose some of these advantages.**

## Structure of This Module

```
   |__ simple-binary
   |    \__ api.h
   |__ source
   |    |__ link-lib.cmake  # custom CMake used to link in the library
   |    \__ some-library.a  # pre-compiled library (ELF file, compiled for ARM v7-m)
   |__ module.json          # module description file
   |__ LICENSE              # license file
   \__ README.md            # this readme
```

## Minimal Requirements

For a binary module to work, it must have at least:

  * the pre-compiled library being distributed, as an ELF archive. This may
    have been produced by compiling a source yotta module, or otherwise.
  * a valid [`module.json`
    file](http://yottadocs.mbed.com/reference/module.html). If your binary
    module has dependencies on other yotta modules they must be listed here.
  * custom CMake build rules that add a link dependency from this module to the
    pre-built library archive.
  * header files in the public header directory defining the public API of the
    library.

In this case, this module packages up a pre-compiled version of the
[simplelog](http://yottadocs.mbed.com/tutorial/tutorial.html) yotta example
module. Because `simplelog` is a public module, the name and headers of this
module are different, but if you are packaging a private source module, you
would normally choose the same module name (and header names/paths) as your
private source version.

## Using This Module

Add this module as a dependency to another module's module.json file, and then 

```C
#include "simple-binary/api.h"

void foo(){
   // can call functions declared in api.h, e.g:
   simpleLogInfo("hello, binary module!\n\r");   
}
```

## Disparate Platform Support
If you require differently compiled versions of your module when being used on
different platforms, you should include all of these, and use logic in your
cmake file to choose which one to link. The [yotta
config](http://yottadocs.mbed.com/reference/config.html) guide explains the
information available to cmake files that can be used to choose which file to
link.
