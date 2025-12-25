# sys/time.h - Time Types and Functions

**Category:** 📂 sys/ (Linux & POSIX API)
**Header:** `<sys/time.h>`
**Scope:** POSIX.1-2008

The `sys/time.h` header defines data structures and functions for high-precision timing, primarily `gettimeofday`.

## Facilities Overview

| Facility Category | Key Symbols | Description |
|-------------------|-------------|-------------|
| [Time Retrieval](#gettimeofday) | [gettimeofday](#gettimeofday) | Getting system time with microsecond precision. |
| [Time Setting](#settimeofday) | [settimeofday](#settimeofday) | Setting the system clock (privileged). |
| [Structures](#struct-timeval) | [timeval](#struct-timeval) | Structure for seconds and microseconds. |

---

## Types & Variables

### struct timeval

```c
struct timeval {
    time_t      tv_sec;     /* seconds */
    suseconds_t tv_usec;    /* microseconds */
};
```

Represents a time interval or absolute time with microsecond resolution.

### struct timezone

```c
struct timezone {
    int tz_minuteswest;     /* minutes west of Greenwich */
    int tz_dsttime;         /* type of DST correction */
};
```

*Obsolete*. Used for timezone information in `gettimeofday`.

---

## Functions

### gettimeofday

```c
int gettimeofday(struct timeval *restrict tv, struct timezone *restrict tz)
```

Gets the system's clock time.

*   `tv`: Pointer to a `timeval` structure to fill.
*   `tz`: Usually `NULL` (timezone semantics are obsolete).

**Returns:** `0` on success. Returns `-1` on error and sets `errno`.

<details><summary>Example</summary>

```c
#include <sys/time.h>
#include <stdio.h>

int main(void) {
    struct timeval start, end;
    
    gettimeofday(&start, NULL);
    
    // Do some work
    for (volatile int i = 0; i < 1000000; i++);
    
    gettimeofday(&end, NULL);
    
    long seconds = end.tv_sec - start.tv_sec;
    long micros = end.tv_usec - start.tv_usec;
    double elapsed = seconds + micros*1e-6;
    
    printf("Elapsed: %.6f seconds\n", elapsed);
    return 0;
}
```

</details>

### settimeofday

```c
int settimeofday(const struct timeval *tv, const struct timezone *tz)
```

Sets the system's clock time. Requires appropriate privileges (e.g., root).

**Returns:** `0` on success. Returns `-1` on error.
