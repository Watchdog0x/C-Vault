# time.h - Time and Date

**Category:** 📂 std/ (C Standard Library)  
**Header:** `<time.h>`  
**Scope:** C89, C99, C11

The `time.h` header declares several functions for manipulating and displaying time and date information.

## Facilities Overview

| Facility Category | Key Symbols | Description |
|-------------------|-------------|-------------|
| [Time Manipulation](#time-manipulation) | [time](#time), [difftime](#difftime), [mktime](#mktime), [clock](#clock) | Getting system time and calculating intervals. |
| [Conversion](#conversion-functions) | [gmtime](#gmtime), [localtime](#localtime), [strftime](#strftime), [ctime](#ctime), [asctime](#asctime) | Formatting time into strings or calendar structures. |
| [Types & Macros](#types--variables) | time_t, clock_t, struct tm | Core data structures for representing time. |

---

## Types & Variables

### struct tm

A structure holding a calendar date and time broken down into its components:

```c
struct tm {
    int tm_sec;   // seconds [0, 60]
    int tm_min;   // minutes [0, 59]
    int tm_hour;  // hours [0, 23]
    int tm_mday;  // day of month [1, 31]
    int tm_mon;   // month of year [0, 11]
    int tm_year;  // years since 1900
    int tm_wday;  // day of week [0, 6] (Sunday = 0)
    int tm_yday;  // day of year [0, 365]
    int tm_isdst; // daylight saving time flag
};
```

### time_t

```c
typedef long time_t;
```

Arithmetic type capable of representing times (usually seconds since Unix Epoch: January 1, 1970).

### clock_t

```c
typedef long clock_t;
```

Arithmetic type capable of representing process processor time.

### CLOCKS_PER_SEC

```c
#define CLOCKS_PER_SEC 1000000
```

Macro representing the number of clock ticks per second, used to convert `clock()` return values to seconds.

---

## Functions

### Time Manipulation

#### time

```c
time_t time(time_t *timer)
```

Returns the current system calendar time. If `timer` is not NULL, the return value is also stored in `*timer`.

**Example:**

```c
#include <time.h>
#include <stdio.h>

int main(void) {
    time_t now = time(NULL);
    printf("Seconds since Epoch: %ld\n", (long)now);
    // Output: Seconds since Epoch: 1703082000 (example)
    return 0;
}
```

#### difftime

```c
double difftime(time_t time1, time_t time0)
```

Computes the difference in seconds between two calendar times (`time1 - time0`).

**Example:**

```c
#include <time.h>
#include <stdio.h>

int main(void) {
    time_t start = time(NULL);
    // Simulate some work
    for (volatile long i = 0; i < 100000000; i++);
    time_t end = time(NULL);
    
    double elapsed = difftime(end, start);
    printf("Elapsed time: %.0f seconds\n", elapsed);
    return 0;
}
```

#### mktime

```c
time_t mktime(struct tm *timeptr)
```

Converts a `struct tm` to a `time_t` value. Useful for creating custom dates.

**Example:**

```c
#include <time.h>
#include <stdio.h>

int main(void) {
    struct tm custom_time = {0};
    custom_time.tm_year = 2024 - 1900; // Years since 1900
    custom_time.tm_mon = 0;            // January (0-11)
    custom_time.tm_mday = 1;           // 1st day
    custom_time.tm_hour = 12;
    custom_time.tm_min = 30;
    custom_time.tm_sec = 0;
    
    time_t t = mktime(&custom_time);
    printf("Custom time: %ld\n", (long)t);
    return 0;
}
```

#### clock

```c
clock_t clock(void)
```

Returns the processor time used by the program since it started. Divide by `CLOCKS_PER_SEC` to get seconds.

**Example:**

```c
#include <time.h>
#include <stdio.h>

int main(void) {
    clock_t start = clock();
    
    // Simulate some work
    for (volatile long i = 0; i < 100000000; i++);
    
    clock_t end = clock();
    double cpu_time = ((double)(end - start)) / CLOCKS_PER_SEC;
    printf("CPU time used: %f seconds\n", cpu_time);
    return 0;
}
```

---

### Conversion Functions

#### localtime

```c
struct tm *localtime(const time_t *timer)
```

Converts a calendar time to a `struct tm` representing local time (adjusted for timezone).

**Example:**

```c
#include <time.h>
#include <stdio.h>

int main(void) {
    time_t now = time(NULL);
    struct tm *lt = localtime(&now);
    printf("Local time: %d-%02d-%02d %02d:%02d:%02d\n",
           lt->tm_year + 1900, lt->tm_mon + 1, lt->tm_mday,
           lt->tm_hour, lt->tm_min, lt->tm_sec);
    // Output: Local time: 2024-12-20 15:30:45 (example)
    return 0;
}
```

#### gmtime

```c
struct tm *gmtime(const time_t *timer)
```

Converts a calendar time to a `struct tm` representing UTC/GMT time.

**Example:**

```c
#include <time.h>
#include <stdio.h>

int main(void) {
    time_t now = time(NULL);
    struct tm *gt = gmtime(&now);
    printf("UTC time: %d-%02d-%02d %02d:%02d:%02d\n",
           gt->tm_year + 1900, gt->tm_mon + 1, gt->tm_mday,
           gt->tm_hour, gt->tm_min, gt->tm_sec);
    return 0;
}
```

#### strftime

```c
size_t strftime(char *s, size_t max, const char *format, const struct tm *tm)
```

Formats the time in `tm` according to `format` and stores it in string `s`. Returns the number of characters written.

**Common format specifiers:**
- `%Y` - Year (4 digits)
- `%m` - Month (01-12)
- `%d` - Day of month (01-31)
- `%H` - Hour (00-23)
- `%M` - Minute (00-59)
- `%S` - Second (00-59)
- `%A` - Full weekday name
- `%B` - Full month name

**Example:**

```c
#include <time.h>
#include <stdio.h>

int main(void) {
    char buf[100];
    time_t now = time(NULL);
    struct tm *ts = localtime(&now);
    
    strftime(buf, sizeof(buf), "%Y-%m-%d %H:%M:%S", ts);
    printf("Formatted: %s\n", buf);
    // Output: Formatted: 2024-12-20 15:30:45
    
    strftime(buf, sizeof(buf), "%A, %B %d, %Y", ts);
    printf("Readable: %s\n", buf);
    // Output: Readable: Friday, December 20, 2024
    
    return 0;
}
```

#### ctime

```c
char *ctime(const time_t *timer)
```

Converts a calendar time to a string in a fixed format. Equivalent to `asctime(localtime(timer))`.

**Example:**

```c
#include <time.h>
#include <stdio.h>

int main(void) {
    time_t now = time(NULL);
    char *time_str = ctime(&now);
    printf("Current time: %s", time_str);
    // Output: Current time: Fri Dec 20 15:30:45 2024
    return 0;
}
```

#### asctime

```c
char *asctime(const struct tm *timeptr)
```

Converts a `struct tm` to a string in a fixed format.

**Example:**

```c
#include <time.h>
#include <stdio.h>

int main(void) {
    time_t now = time(NULL);
    struct tm *ts = localtime(&now);
    char *time_str = asctime(ts);
    printf("Time string: %s", time_str);
    // Output: Time string: Fri Dec 20 15:30:45 2024
    return 0;
}
```
