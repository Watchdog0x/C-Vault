# setjmp.h - Non-local Jumps

**Category:** 📂 std/ (C Standard Library)  
**Header:** `<setjmp.h>`  
**Scope:** C89, C99, C11

The `setjmp.h` header declares the function `longjmp` and defines the macro `setjmp`, for bypassing the normal function call and return discipline.

## Facilities Overview

| Facility Category | Key Symbols | Description |
|-------------------|-------------|-------------|
| Control Flow | setjmp, longjmp | Saving and restoring program state (jump buffers). |
| Types | jmp_buf | Structure for storing execution environment. |

---

## Types & Variables

### jmp_buf

An array type suitable for holding the information needed to restore a calling environment.


## Functions

### Control Flow

### setjmp

```c
int setjmp(jmp_buf env)
```

Saves the current stack environment in `env`. Returns `0` when called directly; returns a non-zero value when returning via `longjmp`.

### longjmp

```c
void longjmp(jmp_buf env, int val)
```

Restores the environment saved in `env` by the most recent call to `setjmp`. The program execution resumes at the `setjmp` call point as if it had returned `val`.

**Example:**

```c
#include <setjmp.h>
#include <stdio.h>

jmp_buf buffer;

void error_recovery(void) {
    printf("Error occurred! Jumping back...\n");
    longjmp(buffer, 1);
}

int main(void) {
    if (setjmp(buffer) == 0) {
        printf("Initial state saved.\n");
        // error_recovery();
    } else {
        printf("Recovered in main.\n");
    }
    return 0;
}
```
