# API
## Records
```c
//create a new record - use EC_RECORD_{KCOPY,KFREE,DCOPY,DFREE} for memory management
ec_record_t *ec_record(uint16_t flags, unsigned char *key,
    uint8_t key_len, unsigned char *data, uint16_t data_len);

//append a record to a certificate
ec_record_t *ec_append(ec_cert_t *c, char *section,
    ec_record_t *r);

//find the first matching record in a record list
ec_record_t *ec_match(ec_record_t *start, char *section, uint16_t flags,
    unsigned char *key, uint8_t key_len, unsigned char *data,
    uint16_t data_len);

//free a record, plus associated data if KFREE / DFREE is set
void ec_record_destroy(ec_record_t *r);
```

## Certificates
```c
//create a new certificate
ec_cert_t *ec_cert(void);

//sign a certificate and set the validity period
ec_err_t ec_sign(ec_cert_t *c, ec_cert_t *signer, uint64_t valid_from,
    uint64_t valid_until);

//check that a certificate is valid. See EC_CHECK_* for possible tests.
ec_err_t ec_check(ec_cert_t *c, int checks);

//compare two certificates - returns zero if equal, arbitrary nonzero
//otherwise
int ec_certcmp(ec_cert_t *c1, ec_cert_t *c2);

//free a certificate and all attached records
void ec_cert_destroy(ec_cert_t *c);
```

## Roles & Grants
```c
//add a role to a certificate
ec_record_t *ec_role_add(ec_cert_t *c, char *role);

//add a grant to a certificate
ec_record_t *ec_role_grant(ec_cert_t *c, char *role);

//check whether a certificate has the given role - returns nonzero
//error if it does not
ec_err_t ec_role_has(ec_cert_t *c, char *role);

//check whether a certificate grants the given role - returns nonzero
//error if it does not
ec_err_t ec_role_has_grant(ec_cert_t *c, char *role);
```

## Import & Export
```c
//get the export length for a certificate
size_t ec_export_len(ec_cert_t *c, int flags);

//export a certificate
ec_err_t ec_export(unsigned char *dest, ec_cert_t *c, int flags);

//import a certificate
ec_cert_t *ec_import(unsigned char *src, size_t src_len, int flags);
```