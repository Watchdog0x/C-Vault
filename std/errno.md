# errno.h - Errors

**Category:** 📂 std/ (C Standard Library)  
**Header:** `<errno.h>`  
**Scope:** C89, C99, C11

The `errno.h` header defines several macros for reporting error conditions.

## Facilities Overview

| Facility Category | Key Symbols | Description |
|-------------------|-------------|-------------|
| [Error Reporting](#error-reporting) | [errno](#errno) | Macro that expands to a modifiable lvalue of type int. |
| [Error Constants](#error-constants) | EDOM, ERANGE, EILSEQ | Predefined error codes for domain, range, and encoding errors. |

---

## Types & Variables

### errno

```c
#define errno /* implementation-defined */
```

An integer variable that is set by system calls and some library functions in the event of an error to indicate what went wrong. Its value is significant only when the return value of the call indicated an error.

<details><summary>Example</summary>

```c
#include <errno.h>
#include <math.h>
#include <stdio.h>

int main(void) {
    errno = 0;
    double val = sqrt(-1.0);
    if (errno == EDOM) {
        printf("Domain error occurred\n");
    }
    return 0;
}
```

</details>

### Error Constants

- `EDOM`: Domain error (argument not in function's domain). 
- `ERANGE`: Range error (result too large). 
- `EILSEQ`: Illegal byte sequence (invalid multibyte conversion).
