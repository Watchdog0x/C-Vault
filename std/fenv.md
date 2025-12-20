# fenv.h - Floating-Point Environment

**Category:** 📂 std/ (C Standard Library)  
**Header:** `<fenv.h>`  
**Scope:** C99, C11

The `fenv.h` header defines several macros and functions for managing and controlling the floating-point environment.

## Facilities Overview

| Facility Category | Key Symbols | Description |
|-------------------|-------------|-------------|
| Exceptions | feclearexcept, fetestexcept | Managing floating-point status flags. |
| Rounding | fegetround, fesetround | Querying and setting the rounding direction. |
| Environment | fegetenv, fesetenv | Saving/restoring the entire FP state. |

---

## Types & Variables

### fenv_t

A type representing the entire floating-point environment.

### fexcept_t

A type representing the floating-point exception flags collectively.

---

## Functions

### Rounding Modes

Common macros for rounding directions:
- `FE_TONEAREST`: Round to nearest.
- `FE_UPWARD`: Round toward positive infinity.
- `FE_DOWNWARD`: Round toward negative infinity.
- `FE_TOWARDZERO`: Round toward zero.

<details><summary>Example</summary>

```c
#include <fenv.h>
#include <stdio.h>

int main(void) {
    fesetround(FE_UPWARD);
    printf("Current rounding mode: %d\n", fegetround());
    return 0;
}
```

</details>

### Exceptions

Common floating-point exception macros:
- `FE_DIVBYZERO`: Pole error.
- `FE_INEXACT`: Inexact result.
- `FE_INVALID`: Domain error.
- `FE_OVERFLOW`: Overflow.
- `FE_UNDERFLOW`: Underflow.
