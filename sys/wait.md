# sys/wait.h - Process Wait Operations

The `sys/wait.h` header defines functions for waiting for a child process to change state.

## Facilities Overview

| Facility Category | Key Symbols | Description |
|-------------------|-------------|-------------|
| Waiting | wait, waitpid | Blocking until a child terminates or stops. |
| Status Macros | WIFEXITED, WEXITSTATUS | Interpreting the status returned by `wait`. |

---

## Types & Variables

### Status Macros

Used to interpret the `int status` value filled by `wait` or `waitpid`.

- `WIFEXITED(status)`: True if child terminated normally.
- `WEXITSTATUS(status)`: Returns exit code of child.
- `WIFSIGNALED(status)`: True if child was terminated by a signal.
- `WTERMSIG(status)`: Returns signal number that caused termination.

---

## Functions

### Waiting Functions

#### wait

```c
pid_t wait(int *status)
```

Suspends execution of the calling process until one of its children terminates.

#### waitpid

```c
pid_t waitpid(pid_t pid, int *status, int options)
```

Suspends execution of the calling process until a child specified by `pid` argument has changed state.

**Example:**

```c
#include <sys/wait.h>
#include <unistd.h>
#include <stdio.h>

int main(void) {
    pid_t pid = fork();
    if (pid == 0) {
        return 42;
    }
    int status;
    waitpid(pid, &status, 0);
    if (WIFEXITED(status)) {
        printf("Child exited with: %d\n", WEXITSTATUS(status));
    }
    return 0;
}
```