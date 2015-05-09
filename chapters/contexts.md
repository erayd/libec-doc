# Contexts
Contexts are used to manage groups of certificates together. They contain a centralised configuration, and a pool of stored certificates.

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

ec_ctx_autoload(ctx, my_autoloader);
...
```