# unistd.h - Standard Symbolic Constants and Types

**Category:** 📂 sys/ (Linux & POSIX API)
**Header:** `<unistd.h>`
**Scope:** POSIX.1-2008

The `unistd.h` header defines miscellaneous symbolic constants and types, and declares functions that provide access to the POSIX operating system API.

## Facilities Overview

| Facility Category | Key Symbols | Description |
|-------------------|-------------|-------------|
| [File Operations](#file-operations) | [read](#read), [write](#write), [close](#close), [unlink](#unlink), [lseek](#lseek) | Low-level I/O and file management. |
| [Process Control](#process-control) | [fork](#fork), [exec](#exec-family), [exit](#_exit), [getpid](#getpid) | Creating and managing processes. |
| [Directory Ops](#directory-operations) | [getcwd](#getcwd), [chdir](#chdir), [rmdir](#rmdir) | Directory navigation and modification. |
| [System Configuration](#system-configuration) | [sysconf](#sysconf), [pathconf](#pathconf) | Querying system limits at runtime. |
| [Sleep](#sleep) | [sleep](#sleep), [usleep](#usleep) | Suspending execution. |

---

## Types & Variables

### ssize_t

```c
typedef long ssize_t;
```

Signed integer type used for a count of bytes or an error indication.

### pid_t

```c
typedef int pid_t;
```

Signed integer type used for process IDs.

### Standard File Descriptors

* **`STDIN_FILENO`** (0): Standard input.
* **`STDOUT_FILENO`** (1): Standard output.
* **`STDERR_FILENO`** (2): Standard error.

---

## Functions

### File Operations

### read

```c
ssize_t read(int fd, void *buf, size_t count)
```

Attempts to read up to `count` bytes from file descriptor `fd` into the buffer starting at `buf`.

**Returns:** The number of bytes read on success. Returns `0` on EOF. Returns `-1` on error and sets `errno`.

<details><summary>Example</summary>

```c
#include <unistd.h>
#include <stdio.h>
#include <stdlib.h>

int main(void) {
    char buffer[128];
    ssize_t bytes = read(STDIN_FILENO, buffer, sizeof(buffer) - 1);
    
    if (bytes == -1) {
        perror("read failed");
        return 1;
    }
    
    buffer[bytes] = '\0'; // Null-terminate
    printf("Read %zd bytes: %s", bytes, buffer);
    return 0;
}
```

</details>

### write

```c
ssize_t write(int fd, const void *buf, size_t count)
```

Writes up to `count` bytes from the buffer starting at `buf` to the file referred to by the file descriptor `fd`.

**Returns:** The number of bytes written on success. Returns `-1` on error and sets `errno`.

<details><summary>Example</summary>

```c
#include <unistd.h>
#include <stdio.h>

int main(void) {
    const char *msg = "Hello Kernel\n";
    // Write directly to stdout (file descriptor 1)
    if (write(STDOUT_FILENO, msg, 13) == -1) {
        perror("write failed");
        return 1;
    }
    return 0;
}
```

</details>

### close

```c
int close(int fd)
```

Closes a file descriptor, so that it no longer refers to any file and may be reused.

**Returns:** `0` on success. Returns `-1` on error (e.g., `EBADF`) and sets `errno`.

### unlink

```c
int unlink(const char *pathname)
```

Deletes a name from the filesystem. If that name was the last link to a file and no processes have the file open, the file is deleted.

**Returns:** `0` on success. Returns `-1` on error and sets `errno`.

### lseek

```c
off_t lseek(int fd, off_t offset, int whence)
```

Repoisitions the file offset of the description associated with `fd`.
`whence`: `SEEK_SET`, `SEEK_CUR`, or `SEEK_END`.

**Returns:** The resulting offset location from the beginning of the file. Returns `(off_t)-1` on error.

---

### Process Control

### fork

```c
pid_t fork(void)
```

Creates a new process by duplicating the calling process. The new process is the child; the calling process is the parent.

**Returns:**
* On success: PID of the child in the parent, and `0` in the child.
* On failure: `-1` in the parent, no child is created, and `errno` is set.

<details><summary>Example</summary>

```c
#include <unistd.h>
#include <stdio.h>

int main(void) {
    pid_t pid = fork();
    
    if (pid == -1) {
        perror("fork failed");
        return 1;
    } else if (pid == 0) {
        printf("Child process (PID: %d)\n", getpid());
    } else {
        printf("Parent process created child %d\n", pid);
    }
    return 0;
}
```

</details>

### exec family

```c
int execl(const char *path, const char *arg, ...)
int execv(const char *path, char *const argv[])
```

Replaces the current process image with a new process image.

**Returns:** Does **not** return on success. Returns `-1` on error and sets `errno`.

<details><summary>Example</summary>

```c
#include <unistd.h>
#include <stdio.h>
#include <stdlib.h>

int main(void) {
    char *args[] = {"/bin/ls", "-l", NULL};
    execv(args[0], args);
    
    // If execv returns, it failed
    perror("execv failed");
    return 1;
}
```

</details>

### getpid

```c
pid_t getpid(void)
```

Returns the process ID (PID) of the calling process.

---

### Directory Operations

### getcwd

```c
char *getcwd(char *buf, size_t size)
```

Returns a null-terminated string containing an absolute pathname that is the current working directory.

**Returns:** `buf` on success. Returns `NULL` on error (e.g. buffer too small) and sets `errno`.

<details><summary>Example</summary>

```c
#include <unistd.h>
#include <stdio.h>
#include <limits.h>

int main(void) {
    char cwd[PATH_MAX];
    if (getcwd(cwd, sizeof(cwd)) != NULL) {
        printf("Current dir: %s\n", cwd);
    } else {
        perror("getcwd failed");
    }
    return 0;
}
```

</details>

### chdir

```c
int chdir(const char *path)
```

Changes the current working directory of the calling process to the directory specified in `path`.

**Returns:** `0` on success. Returns `-1` on error.

---

### Sleep

### sleep

```c
unsigned int sleep(unsigned int seconds)
```

Causes the calling thread to sleep either until the number of real-time seconds specified in `seconds` have elapsed or until a signal arrives.

**Returns:** Zero if the requested time has elapsed, or the number of seconds left to sleep if interrupted.