# Records

Records are the fundamental unit used to store additional data attached to a certificate. Records are key / value pairs. The key and value are both optional, and may contain either string or binary data.

Records are organised by section. Each section begins with a 'section header' record, which must use a NULL-terminated string containing the name of the section as its key.

Section names beginning with an underscore are reserved for internal use by the library.

##ec_record()
`ec_record_t *ec_record(uint16_t flags, char *key, unsigned char *data, uint16_t data_len);`

Create a new record. Returns a pointer to the new record, or NULL on failure. `key` must be a NULL-terminated string.

`flags` is used to set additional metadata on the record, according to the following table:

Flag|Meaning
-|-
EC_RECORD_SECTION|Record is a section header.
EC_RECORD_REQUIRE|Client *must* understand this record. If it does not, the certificate must be treated as invalid.
EC_RECORD_INHERIT|If this record is a section header, then all records added to this section will inherit the same flags by default. Otherwise, this record will inherit its flags from the section header. Inheritance is defined as a bitwise OR with the section header for all inheritable flags.
EC_RECORD_NOSIGN|This record will not be signed, and should be treated as untrustworthy. Intended for adding comments, internal metadata etc. to an already-signed certificate.
EC_RECORD_KFREE|The key for this record will be automatically freed when the record is destroyed.
EC_RECORD_KCOPY|The key for this record will be copied, rather than added by reference. Implies EC_RECORD_KFREE.
EC_RECORD_DFREE|The data for this record will be automatically freed when the record is destroyed.
EC_RECORD_DCOPY|The data for this record will be copied, rather than added by reference. Implies EC_RECORD_DFREE.

```c
#include <ec.h>
...
ec_record_t *r = ec_record(EC_RECORD_NOSIGN | EC_RECORD_DCOPY, "my_key", my_data, my_data_length);
if(r == NULL) {
    //failed to create record
}
...
```

##ec_record_bin()
`ec_record_t *ec_record_bin(uint16_t flags, unsigned char *key, uint8_t key_len, unsigned char *data, uint16_t data_len);`

Identical to `ec_record()`, except using a binary key.

##ec_record_str()
`ec_record_t *ec_record_str(uint16_t flags, char *key, char *data);`

Identical to `ec_record()`, except data is a NULL-terminated string.

##ec_add()
`ec_record_t *ec_add(ec_cert_t *c, char *section, ec_record_t *r);`

Add a record to a certificate. Returns `r` on success, NULL otherwise.

The record will be added to the section referred to by `section`, and the section will be created if it does not already exist. If `EC_RECORD_SECTION` is set, then `section` will be ignored and the record will be treated as a section header.

```c
#include <ec.h>
...
if(ec_add(c, "my_section", my_record) == NULL) {
    //failed to add record
}
...
```

##ec_match()
`ec_record_t *ec_match(ec_record_t *start, char *section, uint16_t flags, char *key, unsigned char *data,`  
  `uint16_t data_len);`