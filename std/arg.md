# stdarg.h - Variable Arguments

**Category:** 📂 std/ (C Standard Library)  
**Header:** `<stdarg.h>`  
**Scope:** C89, C99, C11

The `stdarg.h` header defines macros for stepping through a list of function arguments whose number and types are not known to the called function.

## Facilities Overview

| Facility Category | Key Symbols | Description |
|-------------------|-------------|-------------|
| [Macros](#argument-list-management) | [va_start](#va_start), [va_arg](#va_arg), [va_end](#va_end), [va_copy](#va_copy) | Managing variable argument lists. |
| [Types](#types--variables) | [va_list](#va_list) | Type for holding argument list information. |

---

## Types & Variables

### va_list

An opaque type used to hold the information needed by the macros `va_start`, `va_arg`, `va_end`, and `va_copy`.


## Functions

### Argument List Management

### va_start

```c
void va_start(va_list ap, last)
```

Initializes `ap` for use by `va_arg` and `va_end`. `last` is the name of the last fixed parameter.

### va_arg

```c
type va_arg(va_list ap, type)
```

Expands to the next argument in the list with the specified `type`.

### va_end

```c
void va_end(va_list ap)
```

Cleans up the `va_list` initialized by `va_start`.

<details><summary>Example</summary>

```c
#include <stdarg.h>
#include <stdio.h>

void print_ints(int count, ...) {
    va_list args;
    va_start(args, count);
    for (int i = 0; i < count; i++) {
        printf("%d ", va_arg(args, int));
    }
    va_end(args);
    printf("\n");
}

int main(void) {
    print_ints(3, 10, 20, 30);
    return 0;
}
```
</details>

