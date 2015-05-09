# Contexts
Contexts are used to manage groups of certificates together. They contain a centralised configuration, and a pool of stored certificates.

When a context is destroyed, all associated certificates are also destroyed at the same time.

##ec_ctx_create()
`ec_ctx_t *ec_ctx_create(void);`

Creates a new context. Returns `NULL` on failure.

The user must free a created context using `ec_ctx_destroy()` once it is no longer required.