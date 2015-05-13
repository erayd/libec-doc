# Channels

Libec uses channels to implement secure communication between two entities. Data is encrypted and protected with a MAC.

##ec_channel_init()
`ec_err_t ec_channel_init(ec_ctx_t *ctx, ec_channel_t *ch, ec_cert_t *c, unsigned char *dh);`

Initialise a channel and generate a signed diffie-hellman pair to pass to the remote endpoint.

`dh` should be a buffer of at least `EC_CHANNEL_DH_BYTES` bytes.