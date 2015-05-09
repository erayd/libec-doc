# Records

Records are the fundamental unit used to store additional data attached to a certificate. Records are key / value pairs. The key and value are both optional, and may contain either string or binary data.

Records are organised by section. Each section begins with a 'section header' record, which must use a NULL-terminated string containing the name of the section as its key.

Section names beginning with an underscore are reserved for internal use by the library.

