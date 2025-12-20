# limits.h - Implementation Limits

**Category:** 📂 std/ (C Standard Library)  
**Header:** `<limits.h>`  
**Scope:** C89, C99, C11

The `limits.h` header defines macros for the sizes and ranges of integer types and character types.

## Facilities Overview

| Facility Category | Key Symbols | Description |
|-------------------|-------------|-------------|
| [Character Types](#character-types) | CHAR_BIT, MB_LEN_MAX | Bit width and multibyte length. |
| [Integer Types](#integer-types) | INT_MIN, INT_MAX, LONG_MAX | Bounds for signed and unsigned integer types. |

---

## Types & Variables

### Character Types

| Macro | Typical Value | Description |
|-------|---------------|-------------|
| `CHAR_BIT` | 8 | Number of bits in a `char`. |
| `SCHAR_MIN` | -128 | Minimum value of `signed char`. |
| `SCHAR_MAX` | 127 | Maximum value of `signed char`. |
| `UCHAR_MAX` | 255 | Maximum value of `unsigned char`. |
| `CHAR_MIN` | 0 or SCHAR_MIN | Minimum value of `char` (implementation-defined). |
| `CHAR_MAX` | UCHAR_MAX or SCHAR_MAX | Maximum value of `char` (implementation-defined). |
| `MB_LEN_MAX` | 1 | Maximum number of bytes in a multibyte character. |

### Integer Types

| Macro | Typical Value | Description |
|-------|---------------|-------------|
| `SHRT_MIN` | -32768 | Minimum value of `short`. |
| `SHRT_MAX` | 32767 | Maximum value of `short`. |
| `USHRT_MAX` | 65535 | Maximum value of `unsigned short`. |
| `INT_MIN` | -2147483648 | Minimum value of `int`. |
| `INT_MAX` | 2147483647 | Maximum value of `int`. |
| `UINT_MAX` | 4294967295 | Maximum value of `unsigned int`. |
| `LONG_MIN` | -9223372036854775808 | Minimum value of `long`. |
| `LONG_MAX` | 9223372036854775807 | Maximum value of `long`. |
| `ULONG_MAX` | 18446744073709551615 | Maximum value of `unsigned long`. |
| `LLONG_MIN` | -9223372036854775808 | Minimum value of `long long`. |
| `LLONG_MAX` | 9223372036854775807 | Maximum value of `long long`. |
| `ULLONG_MAX` | 18446744073709551615 | Maximum value of `unsigned long long`. |

<details><summary>Example</summary>

```c
#include <limits.h>
#include <stdio.h>

int main(void) {
    printf("Number of bits in a char: %d\n", CHAR_BIT);
    printf("Maximum value of an int: %d\n", INT_MAX);
    printf("Maximum value of an unsigned int: %u\n", UINT_MAX);
    printf("Minimum value of a long long: %lld\n", LLONG_MIN);
    printf("Maximum value of an unsigned long long: %llu\n", ULLONG_MAX);
    return 0;
}
```
</details>

