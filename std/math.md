# math.h - Mathematical Functions

**Category:** 📂 std/ (C Standard Library)  
**Header:** `<math.h>`  
**Scope:** C89, C99, C11

The `math.h` header declares a set of functions to perform common mathematical operations and manipulations on floating-point numbers.

## Facilities Overview

| Facility Category | Key Symbols | Description |
|-------------------|-------------|-------------|
| [Trigonometric](#trigonometric-functions) | [sin](#sin), [cos](#cos), [tan](#tan), [asin](#asin), [acos](#acos), [atan](#atan), [atan2](#atan2) | Standard angle-based transformations. |
| [Exponential & Log](#exponential--logarithmic) | [exp](#exp), [log](#log), [log10](#log10), [pow](#pow), [sqrt](#sqrt), [cbrt](#cbrt) | Power and logarithmic operations. |
| [Rounding & Remainder](#rounding--remainder) | [ceil](#ceil), [floor](#floor), [round](#round), [trunc](#trunc), [fmod](#fmod), [remainder](#remainder) | Mapping reals to integers or modular arithmetic. |
| [Absolute & Min/Max](#absolute-value--minmax) | [fabs](#fabs), [fmax](#fmax), [fmin](#fmin) | Magnitude and value bounds. |
| [Hyperbolic](#hyperbolic-functions) | [sinh](#sinh), [cosh](#cosh), [tanh](#tanh), [asinh](#asinh), [acosh](#acosh), [atanh](#atanh) | Hyperbolic angle transformations. |

---

## Types & Variables

### HUGE_VAL

```c
#define HUGE_VAL (__builtin_huge_val())
```

Positive `double` expression representing a very large value, often used to indicate overflow.

### INFINITY

```c
#define INFINITY (__builtin_inf())
```

Constant expression of type `float` representing positive infinity (C99).

### NAN

```c
#define NAN (__builtin_nan(""))
```

Constant expression of type `float` representing a quiet NaN (Not a Number) (C99).

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
    double angle = 0.0;
    printf("sin(0) = %f\n", sin(angle)); // Output: sin(0) = 0.000000
    printf("sin(π/2) = %f\n", sin(1.5708)); // Output: sin(π/2) ≈ 1.000000
    return 0;
}
```

#### cos

```c
double cos(double x)
```

Computes the cosine of an angle `x` (measured in radians).

**Example:**

```c
#include <math.h>
#include <stdio.h>

int main(void) {
    double angle = 0.0;
    printf("cos(0) = %f\n", cos(angle)); // Output: cos(0) = 1.000000
    printf("cos(π) = %f\n", cos(3.14159)); // Output: cos(π) ≈ -1.000000
    return 0;
}
```

#### tan

```c
double tan(double x)
```

Computes the tangent of an angle `x` (measured in radians).

**Example:**

```c
#include <math.h>
#include <stdio.h>

int main(void) {
    double angle = 0.7854; // π/4 radians (45 degrees)
    printf("tan(π/4) = %f\n", tan(angle)); // Output: tan(π/4) ≈ 1.000000
    return 0;
}
```

#### asin

```c
double asin(double x)
```

Computes the arc sine of `x`, returning a value in the range [-π/2, π/2].

**Example:**

```c
#include <math.h>
#include <stdio.h>

int main(void) {
    double value = 0.5;
    printf("asin(0.5) = %f radians\n", asin(value)); // Output: ≈ 0.523599 (30°)
    return 0;
}
```

#### acos

```c
double acos(double x)
```

Computes the arc cosine of `x`, returning a value in the range [0, π].

**Example:**

```c
#include <math.h>
#include <stdio.h>

int main(void) {
    double value = 0.5;
    printf("acos(0.5) = %f radians\n", acos(value)); // Output: ≈ 1.047198 (60°)
    return 0;
}
```

#### atan

```c
double atan(double x)
```

Computes the arc tangent of `x`, returning a value in the range [-π/2, π/2].

**Example:**

```c
#include <math.h>
#include <stdio.h>

int main(void) {
    double value = 1.0;
    printf("atan(1.0) = %f radians\n", atan(value)); // Output: ≈ 0.785398 (45°)
    return 0;
}
```

#### atan2

```c
double atan2(double y, double x)
```

Computes the arc tangent of `y/x`, using the signs of both arguments to determine the correct quadrant. Returns a value in the range [-π, π].

**Example:**

```c
#include <math.h>
#include <stdio.h>

int main(void) {
    double y = 1.0, x = 1.0;
    printf("atan2(1, 1) = %f radians\n", atan2(y, x)); // Output: ≈ 0.785398
    printf("atan2(1, -1) = %f radians\n", atan2(1.0, -1.0)); // Output: ≈ 2.356194
    return 0;
}
```

---

### Exponential & Logarithmic

#### exp

```c
double exp(double x)
```

Computes the exponential function e^x.

**Example:**

```c
#include <math.h>
#include <stdio.h>

int main(void) {
    double x = 1.0;
    printf("e^1 = %f\n", exp(x)); // Output: e^1 ≈ 2.718282
    printf("e^0 = %f\n", exp(0.0)); // Output: e^0 = 1.000000
    return 0;
}
```

#### log

```c
double log(double x)
```

Computes the natural logarithm (base e) of `x`.

**Example:**

```c
#include <math.h>
#include <stdio.h>

int main(void) {
    double x = 2.718282;
    printf("ln(e) = %f\n", log(x)); // Output: ln(e) ≈ 1.000000
    printf("ln(10) = %f\n", log(10.0)); // Output: ln(10) ≈ 2.302585
    return 0;
}
```

#### log10

```c
double log10(double x)
```

Computes the common logarithm (base 10) of `x`.

**Example:**

```c
#include <math.h>
#include <stdio.h>

int main(void) {
    double x = 100.0;
    printf("log10(100) = %f\n", log10(x)); // Output: log10(100) = 2.000000
    printf("log10(1000) = %f\n", log10(1000.0)); // Output: log10(1000) = 3.000000
    return 0;
}
```

#### pow

```c
double pow(double x, double y)
```

Computes `x` raised to the power of `y` (x^y).

**Example:**

```c
#include <math.h>
#include <stdio.h>

int main(void) {
    double base = 2.0, exponent = 8.0;
    printf("2^8 = %f\n", pow(base, exponent)); // Output: 2^8 = 256.000000
    printf("3^3 = %f\n", pow(3.0, 3.0)); // Output: 3^3 = 27.000000
    return 0;
}
```

#### sqrt

```c
double sqrt(double x)
```

Computes the square root of `x`.

**Example:**

```c
#include <math.h>
#include <stdio.h>

int main(void) {
    double x = 16.0;
    printf("sqrt(16) = %f\n", sqrt(x)); // Output: sqrt(16) = 4.000000
    printf("sqrt(2) = %f\n", sqrt(2.0)); // Output: sqrt(2) ≈ 1.414214
    return 0;
}
```

#### cbrt

```c
double cbrt(double x)
```

Computes the cube root of `x` (C99).

**Example:**

```c
#include <math.h>
#include <stdio.h>

int main(void) {
    double x = 27.0;
    printf("cbrt(27) = %f\n", cbrt(x)); // Output: cbrt(27) = 3.000000
    printf("cbrt(8) = %f\n", cbrt(8.0)); // Output: cbrt(8) = 2.000000
    return 0;
}
```

---

### Rounding & Remainder

#### ceil

```c
double ceil(double x)
```

Rounds `x` upward to the nearest integer.

**Example:**

```c
#include <math.h>
#include <stdio.h>

int main(void) {
    printf("ceil(2.3) = %f\n", ceil(2.3)); // Output: ceil(2.3) = 3.000000
    printf("ceil(-2.3) = %f\n", ceil(-2.3)); // Output: ceil(-2.3) = -2.000000
    return 0;
}
```

#### floor

```c
double floor(double x)
```

Rounds `x` downward to the nearest integer.

**Example:**

```c
#include <math.h>
#include <stdio.h>

int main(void) {
    printf("floor(2.7) = %f\n", floor(2.7)); // Output: floor(2.7) = 2.000000
    printf("floor(-2.3) = %f\n", floor(-2.3)); // Output: floor(-2.3) = -3.000000
    return 0;
}
```

#### round

```c
double round(double x)
```

Rounds `x` to the nearest integer value (C99).

**Example:**

```c
#include <math.h>
#include <stdio.h>

int main(void) {
    printf("round(2.3) = %f\n", round(2.3)); // Output: round(2.3) = 2.000000
    printf("round(2.7) = %f\n", round(2.7)); // Output: round(2.7) = 3.000000
    return 0;
}
```

#### trunc

```c
double trunc(double x)
```

Rounds `x` toward zero, removing the fractional part (C99).

**Example:**

```c
#include <math.h>
#include <stdio.h>

int main(void) {
    printf("trunc(2.7) = %f\n", trunc(2.7)); // Output: trunc(2.7) = 2.000000
    printf("trunc(-2.7) = %f\n", trunc(-2.7)); // Output: trunc(-2.7) = -2.000000
    return 0;
}
```

#### fmod

```c
double fmod(double x, double y)
```

Computes the floating-point remainder of `x/y`.

**Example:**

```c
#include <math.h>
#include <stdio.h>

int main(void) {
    double x = 7.5, y = 2.0;
    printf("fmod(7.5, 2.0) = %f\n", fmod(x, y)); // Output: fmod(7.5, 2.0) = 1.500000
    printf("fmod(10.0, 3.0) = %f\n", fmod(10.0, 3.0)); // Output: fmod(10.0, 3.0) = 1.000000
    return 0;
}
```

#### remainder

```c
double remainder(double x, double y)
```

Computes the IEEE remainder of `x/y` (C99).

**Example:**

```c
#include <math.h>
#include <stdio.h>

int main(void) {
    double x = 10.0, y = 3.0;
    printf("remainder(10.0, 3.0) = %f\n", remainder(x, y)); // Output: ≈ 1.000000
    return 0;
}
```

---

### Absolute Value & Min/Max

#### fabs

```c
double fabs(double x)
```

Computes the absolute value of `x`.

**Example:**

```c
#include <math.h>
#include <stdio.h>

int main(void) {
    printf("fabs(-5.5) = %f\n", fabs(-5.5)); // Output: fabs(-5.5) = 5.500000
    printf("fabs(5.5) = %f\n", fabs(5.5)); // Output: fabs(5.5) = 5.500000
    return 0;
}
```

#### fmax

```c
double fmax(double x, double y)
```

Returns the larger of two floating-point values (C99).

**Example:**

```c
#include <math.h>
#include <stdio.h>

int main(void) {
    printf("fmax(3.5, 2.1) = %f\n", fmax(3.5, 2.1)); // Output: fmax(3.5, 2.1) = 3.500000
    printf("fmax(-1.0, -5.0) = %f\n", fmax(-1.0, -5.0)); // Output: fmax(-1.0, -5.0) = -1.000000
    return 0;
}
```

#### fmin

```c
double fmin(double x, double y)
```

Returns the smaller of two floating-point values (C99).

**Example:**

```c
#include <math.h>
#include <stdio.h>

int main(void) {
    printf("fmin(3.5, 2.1) = %f\n", fmin(3.5, 2.1)); // Output: fmin(3.5, 2.1) = 2.100000
    printf("fmin(-1.0, -5.0) = %f\n", fmin(-1.0, -5.0)); // Output: fmin(-1.0, -5.0) = -5.000000
    return 0;
}
```

---

### Hyperbolic Functions

#### sinh

```c
double sinh(double x)
```

Computes the hyperbolic sine of `x`.

**Example:**

```c
#include <math.h>
#include <stdio.h>

int main(void) {
    double x = 1.0;
    printf("sinh(1.0) = %f\n", sinh(x)); // Output: sinh(1.0) ≈ 1.175201
    return 0;
}
```

#### cosh

```c
double cosh(double x)
```

Computes the hyperbolic cosine of `x`.

**Example:**

```c
#include <math.h>
#include <stdio.h>

int main(void) {
    double x = 1.0;
    printf("cosh(1.0) = %f\n", cosh(x)); // Output: cosh(1.0) ≈ 1.543081
    return 0;
}
```

#### tanh

```c
double tanh(double x)
```

Computes the hyperbolic tangent of `x`.

**Example:**

```c
#include <math.h>
#include <stdio.h>

int main(void) {
    double x = 1.0;
    printf("tanh(1.0) = %f\n", tanh(x)); // Output: tanh(1.0) ≈ 0.761594
    return 0;
}
```

#### asinh

```c
double asinh(double x)
```

Computes the inverse hyperbolic sine of `x` (C99).

**Example:**

```c
#include <math.h>
#include <stdio.h>

int main(void) {
    double x = 1.0;
    printf("asinh(1.0) = %f\n", asinh(x)); // Output: asinh(1.0) ≈ 0.881374
    return 0;
}
```

#### acosh

```c
double acosh(double x)
```

Computes the inverse hyperbolic cosine of `x` (C99).

**Example:**

```c
#include <math.h>
#include <stdio.h>

int main(void) {
    double x = 2.0;
    printf("acosh(2.0) = %f\n", acosh(x)); // Output: acosh(2.0) ≈ 1.316958
    return 0;
}
```

#### atanh

```c
double atanh(double x)
```

Computes the inverse hyperbolic tangent of `x` (C99).

**Example:**

```c
#include <math.h>
#include <stdio.h>

int main(void) {
    double x = 0.5;
    printf("atanh(0.5) = %f\n", atanh(x)); // Output: atanh(0.5) ≈ 0.549306
    return 0;
}
```
