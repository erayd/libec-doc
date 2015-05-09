# Records

Records are the fundamental unit used to store additional data attached to a certificate. Records are key / value pairs. The key and value are both optional, and may contain either string or binary data.

Records are organised by section. Each section begins with a 'section header' record, which must use a NULL-terminated string containing the name of the section as its key.

Section names beginning with an underscore are reserved for internal use by the library.

##ec_record()
`ec_record_t *ec_record(uint16_t flags, char *key, unsigned char *data, uint16_t data_len);`

Create a new record. Returns a pointer to the new record, or NULL on failure. `key` must be a NULL-terminated string.

`flags` is used to set additional metadata on the record, according to the following table:
