# Channels

Libec uses channels to implement secure communication between two entities. Data is encrypted and protected with a MAC.

##ec_channel_init()
`ec_err_t ec_channel_init(ec_ctx_t *ctx, ec_channel_t *ch, ec_cert_t *c, unsigned char *dh);`

Initialise a channel and generate a signed Diffie-Hellman packet to pass to the remote endpoint.

Data to be passed to the remote endpoint (the Diffie-Hellman packet) is stored in `dh`, which should be a buffer of at least `EC_CHANNEL_DH_BYTES` bytes. The contents of this buffer should be provided to `ec_channel_start()` on the remote end.

**This page is still a work in progress, but should be finished within the next 24 hours. Please check back later.*