# stdlib.h - Standard General Utilities

The `stdlib.h` header defines four types and several macros, and declares functions for general utility services.

## Facilities Overview

| Facility Category | Key Symbols | Description |
|-------------------|-------------|-------------|
| Memory Management | malloc, calloc, realloc, free, aligned_alloc | Allocation and deallocation of heap memory. |
| Process Control | exit, abort, atexit, quick_exit, _Exit | Terminating execution and registering cleanup hooks. |
| Numeric Conversion | atoi, atol, strtol, strtod, strtoul | Parsing strings into integer and floating-point types. |
| Pseudo-Random Numbers | rand, srand | Generating sequences of pseudo-random integers. |
| Searching & Sorting | qsort, bsearch | Generic algorithms for array manipulation. |
| Integer Arithmetic | abs, labs, div, ldiv | Absolute values and integer division with remainder. |
| Environment | system, getenv | Executing commands and reading environment variables. |
| Multibyte Characters | mblen, mbtowc, wctomb, mbstowcs | Basic multibyte string conversion utilities. |

---

## Types & Variables

### size_t

Unsigned integer type used for representing sizes of objects.

### div_t, ldiv_t, lldiv_t

Structures returned by `div`, `ldiv`, and `lldiv` containing the quotient (`quot`) and remainder (`rem`).

### EXIT_SUCCESS, EXIT_FAILURE

Standard integer constants for program exit status.

---

## Functions

### Memory Management

#### malloc

```c
void *malloc(size_t size)
```

Allocates `size` bytes of uninitialized storage.

**Example:**

```c
#include <stdlib.h>

int main(void) {
    int *ptr = malloc(10 * sizeof(int));
    if (ptr) {
        free(ptr);
    }
    return 0;
}
```

#### calloc

```c
void *calloc(size_t nmemb, size_t size)
```

Allocates memory for an array and initializes all bytes to zero.

#### realloc

```c
void *realloc(void *ptr, size_t size)
```

#### free

```c
void free(void *ptr)
```

Deallocates the space previously allocated.

---

### Process Control

#### exit

```c
void exit(int status)
```

Terminating the calling process normally.

**Example:**

```c
#include <stdlib.h>

int main(void) {
    exit(EXIT_SUCCESS);
}
```

#### abort

```c
void abort(void)
```

#### atexit

```c
int atexit(void (*func)(void))
```

---

### Numeric Conversion

#### atoi

```c
int atoi(const char *nptr)
```

Converts the initial portion of a string to an `int`.

#### strtol

```c
long strtol(const char *nptr, char **endptr, int base)
```

---

### Pseudo-Random Numbers

#### rand

```c
int rand(void)
```

Returns a pseudo-random integer.

#### srand

```c
void srand(unsigned int seed)
```