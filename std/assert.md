# assert.h - Diagnostics

The `assert.h` header defines the `assert` macro and the `static_assert` macro (since C11).

## Facilities Overview

| Facility Category | Key Symbols | Description |
|-------------------|-------------|-------------|
| Runtime Diagnostics| assert | Aborting program execution if a condition is false. |
| Static Diagnostics | static_assert | Compile-time constant expression checking. |

---

## Types & Variables

### static_assert

```c
#define static_assert(expression, message)
```

(C11) Checks the `expression` at compile time. If it is false, the compiler issues an error with the given `message`.

**Example:**

```c
#include <assert.h>
#include <stdint.h>

static_assert(sizeof(int) >= 4, "Integers must be at least 32 bits");

int main(void) {
    return 0;
}
```

---

## Functions

### Runtime Diagnostics

#### assert

```c
#define assert(expression)
```

If `expression` is false (equal to zero), `assert` writes information about the call that failed to stderr and then calls `abort`.

**Note:** If the macro `NDEBUG` is defined when `assert.h` is included, the `assert` macro expands to nothing.

**Example:**

```c
#include <assert.h>
#include <stdio.h>

int main(void) {
    int x = 5;
    assert(x == 5);
    printf("Assertion passed!\n");
    
    // assert(x == 10); // This would abort the program
    return 0;
}
```