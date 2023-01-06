# FORK from Go library for Revolution Pi

Refer to the Go doc in the code stored in the folders:

- [gopicontrol package](pkg/gopicontrol): this is a Go port of the piControl C/C++ driver methods wrapped in a Go object. Native syscalls have been used to access the process image in the kernel. These are a one-to-one translate of the piControl.c interface.
- [gopitest application](cmd/gopitest): this is a Go sample application which mimics the functionality of the piTest C application available as a standard command line tool using piControl.

## Go library for Revolution Pi and cross-compilation tools

This project was originally born to port the RevPi piControl C library to the Go language. The initial attempt included porting C structures to Go code and use cgo to keep the RevPi original code base.

For this reason this project besides the Go piControl port contains CMake scripts to cross-compile C/C++ applications for RaspberryPi (RevPi Core).
Readers interested in either topic should refer to the REAMDE files in the specific folders. Below the two areas and more details about this project if you are keen to know its origin.

### How to use the Go package and cross-compile for the RevPi

Install Go on any Linux-based system.

To use the Go package inside your project you just need to refer to the gopicontrol package by using a standard Go import like:

```go
import "github.com/couillonnade/revpi/pkg/gopicontrol"
```

It is normally faster to cross-compile the code on a decent machine and upload it to the RevPi, eg launch:

```go
GOOS=linux GOARCH=arm GOARM=6 go build
```

### How to keep the Go code in sync with the piControl C headers

There is a shell script [generate_godefs.sh](pkg/gopicontrol/generate_godefs.sh) to generate Go structs and constants from the C headers via cgo `-godefs` option.
You just need to keep aligned the file [ctypes_linux.go](pkg/gopicontrol/ctypes_linux.go) with the piControl headers to export what you need.

IMPORTANT: **You should launch the script on the RevPi not on a generic Linux machine.**

## C/C++ cross-compilation utilities

The standard RevPi examples are based on make files, while this is the standard way of building C code CMake provides simplicity and flexibility, particularly if you need to build your own solution and you are interested in cross-compilation.

Refer to [REAMDE in the kunbus folder](kunbus/README.md), in particular to the [buildme](kunbus/buildme) script to see native and cross-compilation options with CMake.
