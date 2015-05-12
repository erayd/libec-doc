#libec

Libec is a small PKI library leveraging [libsodium](https://github.com/jedisct1/libsodium).

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

##Support
For development support, questions etc. please use the [mailing list](https://groups.google.com/a/erayd.net/forum/#!forum/libec).

This documentation may also be downloaded as an ebook or PDF [here](http://download.libec.erayd.net/).

##Licence
Libec is available under the [ISC Licence](http://en.wikipedia.org/wiki/ISC_license). Please see the LICENCE file in the root of the project for more information.