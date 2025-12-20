# tgmath.h - Type-generic Math

**Category:** 📂 std/ (C Standard Library)  
**Header:** `<tgmath.h>`  
**Scope:** C99, C11

The `tgmath.h` header defines type-generic macros that expand to the appropriate `math.h` or `complex.h` function based on the types of the arguments.

## Facilities Overview

| Facility Category | Key Symbols | Description |
|-------------------|-------------|-------------|
| [Generic Functions](#generic-behavior) | sin, cos, pow, sqrt, log | Overloaded macros for real and complex types. |

---

## Functions

### Generic Behavior

For a function `f`, a call to its macro `f(x)` will resolve to:
- `f(x)` if `x` is `double`.
- `ff(x)` if `x` is `float`.
- `fl(x)` if `x` is `long double`.
- `cf(x)` if `x` is `complex`.

<details><summary>Example</summary>

```c
#include <tgmath.h>
#include <stdio.h>
#include <complex.h>

int main(void) {
    float f = 2.0f;
    double d = 2.0;
    double complex c = 2.0 + 1.0 * I;

    // All these use the same 'sqrt' macro name:
    printf("float: %f\n", sqrt(f));
    printf("double: %f\n", sqrt(d));
    printf("complex: %.1f + %.1fi\n", creal(sqrt(c)), cimag(sqrt(c)));

    return 0;
}
```
</details>

