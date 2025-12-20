# complex.h - Complex Arithmetic

The `complex.h` header declares functions and macros for performing arithmetic on complex numbers.

## Facilities Overview

| Facility Category | Key Symbols | Description |
|-------------------|-------------|-------------|
| Arithmetic | creal, cimag, cabs, carg, conj | Extracting parts and modifying complex values. |
| Functions | csin, ccos, cexp, clog, csqrt | Complex versions of standard math functions. |
| Types & Macros | double complex, I | Native complex types and imaginary unit. |

---

## Types & Variables

### complex

Native keyword for complex types.

### I

The imaginary unit `i`.

---

## Functions

#### creal

```c
double creal(double complex z)
```

Extracts the real component.

#### cimag

```c
double cimag(double complex z)
```

Extracts the imaginary component.

#### cabs

```c
double cabs(double complex z)
```

**Example:**

```c
#include <complex.h>
#include <stdio.h>

int main(void) {
    double complex z = 3.0 + 4.0 * I;
    printf("Magnitude: %.1f\n", cabs(z));
    return 0;
}
```

#### carg

```c
double carg(double complex z)
```

#### conj

```c
double complex conj(double complex z)
```