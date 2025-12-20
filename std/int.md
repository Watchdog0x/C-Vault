# stdint.h - Integer Types

The `stdint.h` header declares sets of integer types having specified widths and defines corresponding sets of macros.


## Facilities Overview

| Facility Category | Key Symbols | Description |
|-------------------|-------------|-------------|
| Fixed-width Integers| int8_t, int16_t, int32_t, int64_t | Exact-width signed and unsigned types. |
| Pointer-sized Integers| intptr_t, uintptr_t | Integers capable of holding pointer values. |
| Maximum-width Integers| intmax_t, uintmax_t | Widest available integer types. |
| Limit Macros | INT32_MAX, UINT64_MAX | Ranges for the defined types. |


---

## Types & Variables

### Fixed-width Integer Types

These types are provided only if the implementation supports them.

#### Exact Width

- `int8_t`, `int16_t`, `int32_t`, `int64_t`
- `uint8_t`, `uint16_t`, `uint32_t`, `uint64_t`

**Example:**

```c
#include <stdint.h>
#include <stdio.h>

int main(void) {
    uint32_t value = 0xFFFFFFFF;
    printf("Max 32-bit unsigned: %u\n", value);
    return 0;
}
```

### Pointer-sized Integer Types

#### intptr_t, uintptr_t

Signed and unsigned integer types capable of holding a pointer to void. Any valid pointer to void can be converted to these types and then back to a pointer to void, and the result will compare equal to the original pointer.

### Maximum-width Integer Types

#### intmax_t, uintmax_t

Signed and unsigned integer types that can represent any value of any other integer type supported by the implementation.

### Macros for Integer Limits

For each type defined in `stdint.h`, there are macros defining its minimum and maximum values (e.g., `INT8_MIN`, `INT8_MAX`, `UINT8_MAX`).

**Example:**

```c
#include <stdint.h>
#include <stdio.h>

int main(void) {
    printf("Range of int16_t: %d to %d\n", INT16_MIN, INT16_MAX);
    return 0;
}
```