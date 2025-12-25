# complex.h - Complex Arithmetic

**Category:** 📂 std/ (C Standard Library)  
**Header:** `<complex.h>`  
**Scope:** C99

The `complex.h` header declares functions and macros for performing arithmetic on complex numbers.

## Facilities Overview

| Facility Category | Key Symbols | Description |
|-------------------|-------------|-------------|
| [Arithmetic](#arithmetic) | [creal](#creal), [cimag](#cimag), [cabs](#cabs), [carg](#carg), [conj](#conj) | Extracting parts and modifying complex values. |
| [Functions](#functions) | [cexp](#cexp), [clog](#clog), [csqrt](#csqrt), [csin](#csin), [ccos](#ccos) | Complex versions of standard math functions. |
| [Types & Macros](#types--variables) | [double complex](#complex), [I](#i) | Native complex types and imaginary unit. |

---

## Types & Variables

### complex

Native keyword for complex types.

### I

```c
#define I _Complex_I
```

The imaginary unit `i`.

---

## Functions

### creal

```c
double creal(double complex z)
```

Extracts the real component.

**Returns:** The real part of `z` as a `double`.

### cimag

```c
double cimag(double complex z)
```

Extracts the imaginary component.

**Returns:** The imaginary part of `z` as a `double`.

<details><summary>Example</summary>

```c
#include <complex.h>
#include <stdio.h>

int main(void) {
    double complex z = 3.0 + 4.0 * I;
    printf("Imaginary: %.1f\n", cimag(z)); // 4.0
    return 0;
}
```

</details>

### cabs

```c
double cabs(double complex z)
```

<details><summary>Example</summary>

```c
#include <complex.h>
#include <stdio.h>

int main(void) {
    double complex z = 3.0 + 4.0 * I;
    printf("Magnitude: %.1f\n", cabs(z));
    return 0;
}
```

</details>

### carg

```c
double carg(double complex z)
```

Computes the argument (phase angle) of `z`.

<details><summary>Example</summary>

```c
#include <complex.h>
#include <stdio.h>

int main(void) {
    double complex z = 1.0 + 1.0 * I;
    printf("Argument: %.3f\n", carg(z)); // ≈ 0.785 (π/4)
    return 0;
}
```

</details>

### conj

```c
double complex conj(double complex z)
```

Computes the complex conjugate of `z`.

<details><summary>Example</summary>

```c
#include <complex.h>
#include <stdio.h>

int main(void) {
    double complex z = 3.0 + 4.0 * I;
    double complex c = conj(z);
    printf("Conjugate: %.1f + %.1fi\n", creal(c), cimag(c)); // 3.0 - 4.0i
    return 0;
}
```

</details>

### cexp

```c
double complex cexp(double complex z)
```

Computes the complex exponential of `z` ($e^z$).

**Returns:** The complex exponential value.

<details><summary>Example</summary>

```c
#include <complex.h>
#include <stdio.h>

int main(void) {
    double complex z = 1.0 + 1.0 * I;
    double complex res = cexp(z);
    
    // Euler's formula: e^(x+iy) = e^x * (cos(y) + i*sin(y))
    printf("e^(1+i) = %.2f + %.2fi\n", creal(res), cimag(res));
    return 0;
}
```

</details>

### clog

```c
double complex clog(double complex z)
```

Computes the complex natural logarithm of `z`.

<details><summary>Example</summary>

```c
#include <complex.h>

int main(void) {
    double complex z = 1.0 + 0.0 * I;
    double complex l = clog(z); // ≈ 0 + 0i
    return 0;
}
```

</details>

### csqrt

```c
double complex csqrt(double complex z)
```

Computes the complex square root of `z`.

<details><summary>Example</summary>

```c
#include <complex.h>

int main(void) {
    double complex z = -1.0 + 0.0 * I;
    double complex s = csqrt(z); // ≈ 0 + 1i
    return 0;
}
```

</details>

### csin

```c
double complex csin(double complex z)
```

Computes the complex sine of `z`.

<details><summary>Example</summary>

```c
#include <complex.h>

int main(void) {
    double complex z = 0.0 + 0.0 * I;
    double complex s = csin(z); // 0 + 0i
    return 0;
}
```

</details>

### ccos

```c
double complex ccos(double complex z)
```

Computes the complex cosine of `z`.

<details><summary>Example</summary>

```c
#include <complex.h>

int main(void) {
    double complex z = 0.0 + 0.0 * I;
    double complex c = ccos(z); // 1 + 0i
    return 0;
}
```

</details>

### ctan

```c
double complex ctan(double complex z)
```

Computes the complex tangent of `z`.

<details><summary>Example</summary>

```c
#include <complex.h>

int main(void) {
    double complex z = 0.0 + 0.0 * I;
    double complex t = ctan(z); // 0 + 0i
    return 0;
}
```
</details>

