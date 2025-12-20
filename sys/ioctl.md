# sys/ioctl.h - Device Control

The `sys/ioctl.h` header defines the `ioctl` function for device-specific control operations.

## Facilities Overview

| Facility Category | Key Symbols | Description |
|-------------------|-------------|-------------|
| Control | ioctl | Performing various device-specific operations. |
| Terminal Control | TIOCGWINSZ | Example command for getting terminal window size. |

---

## Types & Variables

### request (Commands)

Device-specific commands like:
- `TIOCGWINSZ`: Get window size.
- `FIONBIO`: Set non-blocking I/O.

#### struct winsize

Used with `TIOCGWINSZ`:

```c
struct winsize {
    unsigned short ws_row;
    unsigned short ws_col;
    unsigned short ws_xpixel;   /* unused */
    unsigned short ws_ypixel;   /* unused */
};
```

---

## Functions

### Device Control

#### ioctl

```c
int ioctl(int fd, unsigned long request, ...)
```

The `ioctl()` function manipulates the underlying device parameters of special files.

**Example:**

```c
#include <sys/ioctl.h>
#include <stdio.h>
#include <unistd.h>

int main(void) {
    struct winsize w;
    if (ioctl(STDOUT_FILENO, TIOCGWINSZ, &w) == 0) {
        printf("Terminal size: %d rows, %d columns\n", w.ws_row, w.ws_col);
    }
    return 0;
}
```