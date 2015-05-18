# Contexts
Contexts are used to manage groups of certificates together. They contain a centralised configuration, and a pool of stored certificates.

Contexts are automatically used when checking a certificate's signer, trust chain, or grants & roles. In most cases, every certificate you create should be assigned to a context.

When a context is destroyed, all associated certificates are also destroyed at the same time.

##ec_ctx_create()
`ec_ctx_t *ec_ctx_create(void);`

Creates a new context. Returns `NULL` on failure.

Contexts created using this function must be destroyed using `ec_ctx_destroy()` once they are no longer required.

```c
#include <ec.h>
...
ec_ctx_t *ctx = ec_ctx_create();
if(ctx != NULL) {
    //context created OK
}
...
```

##ec_ctx_destroy()
`void ec_ctx_destroy(ec_ctx_t *ctx);`

Destroys a context previously created with `ec_ctx_create()`.

```c
#include <ec.h>
...
ec_ctx_destroy(ctx);
...
```

##ec_ctx_autoload()
`typedef ec_cert_t *(*ec_autoload_t)(ec_id_t id);`  
`void ec_ctx_autoload(ec_ctx_t *ctx, ec_autoload_t autoload);`

Sets the autoload function that the context will use when trying to locate certificates it doesn't have available in its internal store.

This can be used to e.g. load certificates from disk, via a network connection etc.

```c
#include <ec.h>
...
ec_cert_t *my_autoloader(ec_id_t id) {
    ec_cert_t *c = //load cert from somewhere
    return c;
}
...
ec_ctx_autoload(ctx, my_autoloader);
...
```

##ec_ctx_validator()
`void ec_ctx_validator(ec_ctx_t *ctx, ec_record_validator_t validator);`

Sets the validator function used to check whether records with `EC_RECORD_REQUIRE` are acceptable. The validator function should return zero for acceptable, nonzero otherwise.

A validator function *must* be set in order to use `EC_CHECK_REQUIRE`. The validator must reject as failed any records it does not understand.

All arguments to the validator are guaranteed to be present and correct, and the certificate will have passed at least `EC_CHECK_CERT` before the validator is run.

```c
#include <ec.h>
...
int my_validator(ec_ctx_t *ctx, ec_cert_t *c, ec_record_t *r) {
    if(record_relates_to_goldfish())
        return 0;
    else
        return 1;
}
...
ec_ctx_validator(ctx, my_validator);
...
```

##ec_ctx_next()
`ec_ctx_t *ec_ctx_next(ec_ctx_t *ctx, ec_ctx_t *next);`

Sets the next context to search when attempting to load a certificate from the context's internal store. Always returns `next`.

This is used when trying to locate a certificate using `ec_ctx_cert()` which is not available in the context's internal store of via autoload.

```c
#include <ec.h>
...
ec_ctx_next(ctx, my_other_ctx);
...
```

##ec_ctx_save()
`ec_cert_t *ec_ctx_save(ec_ctx_t *ctx, ec_cert_t *c);`

Saves a certificate in the context's internal store, and returns a pointer to the certificate. Returns NULL on failure.

Saving a certificate transfers its ownership to the context, and it will be automatically freed when it is overwritten or when the context is destroyed. The user should ***not*** manually destroy certificates which have been saved in a context.

```c
#include <ec.h>
...
if(ec_ctx_save(ctx, c) == NULL) {
    //failed to save certificate
}
...
```

##ec_ctx_cert()
`ec_cert_t *ec_ctx_cert(ec_ctx_t *ctx, ec_id_t id);`

Loads a certificate from the context's internal store. Returns NULL on failure (e.g. if a certificate is not found).

If an autoloader is defined, an attempt will be made to autoload the certificate if it is not already present in the store.

If an additional search context has been defined using `ec_ctx_next()`, that context will also be searched (*after* attempting to autoload).

```c
#include <ec.h>
...
ec_cert_t *c = ec_ctx_cert(c, my_cert_id);
if(c != NULL) {
    //certificate loaded OK
}
...
```