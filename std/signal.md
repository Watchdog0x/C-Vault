# signal.h - Signals

The `signal.h` header declares a type and two functions and defines several macros, for handling various signals during program execution.

## Facilities Overview

| Facility Category | Key Symbols | Description |
|-------------------|-------------|-------------|
| Signal Handling | signal, raise | Registering handlers and sending signals. |
| Standard Signals | SIGABRT, SIGFPE, SIGILL, SIGINT, SIGSEGV, SIGTERM | Predefined signal types. |

---

## Types & Variables

### sig_atomic_t

An integer type that can be accessed as an atomic entity, even in the presence of asynchronous signals.


## Functions

### Signal Handling Functions

#### signal

```c
void (*signal(int sig, void (*func)(int)))(int)
```

Sets a handler for signal `sig`. The `func` can be a pointer to a function, or one of the macros `SIG_DFL` (default action) or `SIG_IGN` (ignore signal).

#### raise

```c
int raise(int sig)
```

Sends the signal `sig` to the executing program. Returns 0 if successful.

**Example:**

```c
#include <signal.h>
#include <stdio.h>
#include <stdlib.h>

void handler(int sig) {
    printf("Caught signal %d\n", sig);
    exit(0);
}

int main(void) {
    signal(SIGINT, handler);
    // raise(SIGINT); 
    while(1); // Wait for Ctrl+C
    return 0;
}
```

### Standard Signals

- `SIGABRT`: Abnormal termination signal.
- `SIGFPE`: Erroneous arithmetic operation (e.g., division by zero).
- `SIGSEGV`: Invalid access to storage (segmentation violation).
- `SIGINT`: Interactive attention signal (usually Ctrl+C).
- `SIGILL`: Invalid executable code.
- `SIGTERM`: Termination request sent to the program.