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

`ec_init()` should be called before any other function in the library. This function may be called more than once, but is not thread-safe. All other functions are thread-safe.