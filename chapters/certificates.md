# Certificates

Certificates are represented by the `ec_cert_t` type, and are created by `ec_cert_create()`. All certificates must be freed once they are no longer required using `ec_cert_destroy()`.

If a certificate has been saved using `ec_ctx_save()`, then it will be destroyed automatically once it is removed, overwritten, or the context it was saved to is destroyed.

##ec_cert_create()
`ec_cert_t *ec_cert_create(time_t valid_from, time_t valid_until);`

Create a new certificate, and return a pointer to it. Returns NULL on failure. `valid_from` and `valid_until` define the certificate's validity period - if the validity period does not fall entirely within the valitity period of its signer, then the timestamp which violates this constraint will be adjusted to match that of the signer.

Timestamps are handled as seconds since 1970-01-01 00:00:00. The current time can be obtained using `time()` (see `time.h`).

Certificates created using this function must be destroyed using `ec_cert_destroy()` when they are no longer required.

```c
#include <ec.h>
#include <time.h>
...
time_t from = time(NULL);
time_t until = from + (86400 * 365); //one year
ec_cert_t *c = ec_cert_create(from, until);
...
```

##ec_cert_destroy()
`void ec_cert_destroy(ec_cert_t *c);`

Destroy a certificate previously created with `ec_cert_create()`.

```c
#include <ec.h>
...
ec_cert_destroy(c);
...
```
##ec_cert_sign()
`ec_err_t ec_cert_sign(ec_cert_t *c, ec_cert_t *signer);`

Sign a certificate using another certificate. Returns a nonzero error code on failure.

The signer must have a valid secret key available, and both certificates must pass the following checks prior to signing:

```c
ec_cert_check(NULL, c, EC_CHECK_CERT);
ec_cert_check(NULL, signer, EC_CHECK_CERT | EC_CHECK_SECRET);
```
If the certificate's validity falls outside that of the signer, the appropriate timestamp on the certificate will be adjusted to match that of the signer.

```c
#include <ec.h>
...
if(ec_cert_sign(c, signer) != 0) {
    //signing succeeded
}
...
```

##ec_cert_check()
`ec_err_t ec_cert_check(ec_ctx_t *ctx, ec_cert_t *c, int flags);`
Ensure that a certificate passes a given set of tests. Returns a nonzero error code on failure.

All tests which require a valid trust chain to be present must also provide a context where the trust chain may be found. Tests which apply only to the certificate may pass `NULL` instead.

The `flags` parameter is used to set which tests are run, according to the following table:

Flag|Implies|Context Required?|Tests
-|-|-|-
EC_CHECK_CERT||No|Certificate is the correct version, a valid public key is present, the current time falls within the certificate's validity period, any records present are of a valid length, any section records have a NULL-terminated string key.
EC_CHECK_SECRET||No|A secret key is present
EC_CHECK_SIGN||Yes<sup>1</sup>|Signer ID is present, signature is present, signer is available, validity period falls within signer validity period, signature passes cryptographic validation.


<sup>1</sup> Context is not required if the certificate is self-signed.