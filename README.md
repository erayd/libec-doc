#libec

Libec is a small PKI library leveraging the crypto routines available in [libsodium](http://www.libsodium.org/).

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

##Build requirements
###OSX
In order to build libec on OSX, the following additional built-time dependencies are required:

 * XCode
 * autoconf
 * automake
 * libtool
 * pkg-config
 
autoconf, automake, libtool, pkg-config, libsodium, and talloc are all available via homebrew (`brew install <package>`). If you don't have homebrew installed, please see [brew.sh](http://brew.sh).

If `make` is unable to find header files (particularly on OSX 10.10 Yosemite), it's likely that /usr/include is missing or empty. Run `xcode-select --install` and install the Command Line Developer Tools to resolve this issue.

##Support
For development support, questions etc. please use the [mailing list](https://groups.google.com/a/erayd.net/forum/#!forum/libec).

This documentation may also be downloaded as an ebook or PDF [here](http://manual.libec.erayd.net/).

##Licence
Libec is available under the [ISC Licence](http://en.wikipedia.org/wiki/ISC_license). Please see the LICENCE file in the root of the project for more information.