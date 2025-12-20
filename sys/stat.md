# sys/stat.h - File Status and Metadata

The `sys/stat.h` header defines the structure of data returned by the functions `fstat`, `lstat`, and `stat`.

## Facilities Overview

| Facility Category | Key Symbols | Description |
|-------------------|-------------|-------------|
| File Status | stat, fstat, lstat | Retrieving information about a file. |
| File Permissions | chmod, fchmod | Changing the access permissions of a file. |
| Directory Ops | mkdir | Creating new directories. |
| Macros | S_ISREG, S_ISDIR | Macros for checking file types. |

---

## Types & Variables

### struct stat

Contains metadata about a file.

---

## Functions

### File Status Functions

#### stat

```c
int stat(const char *pathname, struct stat *statbuf)
```

Retrieves information about a file.

**Example:**

```c
#include <sys/stat.h>
#include <stdio.h>

int main(void) {
    struct stat sb;
    if (stat(".", &sb) == 0) {
        printf("Inode: %ld\n", (long)sb.st_ino);
    }
    return 0;
}
```

#### fstat

```c
int fstat(int fd, struct stat *statbuf)
```

#### lstat

```c
int lstat(const char *pathname, struct stat *statbuf)
```

### Directory Operations

#### mkdir

```c
int mkdir(const char *pathname, mode_t mode)
```