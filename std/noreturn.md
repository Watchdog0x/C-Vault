# stdnoreturn.h - Non-returning Functions

The `stdnoreturn.h` (C11) header defines a macro for specifying that a function does not return to its caller.

## Facilities Overview

| Macro | Description |
|-------|-------------|
| noreturn | Indicates that a function does not return. |

---

## Types & Variables

### noreturn

```c
#define noreturn _Noreturn
```

Used in a function declaration to specify that the function will not return to its caller (e.g., it calls `exit()` or `abort()`).

**Example:**

```c
#include <stdnoreturn.h>
#include <stdlib.h>
#include <stdio.h>

noreturn void fatal(const char *msg) {
    fprintf(stderr, "Fatal error: %s\n", msg);
    exit(1);
}

int main(void) {
    fatal("System failure");
    return 0; // Never reached
}
```