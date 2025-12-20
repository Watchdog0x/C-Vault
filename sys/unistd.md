# unistd.h - Standard Symbolic Constants and Types

The `unistd.h` header defines miscellaneous symbolic constants and types, and declares functions that provide access to the POSIX operating system API.

## Facilities Overview

| Facility Category | Key Symbols | Description |
|-------------------|-------------|-------------|
| File Operations | read, write, close, lseek, unlink | Low-level I/O and file management. |
| Process Control | fork, exec, pipe, dup, dup2 | Creating and managing processes. |
| Process Info | getpid, getppid, getuid, getgid | Retrieving process and user identifiers. |
| Navigation | chdir, getcwd | Directory and path manipulation. |
| Miscellaneous | sleep, usleep, sysconf | Delaying execution and querying system limits. |

---

## Types & Variables

### pid_t, uid_t, gid_t

Integer types used for process IDs, user IDs, and group IDs.

### ssize_t, size_t

Signed and unsigned integer types used for sizes and counts of bytes.

### STDIN_FILENO, STDOUT_FILENO, STDERR_FILENO

Standard file descriptors for standard input (0), standard output (1), and standard error (2).

---

## Functions

### File Operations

#### read

```c
ssize_t read(int fd, void *buf, size_t count)
```

Attempts to read up to `count` bytes from file descriptor `fd` into the buffer starting at `buf`.

#### write

```c
ssize_t write(int fd, const void *buf, size_t count)
```

Writes up to `count` bytes from the buffer starting at `buf` to the file referred to by the file descriptor `fd`.

#### close

```c
int close(int fd)
```

**Example:**

```c
#include <unistd.h>
#include <stdio.h>

int main(void) {
    char buffer[128];
    ssize_t bytes = read(STDIN_FILENO, buffer, sizeof(buffer) - 1);
    if (bytes > 0) {
        buffer[bytes] = '\0';
        printf("Read: %s", buffer);
    }
    return 0;
}
```

### Process Control

#### fork

```c
pid_t fork(void)
```

Creates a new process by duplicating the calling process. Returns 0 in the child and the PID of the child in the parent.

#### exec

```c
int execl(const char *path, const char *arg, ...)
int execv(const char *path, char *const argv[])
```

Replaces the current process image with a new process image.

**Example:**

```c
#include <unistd.h>
#include <stdio.h>

int main(void) {
    pid_t pid = fork();
    if (pid == 0) {
        printf("Child process (PID: %d)\n", getpid());
    } else if (pid > 0) {
        printf("Parent process of child %d\n", pid);
    }
    return 0;
}
```

### Navigation

#### getcwd

```c
char *getcwd(char *buf, size_t size)
```

Returns a null-terminated string containing an absolute pathname that is the current working directory.

**Example:**

```c
#include <unistd.h>
#include <stdio.h>

int main(void) {
    char cwd[1024];
    if (getcwd(cwd, sizeof(cwd)) != NULL) {
        printf("Current dir: %s\n", cwd);
    }
    return 0;
}
```