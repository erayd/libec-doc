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

##ec_init()
`ec_err_t ec_init(void);`

Initialises libec and its dependencies. Returns a nonzero error code on failure.

This function may be called more than once, but is not thread-safe. All other functions are thread-safe.

```c
#include <ec/h>
...
if(ec_init() == 0) {
    //initialised OK
}
...
```