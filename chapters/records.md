# Records

Records are the fundamental unit used to store additional data attached to a certificate (e.g. roles, grants, user data, etc.) Records are key / value pairs. The key and value are both optional, and may contain either string or binary data.

Records are organised by section. Each section begins with a 'section header' record, which must use a NULL-terminated string containing the name of the section as its key.

Section names beginning with an underscore are reserved for internal use by the library.

If a section name is prefixed by '$', then all records in that section (including the section header) *must* be signed. Certificates containing a NOSIGN record in such a section will not validate.

##API

 * [ec_record_create()](#ec-record-create)
 * [ec_record_bin()](#ec-record-bin)
 * [ec_record_str()](#ec-record-str)
 * [ec_record_destroy()](#ec-record-destroy)
 * [ec_record_add()](#ec-record-add)
 * [ec_record_remove()](#ec-record-remove)
 * [ec_record_remove_section()](#ec-record-remove-section)
 * [ec_record_match()](#ec-record-match)
 * [ec_record_match_bin()](#ec-record-match-bin)
 * [ec_record_match_str()](#ec-record-match-str)
 * [ec_record_next()](#ec-record-next)
 * [ec_record_set()](#ec-record-set)
 * [ec_record_get()](#ec-record-get)
 * [ec_record_section()](#ec-record-section)
 * [ec_record_data()](#ec-record-data)

###ec_record_create()
`ec_record_t *ec_record_create(uint16_t flags, char *key, unsigned char *data, uint16_t data_len);`

Create a new record. Returns a pointer to the new record, or NULL on failure. `key` must be a NULL-terminated string.

Records created with `ec_record_create()` must be freed using `ec_record_destroy()` once they are no longer required, unless they are attached to a certificate. All records attached to a certificate are automatically freed when the certificate is destroyed.

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
ec_record_t *r = ec_record_create(EC_RECORD_NOSIGN | EC_RECORD_DCOPY, "my_key", my_data,
    my_data_length);
if(r == NULL) {
    //failed to create record
}
...
```

###ec_record_bin()
`ec_record_t *ec_record_bin(uint16_t flags, unsigned char *key, uint8_t key_len, unsigned char *data, uint16_t data_len);`

Identical to `ec_record_create()`, except using a binary key.

###ec_record_str()
`ec_record_t *ec_record_str(uint16_t flags, char *key, char *data);`

Identical to `ec_record_create()`, except data is a NULL-terminated string.

###ec_record_destroy()
`void ec_record_destroy(ec_record_t *r);`

Destroy a record. If EC_RECORD_KFREE / EC_RECORD_DFREE are set, then also free the record's key / data respectively.

```c
#include <ec.h>
...
ec_record_destroy(r);
...
```

###ec_record_add()
`ec_record_t *ec_record_add(ec_cert_t *c, char *section, ec_record_t *r);`

Add a record to a certificate. Returns `r` on success, NULL otherwise.

The record will be added to the section referred to by `section`, and the section will be created if it does not already exist. If `EC_RECORD_SECTION` is set, then `section` will be ignored and the record will be treated as a section header.

```c
#include <ec.h>
...
if(ec_record_add(c, "my_section", my_record) == NULL) {
    //failed to add record
}
...
```

###ec_record_remove()
`ec_record_t *ec_record_remove(ec_cert_t *c, ec_record_t *r);`

Remove a record from a certificate, if present. Always returns `r`.

```c
#include <ec.h>
...
ec_record_remove(c, r);
...
```

###ec_record_remove_section()
`void ec_record_remove_section(ec_cert_t *c, char *section, ec_freefn_t freefn);`

Remove an entire section and all its child records from a certificate, if present.

If `freefn` is provided, it will be run once for each removed record, after that record is removed. If `freefn` is NULL, then nothing will be run on the removed records, and you should destroy them manually if they are no longer required.

```c
#include <ec.h>
...
ec_record_remove_section(c, "mySection", (ec_freefn_t)ec_record_destroy);
...
```

###ec_record_match()
`ec_record_t *ec_record_match(ec_record_t *start, char *section, uint16_t flags, char *key,`  
  `unsigned char *data, uint16_t data_len);`
  
Find the first matching record in a record list. `key` must be a NULL-terminated string. All provided flags must be present in a record for it to match. `section` must be defined unless searching for a section header, in which case it should be NULL.

```c
#include <ec.h>
...
ec_record_t *list = ec_cert_records(c);
ec_record_t *r = ec_record_match(list, "my_section", EC_RECORD_NOSIGN,
    "my_record", NULL, 0);
if(r != NULL) {
    //found a matching record
}
...
```

###ec_record_match_bin()
`ec_record_t *ec_record_match_bin(ec_record_t *start, char *section, uint16_t flags,`  
    `unsigned char *key, uint8_t key_len, unsigned char *data, uint16_t data_len);`

Identical to `ec_record_match()`, except using a binary key.

###ec_record_match_str()
`ec_record_t *ec_record_match_str(ec_record_t *start, char *section, uint16_t flags,`  
  `char *key, char *data);`

Identical to `ec_record_match()`, except data is a NULL-terminated string.

###ec_record_next()
`ec_record_t *ec_record_next(ec_record_t *start, int filter);`

Get the next matching record in the same section. The next record that matches `start` is returned, or NULL if no matching record is found.

`filter` specifies which aspects of a record must match, according to the following table:

Filter | Description
-|-
EC_MATCH_FLAGS|Flags must match
EC_MATCH_KEY|Key must match
EC_MATCH_KEY_LEN|Key length must match
EC_MATCH_DATA|Data must match
EC_MATCH_DATA_LEN|Data length must match

```c
#include <ec.h>
...
ec_record_t *next = ec_record_next(r, EC_MATCH_FLAGS | EC_MATCH_KEY);
if(next !== NULL) {
    //found a matching record
}
...
```

###ec_record_set()
`ec_record_t *ec_record_set(ec_cert_t *c, char *section, uint16_t flags, char *key, char *data);`

Identical to `ec_record_add(c, section, ec_record_str(flags, key, data));`

###ec_record_get()
`char *ec_record_get(ec_record_t *start, char *section, uint16_t flags, char *key);`

Get the NULL-terminated data field from a matching record. Returns NULL if not found, or if the data field is not NULL-terminated.

```c
#include <ec.h>
...
ec_record_t *list = ec_cert_records(c);
char *s = ec_record_get(list, "my_section", EC_RECORD_NOSIGN, "my_record");
if(s != NULL) {
    //found a valid matching string
}
...
```

###ec_record_section()
`char *ec_record_section(ec_record_t *r);`

Get the name for the section to which the record belongs. Returns the section name if present, or NULL if the record is not associated with a section, or is a section header.

```c
#include <ec.h>
...
char *section_name = ec_record_section(r);
if(section_name != NULL) {
    //record has a valid section name
}
...
```

###ec_record_data()
`unsigned char *ec_record_data(ec_record_t *r);`

Get a pointer to a record's data buffer. Returns NULL if `r` is NULL.

```c
#include <ec.h>
...
unsigned char *buf = ec_record_data(r);
...
```