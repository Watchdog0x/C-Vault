# signal.h - Signals

**Category:** 📂 std/ (C Standard Library)  
**Header:** `<signal.h>`  
**Scope:** C89, C99, C11

The `signal.h` header declares a type and two functions and defines several macros, for handling various signals during program execution.

## Facilities Overview

| Facility Category | Key Symbols | Description |
|-------------------|-------------|-------------|
| [Signal Handling](#signal-handling-functions) | [signal](#signal), [raise](#raise) | Registering handlers and sending signals. |
| [Standard Signals](#standard-signals) | SIGABRT, SIGFPE, SIGILL, SIGINT, SIGSEGV, SIGTERM | Predefined signal types. |
| [Signal Actions](#signal-actions) | SIG_DFL, SIG_IGN | Default and ignore actions for signals. |

---

## Types & Variables

### sig_atomic_t

```c
typedef int sig_atomic_t;
```

An integer type that can be accessed as an atomic entity, even in the presence of asynchronous signals. Commonly `int` or `volatile int`.

### Signal Actions

### SIG_DFL

```c
#define SIG_DFL /* implementation-defined */
```

Requests the default signal handling action.

### SIG_IGN

```c
#define SIG_IGN /* implementation-defined */
```

Requests that the signal be ignored.

---

## Functions

### Signal Handling Functions

### signal

```c
void (*signal(int sig, void (*func)(int)))(int)
```

Sets a handler for signal `sig`. The `func` can be a pointer to a function, or one of the macros `SIG_DFL` (default action) or `SIG_IGN` (ignore signal).

**Returns:** The previous value of the signal handler, or `SIG_ERR` on error (sets `errno`).

<details><summary>Example</summary>

```c
#include <signal.h>
#include <stdio.h>
#include <stdlib.h>
#include <errno.h>

void handler(int sig) {
    printf("Caught signal %d\n", sig);
    exit(0);
}

int main(void) {
    // Always check if signal registration failed
    if (signal(SIGINT, handler) == SIG_ERR) {
        perror("Failed to register SIGINT handler");
        return 1;
    }
    
    if (signal(SIGTERM, SIG_IGN) == SIG_ERR) {
        perror("Failed to ignore SIGTERM");
        return 1;
    }
    
    printf("Waiting for signals...\n");
    while(1); 
    return 0;
}
```

</details>

### raise

```c
int raise(int sig)
```

Sends the signal `sig` to the executing program.

**Returns:** `0` if successful, non-zero if unsuccessful.

<details><summary>Example</summary>

```c
#include <signal.h>
#include <stdio.h>

void handler(int sig) {
    printf("Signal %d raised\n", sig);
}

int main(void) {
    if (signal(SIGUSR1, handler) == SIG_ERR) {
        perror("signal");
        return 1;
    }

    if (raise(SIGUSR1) != 0) {
        fprintf(stderr, "Failed to raise signal\n");
        return 1;
    }
    
    return 0;
}
```

</details>

---

### Standard Signals

### SIGABRT

```c
#define SIGABRT /* implementation-defined */
```

Abnormal termination signal, such as initiated by `abort()`.

### SIGFPE

```c
#define SIGFPE /* implementation-defined */
```

Erroneous arithmetic operation (e.g., division by zero).

### SIGILL

```c
#define SIGILL /* implementation-defined */
```

Invalid executable code.

### SIGINT

```c
#define SIGINT /* implementation-defined */
```

Interactive attention signal (usually Ctrl+C).

### SIGSEGV

```c
#define SIGSEGV /* implementation-defined */
```

Invalid access to storage (segmentation violation).

### SIGTERM

```c
#define SIGTERM /* implementation-defined */
```

Termination request sent to the program.
