# Misc

##ec_init()
`ec_err_t ec_init(void);`

Initialises libec and its dependencies. Returns a nonzero error code on failure. `ec_init()` must be called before any other function in the library.

This function may be called more than once, but is not thread-safe.

```c
#include <ec/h>
...
if(ec_init() == 0) {
    //initialised OK
}
...
```

##ec_errstr()
`char *ec_errstr(ec_err_t error);`

Get a string description for a libec error code.

```c
#include <ec.h>
...
printf("Error: %s\n", ec_errstr(error));
...
```