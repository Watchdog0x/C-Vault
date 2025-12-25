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

**Returns:** Does not return (jumps back).

> [!WARNING]
> **Safety Note**: Variables local to the function containing `setjmp` that are modified after `setjmp` but before `longjmp` may have indeterminate values unless declared `volatile`.

<details><summary>Example</summary>

```c
#include <setjmp.h>
#include <stdio.h>
#include <stdlib.h>

jmp_buf env;

void risky_operation(void) {
    printf("Operation failed... aborting.\n");
    longjmp(env, 1); // Jump back with code 1
}

int main(void) {
    // 'volatile' is critical here to preserve value across longjmp
    volatile int count = 0; 
    
    // Save state. Returns 0 on direct call, non-zero on longjmp return
    if (setjmp(env) == 0) {
        printf("State saved. Doing work...\n");
        count++; // Modified between setjmp and longjmp
        risky_operation();
    } else {
        printf("Recovered! Count is: %d\n", count); // count should be 1
    }
    return 0;
}
```

</details>

