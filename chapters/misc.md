# Misc
##ec_errstr()
`char *ec_errstr(ec_err_t error);`

Get a string description for a libec error code.

```c
#include <ec.h>
...
printf("Error: %s\n", ec_errstr(error));
...
```