# inttypes.h - Integer Formatting

The `inttypes.h` header includes `stdint.h` and defines macros for formatting fixed-width integer types.

## Facilities Overview

| Facility Category | Key Symbols | Description |
|-------------------|-------------|-------------|
| Format Macros | PRIu32, PRId64, PRIxPTR | Conversion specifiers for `printf`. |
| Scan Macros | SCNu32, SCNd64 | Conversion specifiers for `scanf`. |
| Extended Conversion| strtoimax, strtoumax | Converting strings to widest available integers. |

---

## Types & Variables

### Format Specifiers

To ensure portable printing of fixed-width types, use these macros inside the format string.

**Example:**

```c
#include <inttypes.h>
#include <stdio.h>

int main(void) {
    uint64_t large = 123456789012345ULL;
    printf("Large value: %" PRIu64 "\n", large);
    return 0;
}
```

---

## Functions

### Numeric Conversion

#### strtoimax

```c
intmax_t strtoimax(const char *nptr, char **endptr, int base)
```

Equivalent to `strtol`, but returns `intmax_t`.

#### strtoumax

```c
uintmax_t strtoumax(const char *nptr, char **endptr, int base)
```

Equivalent to `strtoul`, but returns `uintmax_t`.