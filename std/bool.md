# stdbool.h - Boolean Types

**Category:** 📂 std/ (C Standard Library)  
**Header:** `<stdbool.h>`  
**Scope:** C99, C11

The `stdbool.h` header defines macros for boolean types and values.

## Facilities Overview

| Facility Category | Key Symbols | Description |
|-------------------|-------------|-------------|
| [Types](#types--variables) | [bool](#bool) | Expands to `_Bool`. |
| [Values](#types--variables) | [true](#true--false), [false](#true--false) | Standard boolean constants. |
| [Macros](#types--variables) | [__bool_true_false_are_defined](#types--variables) | Indicates availability of bool macros. |

---

## Types & Variables

### bool

```c
#define bool _Bool
```

A keyword/macro that identifies a boolean type, capable of holding values `0` (false) and `1` (true).

### true / false

```c
#define true 1
#define false 0
```

Standard macros for representing truth values.

**Example:**

```c
#include <stdbool.h>
#include <stdio.h>

int main(void) {
    bool working = true;
    if (working) {
        printf("System is active: %d\n", working);
    }
    return 0;
}
```
