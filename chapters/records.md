# Records

## ec_record()
Creates a new record. Once created, the caller is responsible for freeing the returned record using ec_record_destroy().

Returns a new ec_record_t, or NULL on error.

```c
ec_record_t *r = ec_record(EC_RECORD_NOSIGN, mykey, mykey_len,
    mydata, mydata_len);
```