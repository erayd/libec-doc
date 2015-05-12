# Usage

```c
#include <ec.h>
...
int main(void) {
    if(ec_init() != 0) {
        //init failed
    }
    ...
}
...
```

`ec.h` is the only header required, and declares all user-accessible functionality. `-lec` will link to it.

`ec_init()` should be called before any other function in the library.

All functions other than `ec_init()` are thread-safe, but data structures should not be shared by multiple threads without appropriate protection.
