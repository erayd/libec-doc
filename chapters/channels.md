# Channels

Libec uses channels to implement secure communication between two entities. Data is encrypted and protected with a MAC.

##ec_channel_init()
`ec_err_t ec_channel_init(ec_channel_t *ch, ec_cert_t *c, ec_ctx_t *ctx, unsigned char *dh);`

Initialise a channel and generate a signed Diffie-Hellman packet to pass to the remote endpoint. Returns zero on success, or a nonzero error code otherwise.

`ctx` should point to a context where the remote certificate can be found, and will be used for validation purposes - if trust chain checks are performed, then those certificates will also need to be available.

`c` is the certificate used as the local endpoint. This certificate does *not* need to be available in `ctx`.

Data to be passed to the remote endpoint (the Diffie-Hellman packet) will be stored in `dh`, which should be a buffer of at least `EC_CHANNEL_DH_BYTES` bytes. The contents of this buffer should be provided to `ec_channel_start()` on the remote end.

```c
#include <ec.h>
...
ec_channel_t *ch;
unsigned char dh[EC_CHANNEL_DH_BYTES];
if(ec_channel_init(ch, c, ctx, dh) != 0) {
    //init failed
}
...
```



**This page is still a work in progress, but should be finished within the next few hours. Please check back later.*