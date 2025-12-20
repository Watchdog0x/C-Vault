# stddef.h - Common Definitions

The `stddef.h` header defines several commonly used types and macros.

## Facilities Overview

| Facility Category | Key Symbols | Description |
|-------------------|-------------|-------------|
| Types | size_t, ptrdiff_t, wchar_t | Standard integer types for sizes and offsets. |
| Macros | NULL, offsetof | Common pointer and structure utilities. |

---

## Types & Variables

### size_t

Unsigned integer type used for representing the size of objects.

### ptrdiff_t

Signed integer type used for representing the difference between two pointers.

### wchar_t

An integer type whose range of values can represent distinct codes for all members of the largest extended character set.

### NULL

Macro that expands to a null pointer constant.

### offsetof

```c
#define offsetof(type, member)
```

Expands to an integral constant expression that has type `size_t`, the value of which is the offset in bytes to the structure member `member` from the beginning of its structure `type`.

**Example:**

```c
#include <stddef.h>
#include <stdio.h>

struct data {
    int id;
    char name[20];
    double score;
};

int main(void) {
    printf("Offset of score: %zu\n", offsetof(struct data, score));
    return 0;
}
```