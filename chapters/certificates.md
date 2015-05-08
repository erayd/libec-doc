# Certificates

Certificates are represented by the `ec_cert_t` type, and are created by `ec_cert_create()`. All certificates must be freed once they are no longer required using `ec_cert_destroy()`.

##ec_cert_create()
`ec_cert_t *ec_cert_create(time_t valid_from, time_t valid_until);`

Create a new certificate, and return a pointer to it. Returns NULL on failure. `valid_from` and `valid_until` define the certificate's validity period - if the validity period does not fall entirely within the valitity period of its signer, then the timestamp which violates this constraint will be adjusted to match that of the signer.

Timestamps are handled as seconds since 1970-01-01 00:00:00. The current time can be obtained using `time()` (see `time.h`).

Certificates created using this function must be destroyed using `ec_cert_destroy()` when they are no longer required.