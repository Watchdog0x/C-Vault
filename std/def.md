# stddef.h - Common Definitions

**Category:** 📂 std/ (C Standard Library)  
**Header:** `<stddef.h>`  
**Scope:** C89, C99, C11

The `stddef.h` header defines several commonly used types and macros.

## Facilities Overview

| Facility Category | Key Symbols | Description |
|-------------------|-------------|-------------|
| [Types](#types--variables) | [size_t](#size_t), [ptrdiff_t](#ptrdiff_t), [wchar_t](#wchar_t) | Standard integer types for sizes and offsets. |
| [Macros](#types--variables) | [NULL](#null), [offsetof](#offsetof) | Common pointer and structure utilities. |

---

## Format Specifiers

Format specifiers for types defined in `<stddef.h>`:

| Specifier | Data Type | Description |
|-----------|-----------|-------------|
| `%zu` | `size_t` | Unsigned size type (e.g., for array lengths). |
| `%td` | `ptrdiff_t` | Signed pointer difference type. |
| `%lc` | `wchar_t` | Wide character. |

---

## Types & Variables

### size_t

```c
typedef unsigned long size_t;
```

Unsigned integer type used for representing the size of objects. Commonly used with the `%zu` format specifier for printing.

**Example:**

```c
#include <stddef.h>
#include <stdio.h>

int main(void) {
    size_t array_size = 10;
    printf("Array size: %zu\n", array_size);
    return 0;
}
```

### ptrdiff_t

```c
typedef long ptrdiff_t;
```

Signed integer type used for representing the difference between two pointers. Commonly used with the `%td` format specifier for printing.

**Example:**

```c
#include <stddef.h>
#include <stdio.h>

int main(void) {
    int arr[] = {1, 2, 3, 4, 5};
    int *ptr1 = &arr[0];
    int *ptr2 = &arr[4];
    ptrdiff_t diff = ptr2 - ptr1;
    printf("Pointer difference: %td\n", diff);
    return 0;
}
```

### wchar_t

```c
typedef int wchar_t;
```

An integer type whose range of values can represent distinct codes for all members of the largest extended character set. Used for wide character handling.

**Example:**

```c
#include <stddef.h>
#include <stdio.h>
#include <wchar.h>

int main(void) {
    wchar_t wc = L'A';
    wprintf(L"Wide character: %lc\n", wc);
    return 0;
}
```

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
