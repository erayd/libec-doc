# Usage

```c
#include <ec.h>

int main(void) {
    if(ec_init() != 0) {
        //init failed
    }
    ...
}
```

`ec.h` is the only header required, and declares all user-accessible functionality. `-lec` will link to it.

`ec_init()` should be called before any other function in the library.

All functions other than `ec_init()` are thread-safe, but data structures should not be shared by multiple threads without appropriate protection.

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