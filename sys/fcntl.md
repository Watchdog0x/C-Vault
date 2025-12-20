# fcntl.h - File Control Options

The `fcntl.h` header defines several macros and functions for performing control operations on file descriptors.

## Facilities Overview

| Facility Category | Key Symbols | Description |
|-------------------|-------------|-------------|
| File Opening | open, creat | Opening and creating files or devices. |
| Descriptor Control| fcntl | Performing various operations on file descriptors. |
| File Status Flags | O_RDONLY, O_WRONLY, O_RDWR | Access modes for `open`. |
| Operation Flags | O_CREAT, O_APPEND, O_TRUNC | Creation and behavior flags for `open`. |

---

## Types & Variables

### Access Modes

- `O_RDONLY`: Open for reading only.
- `O_WRONLY`: Open for writing only.
- `O_RDWR`: Open for reading and writing.

### Creation Flags

- `O_CREAT`: Create file if it does not exist.
- `O_EXCL`: Error if `O_CREAT` and file exists.
- `O_TRUNC`: Truncate length to 0 if file exists and is regular.

### Status Flags

- `O_APPEND`: The file is opened in append mode.
- `O_NONBLOCK`: Do not block on open or subsequent I/O.
- `O_SYNC`: Write operations are performed synchronously.

---

## Functions

### File Opening

#### open

```c
int open(const char *pathname, int flags, ...)
```

Opens the file specified by `pathname`. If `O_CREAT` is specified in `flags`, a third argument `mode_t mode` must be provided to specify permissions.

**Example:**

```c
#include <fcntl.h>
#include <unistd.h>
#include <stdio.h>

int main(void) {
    int fd = open("test.txt", O_CREAT | O_WRONLY | O_TRUNC, 0644);
    if (fd != -1) {
        write(fd, "Hello Linux!\n", 13);
        close(fd);
    }
    return 0;
}
```

### Descriptor Control

#### fcntl

```c
int fcntl(int fd, int cmd, ...)
```

Performs one of various operations on the open file descriptor `fd`.

**Common Commands:**
- `F_GETFL`: Get file status flags.
- `F_SETFL`: Set file status flags.
- `F_DUPFD`: Duplicate file descriptor.

**Example:**

```c
#include <fcntl.h>
#include <stdio.h>
#include <unistd.h>

int main(void) {
    int fd = open("log.txt", O_RDONLY);
    if (fd == -1) return 1;
    int flags = fcntl(fd, F_GETFL, 0);
    
    if (flags & O_APPEND) {
        printf("File is in append mode\n");
    }
    close(fd);
    return 0;
}
```