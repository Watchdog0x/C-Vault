# stdlib.h - Standard General Utilities

**Category:** 📂 std/ (C Standard Library)  
**Header:** `<stdlib.h>`  
**Scope:** C89, C99, C11

The `stdlib.h` header defines four types and several macros, and declares functions for general utility services.

## Facilities Overview

| Facility Category | Key Symbols | Description |
|-------------------|-------------|-------------|
| [Memory Management](#memory-management) | [malloc](#malloc), [calloc](#calloc), [realloc](#realloc), [free](#free) | Allocation and deallocation of heap memory. |
| [Process Control](#process-control) | [exit](#exit), [abort](#abort), [atexit](#atexit), [_Exit](#_exit), [quick_exit](#quick_exit), [at_quick_exit](#at_quick_exit) | Terminating execution and registering cleanup hooks. |
| [Numeric Conversion](#numeric-conversion) | [atoi](#atoi), [atol](#atol), [atof](#atof), [strtol](#strtol), [strtod](#strtod), [strtoll](#strtoll), [strtoull](#strtoull), [strtold](#strtold) | Parsing strings into integer and floating-point types. |
| [Pseudo-Random Numbers](#pseudo-random-numbers) | [rand](#rand), [srand](#srand) | Generating sequences of pseudo-random integers. |
| [Searching & Sorting](#searching--sorting) | [qsort](#qsort), [bsearch](#bsearch) | Generic algorithms for array manipulation. |
| [Integer Arithmetic](#integer-arithmetic) | [abs](#abs), [labs](#labs), [llabs](#llabs), [div](#div), [ldiv](#ldiv), [lldiv](#lldiv) | Absolute values and integer division with remainder. |
| [Environment](#environment) | [system](#system), [getenv](#getenv) | Executing commands and reading environment variables. |
| [Multibyte Characters](#multibyte-characters) | [mblen](#mblen), [mbtowc](#mbtowc), [wctomb](#wctomb), [mbstowcs](#mbstowcs), [wcstombs](#wcstombs) | Basic multibyte string conversion utilities. |

---

## Types & Variables

### size_t

```c
typedef unsigned long size_t;
```

Unsigned integer type used for representing sizes of objects.

### div_t

```c
struct div_t {
    int quot;  // quotient
    int rem;   // remainder
};
```

Structure returned by `div` containing the quotient and remainder of integer division.

### ldiv_t

```c
struct ldiv_t {
    long quot;  // quotient
    long rem;   // remainder
};
```

Structure returned by `ldiv` containing the quotient and remainder of long integer division.

### lldiv_t

```c
struct lldiv_t {
    long long quot;  // quotient
    long long rem;   // remainder
};
```

Structure returned by `lldiv` containing the quotient and remainder of long long integer division.

### EXIT_SUCCESS

```c
#define EXIT_SUCCESS 0
```

Constant indicating successful program termination.

### EXIT_FAILURE

```c
#define EXIT_FAILURE 1
```

Constant indicating unsuccessful program termination.

### RAND_MAX

```c
#define RAND_MAX 2147483647
```

Maximum value returned by `rand()`.

### MB_CUR_MAX

```c
#define MB_CUR_MAX 1
```

Maximum number of bytes in a multibyte character for the current locale.

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

Allocates memory for an array of `nmemb` elements of `size` bytes each and initializes all bytes to zero.

**Example:**

```c
#include <stdlib.h>
#include <stdio.h>

int main(void) {
    int *arr = calloc(5, sizeof(int)); // Allocates 5 ints, all zero
    if (arr) {
        for (int i = 0; i < 5; i++) {
            printf("%d ", arr[i]); // Output: 0 0 0 0 0
        }
        free(arr);
    }
    return 0;
}
```

#### realloc

```c
void *realloc(void *ptr, size_t size)
```

Changes the size of the memory block pointed to by `ptr` to `size` bytes. May move the block to a new location.

**Example:**

```c
#include <stdlib.h>
#include <stdio.h>

int main(void) {
    int *ptr = malloc(5 * sizeof(int));
    if (ptr) {
        // Use ptr...
        int *new_ptr = realloc(ptr, 10 * sizeof(int)); // Resize to 10 ints
        if (new_ptr) {
            ptr = new_ptr;
            // Use resized ptr...
        }
        free(ptr);
    }
    return 0;
}
```

#### free

```c
void free(void *ptr)
```

Deallocates the space previously allocated by `malloc`, `calloc`, or `realloc`.

**Example:**

```c
#include <stdlib.h>

int main(void) {
    int *ptr = malloc(sizeof(int));
    if (ptr) {
        *ptr = 42;
        free(ptr); // Deallocate
        // ptr is now invalid
    }
    return 0;
}
```

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

Causes abnormal program termination, without cleaning up resources. Signals SIGABRT.

**Example:**

```c
#include <stdlib.h>

int main(void) {
    abort(); // Program terminates abnormally
    return 0;
}
```

#### atexit

```c
int atexit(void (*func)(void))
```

Registers a function to be called at normal program termination.

**Example:**

```c
#include <stdlib.h>
#include <stdio.h>

void cleanup(void) {
    printf("Cleanup called\n");
}

int main(void) {
    atexit(cleanup);
    printf("Exiting...\n");
    exit(0);
    // Output: Exiting...
    //         Cleanup called
}
```

#### _Exit

```c
void _Exit(int status)
```

Terminates the calling process normally without cleaning up resources (C99).

**Example:**

```c
#include <stdlib.h>

int main(void) {
    _Exit(EXIT_SUCCESS);
}
```

#### quick_exit

```c
void quick_exit(int status)
```

Terminates the calling process normally with quick cleanup (C11).

**Example:**

```c
#include <stdlib.h>

void quick_cleanup(void) {
    // Quick cleanup
}

int main(void) {
    at_quick_exit(quick_cleanup);
    quick_exit(EXIT_SUCCESS);
}
```

#### at_quick_exit

```c
int at_quick_exit(void (*func)(void))
```

Registers a function to be called at quick program termination (C11).

**Example:**

```c
#include <stdlib.h>

void quick_cleanup(void) {
    // Perform quick cleanup
}

int main(void) {
    at_quick_exit(quick_cleanup);
    quick_exit(0);
}
```

---

### Numeric Conversion

#### atoi

```c
int atoi(const char *nptr)
```

Converts the initial portion of the string `nptr` to an `int`. Ignores leading whitespace, stops at first non-digit.

**Example:**

```c
#include <stdlib.h>
#include <stdio.h>

int main(void) {
    const char *str = "123abc";
    int num = atoi(str);
    printf("%d\n", num); // Output: 123
    return 0;
}
```

#### atol

```c
long atol(const char *nptr)
```

Converts the initial portion of the string `nptr` to a `long`.

**Example:**

```c
#include <stdlib.h>
#include <stdio.h>

int main(void) {
    const char *str = "123456789";
    long num = atol(str);
    printf("%ld\n", num); // Output: 123456789
    return 0;
}
```

#### atof

```c
double atof(const char *nptr)
```

Converts the initial portion of the string `nptr` to a `double`.

**Example:**

```c
#include <stdlib.h>
#include <stdio.h>

int main(void) {
    const char *str = "3.14159";
    double num = atof(str);
    printf("%f\n", num); // Output: 3.141590
    return 0;
}
```

#### strtol

```c
long strtol(const char *nptr, char **endptr, int base)
```

Converts the initial portion of `nptr` to a `long` in the given `base` (2-36). Sets `endptr` to point after the converted part.

**Example:**

```c
#include <stdlib.h>
#include <stdio.h>

int main(void) {
    const char *str = "123abc";
    char *end;
    long num = strtol(str, &end, 10);
    printf("Number: %ld, Remaining: %s\n", num, end); // Number: 123, Remaining: abc
    return 0;
}
```

#### strtod

```c
double strtod(const char *nptr, char **endptr)
```

Converts the initial portion of `nptr` to a `double`. Sets `endptr` to point after the converted part.

**Example:**

```c
#include <stdlib.h>
#include <stdio.h>

int main(void) {
    const char *str = "3.14159abc";
    char *end;
    double num = strtod(str, &end);
    printf("Number: %f, Remaining: %s\n", num, end); // Number: 3.141590, Remaining: abc
    return 0;
}
```

#### strtoll

```c
long long strtoll(const char *nptr, char **endptr, int base)
```

Converts the initial portion of `nptr` to a `long long` in the given `base` (C99).

**Example:**

```c
#include <stdlib.h>
#include <stdio.h>

int main(void) {
    const char *str = "123456789012345";
    char *end;
    long long num = strtoll(str, &end, 10);
    printf("Number: %lld\n", num);
    return 0;
}
```

#### strtoull

```c
unsigned long long strtoull(const char *nptr, char **endptr, int base)
```

Converts the initial portion of `nptr` to an `unsigned long long` in the given `base` (C99).

**Example:**

```c
#include <stdlib.h>
#include <stdio.h>

int main(void) {
    const char *str = "18446744073709551615";
    char *end;
    unsigned long long num = strtoull(str, &end, 10);
    printf("Number: %llu\n", num);
    return 0;
}
```

#### strtold

```c
long double strtold(const char *nptr, char **endptr)
```

Converts the initial portion of `nptr` to a `long double` (C99).

**Example:**

```c
#include <stdlib.h>
#include <stdio.h>

int main(void) {
    const char *str = "3.141592653589793";
    char *end;
    long double num = strtold(str, &end);
    printf("Number: %Lf\n", num);
    return 0;
}
```

---

### Pseudo-Random Numbers

#### rand

```c
int rand(void)
```

Returns a pseudo-random integer between 0 and RAND_MAX.

**Example:**

```c
#include <stdlib.h>
#include <stdio.h>

int main(void) {
    int random_num = rand();
    printf("Random number: %d\n", random_num);
    return 0;
}
```

#### srand

```c
void srand(unsigned int seed)
```

Seeds the pseudo-random number generator used by `rand()`.

**Example:**

```c
#include <stdlib.h>
#include <stdio.h>
#include <time.h>

int main(void) {
    srand(time(NULL)); // Seed with current time
    int random_num = rand();
    printf("Seeded random number: %d\n", random_num);
    return 0;
}
```

---

### Searching & Sorting

#### qsort

```c
void qsort(void *base, size_t nmemb, size_t size, int (*compar)(const void *, const void *))
```

Sorts an array of `nmemb` elements, each of `size` bytes, using the comparison function `compar`.

**Example:**

```c
#include <stdlib.h>
#include <stdio.h>

int compare_int(const void *a, const void *b) {
    return *(int*)a - *(int*)b;
}

int main(void) {
    int arr[] = {3, 1, 4, 1, 5};
    qsort(arr, 5, sizeof(int), compare_int);
    for (int i = 0; i < 5; i++) {
        printf("%d ", arr[i]); // Output: 1 1 3 4 5
    }
    return 0;
}
```

#### bsearch

```c
void *bsearch(const void *key, const void *base, size_t nmemb, size_t size, int (*compar)(const void *, const void *))
```

Searches for `key` in a sorted array of `nmemb` elements.

**Example:**

```c
#include <stdlib.h>
#include <stdio.h>

int compare_int(const void *a, const void *b) {
    return *(int*)a - *(int*)b;
}

int main(void) {
    int arr[] = {1, 2, 3, 4, 5};
    int key = 3;
    int *result = bsearch(&key, arr, 5, sizeof(int), compare_int);
    if (result) {
        printf("Found: %d\n", *result); // Output: Found: 3
    }
    return 0;
}
```

---

### Integer Arithmetic

#### abs

```c
int abs(int x)
```

Computes the absolute value of `x`.

**Example:**

```c
#include <stdlib.h>
#include <stdio.h>

int main(void) {
    printf("abs(-5) = %d\n", abs(-5)); // Output: 5
    return 0;
}
```

#### labs

```c
long labs(long x)
```

Computes the absolute value of `x` for `long` type.

**Example:**

```c
#include <stdlib.h>
#include <stdio.h>

int main(void) {
    printf("labs(-123456L) = %ld\n", labs(-123456L)); // Output: 123456
    return 0;
}
```

#### llabs

```c
long long llabs(long long x)
```

Computes the absolute value of `x` for `long long` type (C99).

**Example:**

```c
#include <stdlib.h>
#include <stdio.h>

int main(void) {
    printf("llabs(-9223372036854775807LL) = %lld\n", llabs(-9223372036854775807LL));
    return 0;
}
```

#### div

```c
div_t div(int numer, int denom)
```

Computes quotient and remainder of integer division.

**Example:**

```c
#include <stdlib.h>
#include <stdio.h>

int main(void) {
    div_t result = div(10, 3);
    printf("10 / 3 = %d remainder %d\n", result.quot, result.rem); // Output: 3 remainder 1
    return 0;
}
```

#### ldiv

```c
ldiv_t ldiv(long numer, long denom)
```

Computes quotient and remainder of long integer division.

**Example:**

```c
#include <stdlib.h>
#include <stdio.h>

int main(void) {
    ldiv_t result = ldiv(100000L, 3000L);
    printf("100000 / 3000 = %ld remainder %ld\n", result.quot, result.rem);
    return 0;
}
```

#### lldiv

```c
lldiv_t lldiv(long long numer, long long denom)
```

Computes quotient and remainder of long long integer division (C99).

**Example:**

```c
#include <stdlib.h>
#include <stdio.h>

int main(void) {
    lldiv_t result = lldiv(9223372036854775807LL, 2LL);
    printf("9223372036854775807 / 2 = %lld remainder %lld\n", result.quot, result.rem);
    return 0;
}
```

---

### Environment

#### system

```c
int system(const char *command)
```

Executes a command in the host environment.

**Example:**

```c
#include <stdlib.h>

int main(void) {
    system("echo Hello World"); // Executes shell command
    return 0;
}
```

#### getenv

```c
char *getenv(const char *name)
```

Gets the value of an environment variable.

**Example:**

```c
#include <stdlib.h>
#include <stdio.h>

int main(void) {
    char *path = getenv("PATH");
    if (path) {
        printf("PATH: %s\n", path);
    }
    return 0;
}
```

---

### Multibyte Characters

#### mblen

```c
int mblen(const char *s, size_t n)
```

Returns the number of bytes in the next multibyte character.

**Example:**

```c
#include <stdlib.h>
#include <stdio.h>

int main(void) {
    const char *str = "a"; // Single byte
    int len = mblen(str, 1);
    printf("Length: %d\n", len); // Output: 1
    return 0;
}
```

#### mbtowc

```c
int mbtowc(wchar_t *pwc, const char *s, size_t n)
```

Converts a multibyte character to a wide character.

**Example:**

```c
#include <stdlib.h>
#include <stdio.h>
#include <wchar.h>

int main(void) {
    const char *str = "a";
    wchar_t wc;
    int len = mbtowc(&wc, str, 1);
    printf("Converted: %lc, Length: %d\n", wc, len);
    return 0;
}
```

#### wctomb

```c
int wctomb(char *s, wchar_t wc)
```

Converts a wide character to a multibyte character.

**Example:**

```c
#include <stdlib.h>
#include <stdio.h>
#include <wchar.h>

int main(void) {
    wchar_t wc = L'a';
    char mb[MB_CUR_MAX];
    int len = wctomb(mb, wc);
    printf("Converted: %s, Length: %d\n", mb, len);
    return 0;
}
```

#### mbstowcs

```c
size_t mbstowcs(wchar_t *pwcs, const char *s, size_t n)
```

Converts a multibyte string to a wide string.

**Example:**

```c
#include <stdlib.h>
#include <stdio.h>
#include <wchar.h>

int main(void) {
    const char *str = "hello";
    wchar_t wcs[10];
    size_t len = mbstowcs(wcs, str, 10);
    printf("Converted length: %zu\n", len);
    return 0;
}
```

#### wcstombs

```c
size_t wcstombs(char *s, const wchar_t *pwcs, size_t n)
```

Converts a wide string to a multibyte string.

**Example:**

```c
#include <stdlib.h>
#include <stdio.h>
#include <wchar.h>

int main(void) {
    const wchar_t *wcs = L"hello";
    char mbs[20];
    size_t len = wcstombs(mbs, wcs, 20);
    printf("Converted: %s, Length: %zu\n", mbs, len);
    return 0;
}
