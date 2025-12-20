# stdalign.h - Alignment

The `stdalign.h` header (C11) defines macros for querying and specifying the alignment of objects.

## Facilities Overview

| Macro | Description |
|-------|-------------|
| alignas | Specifying alignment for an object. |
| alignof | Querying the alignment requirement of a type. |

---

## Types & Variables

### alignas

Used to specify the alignment for an object.

### alignof

Used to query the alignment requirement of a type.

**Example:**

```c
#include <stdalign.h>
#include <stdio.h>

struct foo {
    char c;
    alignas(16) int i;
};

int main(void) {
    printf("Alignment of int: %zu\n", alignof(int));
    return 0;
}
```