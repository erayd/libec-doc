# Contexts
Contexts are used to manage groups of certificates together. They contain a centralised configuration, and a pool of stored certificates.

When a context is destroyed, all associated certificates are also destroyed at the same time.

##ec_ctx_create()
`ec_ctx_t *ec_ctx_create(void);`

Creates a new context. Returns `NULL` on failure.

Contexts created using this function must be destroyed using `ec_ctx_destroy()` once they are no longer required.