# Export & Import

Certificates may be exported / imported in either binary or base64-encoded form. Binary is more efficient and should be used by default in most cases - base64 is intended for scenarios where a user may need to interact with the certificate directly (for example, pasting into a web form).

A certificate's secret key will ***not*** be exported unless EC_EXPORT_SECRET is set, but will always be imported if available.

##API

 * [ec_export_len()](#ec-export-len)
 * [ec_export_len_64()](#ec-export-len-64)
 * [ec_export()](#ec-export)
 * [ec_export_64()](#ec-export-64)
 * [ec_import()](#ec-import)
 * [ec_import_64()](#ec-import-64)

###ec_export_len()
`uint16_t ec_export_len(ec_cert_t *c, uint8_t export_flags);`

Get the buffer length required to export a certificate with the given flags.

```c
#include <ec.h>
...
unsigned char buf[ec_export_len(c, EC_EXPORT_SECRET)];
...
```

###ec_export_len_64
`size_t ec_export_len_64(ec_cert_t *c, uint8_t export_flags);`

Identical to `ec_export_len()`, except returns the buffer length required to export to base64.

###ec_export()
`size_t ec_export(unsigned char *dest, ec_cert_t *c, uint8_t export_flags);`

Export a certificate to the given buffer. Returns the number of bytes written to the buffer, or zero on failure.

```c
#include <ec.h>
...
size_t bytes_written = ec_export(buf, c, EC_EXPORT_SECRET);
if(bytes_written == 0) {
    //failed to export certificate
}
...
```

###ec_export_64()
`char *ec_export_64(char *dest, ec_cert_t *c, uint8_t export_flags);`

Identical to `ec_export()`, except that the certificate is base64-encoded and armoured before writing to `dest`. Returns `dest` on success, or NULL on failure.

###ec_import()
`ec_cert_t *ec_import(unsigned char *src, size_t length, size_t *consumed);`

Import a certificate from a buffer. Returns a certificate on success, or NULL otherwise.

If `consumed` is not NULL, then the number of bytes consumed from `src` will be stored there. On error, this value is considered undefined.

The certificate is tested with `EC_CHECK_CERT` before returning - if it does not pass, the import is considered to have failed and NULL is returned. 

```c
#include <ec.h>
...
ec_cert_t *c = ec_import(buf, buf_size);
if(c != NULL) {
    //imported successfully
}
...
```

###ec_import_64()
`ec_cert_t *ec_import_64(char *src, size_t length, size_t *consumed);`

Identical to `ec_import()`, except interprets the buffer as armoured and base64-encoded. The certificate may be present at any offset in the buffer, provided that the envelope is intact.