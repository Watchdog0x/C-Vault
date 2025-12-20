# limits.h - Implementation Limits

The `limits.h` header defines macros for the sizes and ranges of integer types and character types.

## Facilities Overview

| Facility Category | Key Symbols | Description |
|-------------------|-------------|-------------|
| Character Limits | CHAR_BIT, MB_LEN_MAX | Bit width and multibyte length. |
| Integer Ranges | INT_MIN, INT_MAX, LONG_MAX | Bounds for signed and unsigned integer types. |

---

## Types & Variables

### Character Types

- `CHAR_BIT`: Number of bits in a `char`. Usually 8.
- `SCHAR_MIN`, `SCHAR_MAX`: Range of `signed char`.
- `UCHAR_MAX`: Maximum value of `unsigned char`.

### Integer Types

- `SHRT_MIN`, `SHRT_MAX`: Range of `short`.
- `INT_MIN`, `INT_MAX`: Range of `int`.
- `LONG_MIN`, `LONG_MAX`: Range of `long`.
- `LLONG_MIN`, `LLONG_MAX`: Range of `long long`.

**Example:**

```c
#include <limits.h>
#include <stdio.h>

int main(void) {
    printf("Number of bits in a char: %d\n", CHAR_BIT);
    printf("Maximum value of an int: %d\n", INT_MAX);
    return 0;
}
```