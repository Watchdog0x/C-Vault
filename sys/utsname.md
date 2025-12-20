# sys/utsname.h - System Identification

The `sys/utsname.h` header defines the `uname` function and the structure for retrieving system identification information.

## Facilities Overview

| Facility Category | Key Symbols | Description |
|-------------------|-------------|-------------|
| Identification | uname | Getting name and information about current kernel. |
| Structures | struct utsname | Structure containing system version/release info. |

---

## Types & Variables

### struct utsname

Stores system identification information:

```c
struct utsname {
    char sysname[];  /* Operating system name (e.g., "Linux") */
    char nodename[]; /* Name within "some implementation-defined network" */
    char release[];  /* Operating system release (e.g., "2.6.28") */
    char version[];  /* Operating system version */
    char machine[];  /* Hardware identifier */
    char domainname[]; /* NIS domain name */
};
```

---

## Functions

### System Info

#### uname

```c
int uname(struct utsname *buf)
```

Returns system information in the structure pointed to by `buf`.

**Example:**

```c
#include <sys/utsname.h>
#include <stdio.h>

int main(void) {
    struct utsname buffer;
    if (uname(&buffer) == 0) {
        printf("System:   %s\n", buffer.sysname);
        printf("Node:     %s\n", buffer.nodename);
        printf("Release:  %s\n", buffer.release);
        printf("Machine:  %s\n", buffer.machine);
    }
    return 0;
}
```