# float.h - Floating-Point Limits

The `float.h` header defines several macros that expand to various limits and parameters of the standard floating-point types.

## Facilities Overview

| Facility Category | Key Symbols | Description |
|-------------------|-------------|-------------|
| Precision | FLT_DIG, DBL_DIG | Number of decimal digits that can be represented. |
| Range | FLT_MIN, FLT_MAX, DBL_MAX | Minimum and maximum finite values. |
| Epsilon | FLT_EPSILON, DBL_EPSILON | Smallest difference between 1 and the next value. |

---

## Types & Variables

### Float Properties

- `FLT_DIG`: Number of decimal digits of precision.
- `FLT_MIN`: Minimum normalized positive value.
- `FLT_MAX`: Maximum finite value.
- `FLT_EPSILON`: Difference between 1.0 and the least value greater than 1.0.

### Double Properties

- `DBL_DIG`: Number of decimal digits of precision.
- `DBL_MIN`: Minimum normalized positive value.
- `DBL_MAX`: Maximum finite value.
- `DBL_EPSILON`: Difference between 1.0 and the least value greater than 1.0.

**Example:**

```c
#include <float.h>
#include <stdio.h>

int main(void) {
    printf("Float precision: %d digits\n", FLT_DIG);
    printf("Max double: %e\n", DBL_MAX);
    printf("Double epsilon: %e\n", DBL_EPSILON);
    return 0;
}
```