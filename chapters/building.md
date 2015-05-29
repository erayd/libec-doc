#Build & Install

##Installation
Libec requires the following runtime dependencies:

Dependency|Version
-|-
[libsodium](http://www.libsodium.org)|>=1.0.0
[talloc](https://talloc.samba.org)|>=2.0.0

To install, run the following commands:

```bash
$ git clone https://github.com/erayd/libec.git
$ cd libec
$ ./autogen.sh
$ ./configure
$ make check
# make install
```

###System Requirements
Libec is intended for use on little-endian POSIX systems (e.g. Linux, OSX etc.)

***Libec is not targeted at or tested on big-endian systems.*** While it's possible that libec may build on some big-endian systems, it will probably not function as intended, and at a minimum will be incompatible with certificate or other data generated on a little-endian system.

##Build requirements
###Linux
Any recent Linux system with autotools installed should be able to build libec out of the box, provided libsodium and talloc are installed.

The reference system for development is 64bit Gentoo Linux.

###OSX
In order to build libec on OSX, the following additional built-time dependencies are required:

 * XCode
 * autoconf
 * automake
 * libtool
 * pkg-config
 
autoconf, automake, libtool, pkg-config, libsodium, and talloc are all available via homebrew (`brew install <package>`). If you don't have homebrew installed, please see [brew.sh](http://brew.sh).

If `make` is unable to find header files (particularly on OSX 10.10 Yosemite), it's likely that /usr/include is missing or empty. Run `xcode-select --install` and install the Command Line Developer Tools to resolve this issue.

###Windows
Libec is not currently available on Windows, but may be in the future. Code contributions for a Windows build are welcome.