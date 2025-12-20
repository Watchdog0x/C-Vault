# time.h - Time and Date

The `time.h` header declares several functions for manipulating and displaying time and date information.

## Facilities Overview

| Facility Category | Key Symbols | Description |
|-------------------|-------------|-------------|
| Time Manipulation | time, difftime, mktime, clock | Getting system time and calculating intervals. |
| Conversion | gmtime, localtime, strftime, ctime | Formatting time into strings or calendar structures. |
| Types & Macros | time_t, clock_t, struct tm | Core data structures for representing time. |

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

Arithmetic type capable of representing times (usually seconds since Unix Epoch).

### clock_t

Arithmetic type capable of representing process processor time.

---

## Functions

### Time Manipulation

#### time

```c
time_t time(time_t *timer)
```

Returns the current system calendar time. 

#### difftime

```c
double difftime(time_t time1, time_t time0)
```

Computes the difference in seconds between two calendar times (`time1 - time0`).

#### clock

```c
clock_t clock(void)
```

Returns the processor time used by the program since it started. Divided by `CLOCKS_PER_SEC` to get seconds.

**Example:**

```c
#include <time.h>
#include <stdio.h>

int main(void) {
    time_t now = time(NULL);
    printf("Seconds since Epoch: %ld\n", (long)now);
    return 0;
}
```

---

### Conversion Functions

#### localtime

```c
struct tm *localtime(const time_t *timer)
```

Converts a calendar time to a `struct tm` representing local time.

**Example:**

```c
#include <time.h>
#include <stdio.h>

int main(void) {
    time_t now = time(NULL);
    struct tm *lt = localtime(&now);
    printf("Year: %d, Month: %d, Day: %d\n", lt->tm_year + 1900, lt->tm_mon + 1, lt->tm_mday);
    return 0;
}
```

#### strftime

```c
size_t strftime(char *s, size_t max, const char *format, const struct tm *tm)
```

Formats the time in `tm` according to `format` and stores it in string `s`.

**Example:**

```c
#include <time.h>
#include <stdio.h>

int main(void) {
    char buf[80];
    time_t now = time(NULL);
    struct tm *ts = localtime(&now);
    strftime(buf, sizeof(buf), "%Y-%m-%d %H:%M:%S", ts);
    printf("Current time: %s\n", buf);
    return 0;
}
```