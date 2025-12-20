# dirent.h - Directory Entries

The `dirent.h` header defines functions for directory stream operations.

## Facilities Overview

| Facility Category | Key Symbols | Description |
|-------------------|-------------|-------------|
| Streams | opendir, readdir, closedir | Opening and reading directory contents. |
| Positioning | rewinddir, seekdir, telldir | Navigating directory streams. |

---

## Types & Variables

### DIR

An opaque type representing a directory stream.

### struct dirent

Structure containing information about a directory entry.

---

## Functions

### Directory Streams

#### opendir

```c
DIR *opendir(const char *name)
```

Opens a directory stream.

**Example:**

```c
#include <dirent.h>

int main(void) {
    DIR *d = opendir(".");
    if (d) closedir(d);
    return 0;
}
```

#### readdir

```c
struct dirent *readdir(DIR *dirp)
```

#### closedir

```c
int closedir(DIR *dirp)
```