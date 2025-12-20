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

#### SIG_DFL

```c
#define SIG_DFL /* implementation-defined */
```

Requests the default signal handling action.

#### SIG_IGN

```c
#define SIG_IGN /* implementation-defined */
```

Requests that the signal be ignored.

---

## Functions

### Signal Handling Functions

#### signal

```c
void (*signal(int sig, void (*func)(int)))(int)
```

Sets a handler for signal `sig`. The `func` can be a pointer to a function, or one of the macros `SIG_DFL` (default action) or `SIG_IGN` (ignore signal).

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
    signal(SIGINT, handler); // Register handler for SIGINT
    signal(SIGTERM, SIG_IGN); // Ignore SIGTERM
    while(1); // Wait for signals
    return 0;
}
```

#### raise

```c
int raise(int sig)
```

Sends the signal `sig` to the executing program. Returns 0 if successful.

**Example:**

```c
#include <signal.h>
#include <stdio.h>

void handler(int sig) {
    printf("Signal %d raised\n", sig);
}

int main(void) {
    signal(SIGUSR1, handler);
    raise(SIGUSR1); // Send signal to self
    return 0;
}
```

---

### Standard Signals

#### SIGABRT

```c
#define SIGABRT /* implementation-defined */
```

Abnormal termination signal, such as initiated by `abort()`.

#### SIGFPE

```c
#define SIGFPE /* implementation-defined */
```

Erroneous arithmetic operation (e.g., division by zero).

#### SIGILL

```c
#define SIGILL /* implementation-defined */
```

Invalid executable code.

#### SIGINT

```c
#define SIGINT /* implementation-defined */
```

Interactive attention signal (usually Ctrl+C).

#### SIGSEGV

```c
#define SIGSEGV /* implementation-defined */
```

Invalid access to storage (segmentation violation).

#### SIGTERM

```c
#define SIGTERM /* implementation-defined */
```

Termination request sent to the program.
