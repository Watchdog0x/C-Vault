# math.h - Mathematical Functions

The `math.h` header declares a set of functions to perform common mathematical operations.

## Facilities Overview

| Facility Category | Key Symbols | Description |
|-------------------|-------------|-------------|
| Trigonometric | sin, cos, tan, asin, acos, atan, atan2 | Standard angle-based transformations. |
| Exponential & Log | exp, log, log10, pow, sqrt, cbrt | Power and logarithmic operations. |
| Rounding & Remainder| ceil, floor, round, trunc, fmod, remainder | Mapping reals to integers or modular arithmetic. |
| Absolute & Min/Max | fabs, fmax, fmin | Magnitude and value bounds. |
| Hyperbolic | sinh, cosh, tanh, asinh, acosh, atanh | Hyperbolic angle transformations. |

---

## Types & Variables

### HUGE_VAL

Positive `double` expression for large values.

### INFINITY

Expands to a constant expression of type `float` representing positive infinity.

### NAN

Expands to a constant expression of type `float` representing a quiet NaN.

---

## Functions

### Trigonometric Functions

#### sin

```c
double sin(double x)
```

Computes the sine of an angle `x` (measured in radians).

**Example:**

```c
#include <math.h>
#include <stdio.h>

int main(void) {
    printf("sin(0) = %f\n", sin(0.0));
    return 0;
}
```

#### cos

```c
double cos(double x)
```

#### tan

```c
double tan(double x)
```

---

### Exponential & Logarithmic

#### exp

```c
double exp(double x)
```

#### log

```c
double log(double x)
```

#### pow

```c
double pow(double x, double y)
```

#### sqrt

```c
double sqrt(double x)
```

**Example:**

```c
#include <math.h>
#include <stdio.h>

int main(void) {
    printf("sqrt(16) = %f\n", sqrt(16.0));
    return 0;
}
```

---

### Rounding & Remainder

#### ceil

```c
double ceil(double x)
```

#### floor

```c
double floor(double x)
```

#### fmod

```c
double fmod(double x, double y)
```

---

### Absolute Value

#### fabs

```c
double fabs(double x)
```