# threads.h - C11 Threads

**Category:** 📂 std/ (C Standard Library)  
**Header:** `<threads.h>`  
**Scope:** C11

The `threads.h` header (C11) provides a standard facility for multithreading.

## Facilities Overview

| Facility Category | Key Symbols | Description |
|-------------------|-------------|-------------|
| [Thread Control](#thread-control) | [thrd_create](#thrd_create), [thrd_join](#thrd_join), [thrd_exit](#thrd_exit) | Thread management. |
| [Synchronization](#synchronization) | [mtx_lock](#mtx_lock), [mtx_unlock](#mtx_unlock), [cnd_wait](#cnd_wait), [cnd_signal](#cnd_signal) | Mutexes and condition variables. |

---

## Types & Variables

### thrd_t

An opaque type representing a thread.

### mtx_t

An opaque type representing a mutex.

### cnd_t

An opaque type representing a condition variable.

### thrd_start_t

```c
typedef int (*thrd_start_t)(void *)
```

Function pointer type for thread start functions.

### mtx_plain, mtx_timed, mtx_recursive

Mutex type flags for `mtx_init`.

### thrd_success, thrd_nomem, thrd_error

Return values for thread functions.

---

## Functions

### Thread Control

### thrd_create

```c
int thrd_create(thrd_t *thr, thrd_start_t func, void *arg)
```

Starts a new thread. Returns `thrd_success` on success.

**Example:**

```c
#include <threads.h>

int run(void *arg) { return 0; }

int main(void) {
    thrd_t t;
    if (thrd_create(&t, run, NULL) == thrd_success) {
        thrd_join(t, NULL);
    }
    return 0;
}
```

### thrd_join

```c
int thrd_join(thrd_t thr, int *res)
```

Blocks until the thread `thr` terminates. Stores the return value in `res` if not NULL.

**Example:**

```c
#include <threads.h>
#include <stdio.h>

int worker(void *arg) {
    printf("Worker thread\n");
    return 42;
}

int main(void) {
    thrd_t t;
    int result;
    thrd_create(&t, worker, NULL);
    thrd_join(t, &result);
    printf("Thread returned: %d\n", result);
    return 0;
}
```

### thrd_exit

```c
_Noreturn void thrd_exit(int res)
```

Terminates the calling thread with return value `res`.

**Example:**

```c
#include <threads.h>

int worker(void *arg) {
    thrd_exit(42); // Exit with code 42
    return 0; // Never reached
}
```

### thrd_detach

```c
int thrd_detach(thrd_t thr)
```

Detaches the thread `thr`, allowing it to run independently.

**Example:**

```c
#include <threads.h>

int run(void *arg) { return 0; }

int main(void) {
    thrd_t t;
    thrd_create(&t, run, NULL);
    thrd_detach(t); // Thread runs independently
    return 0;
}
```

### thrd_current

```c
thrd_t thrd_current(void)
```

Returns the thread ID of the calling thread.

**Example:**

```c
#include <threads.h>

int main(void) {
    thrd_t self = thrd_current();
    // Use self...
    return 0;
}
```

### thrd_equal

```c
int thrd_equal(thrd_t thr0, thrd_t thr1)
```

Checks if two thread IDs refer to the same thread.

**Example:**

```c
#include <threads.h>

int main(void) {
    thrd_t t1 = thrd_current();
    thrd_t t2 = thrd_current();
    if (thrd_equal(t1, t2)) {
        // Same thread
    }
    return 0;
}
```

### thrd_yield

```c
void thrd_yield(void)
```

Yields the execution of the current thread to another thread.

**Example:**

```c
#include <threads.h>

int worker(void *arg) {
    for (int i = 0; i < 100; i++) {
        // Do work
        thrd_yield(); // Allow other threads to run
    }
    return 0;
}
```

### thrd_sleep

```c
int thrd_sleep(const struct timespec *duration, struct timespec *remaining)
```

Makes the calling thread sleep for the given duration.

**Example:**

```c
#include <threads.h>
#include <time.h>

int main(void) {
    struct timespec ts = {1, 0}; // 1 second
    thrd_sleep(&ts, NULL);
    return 0;
}
```

---

### Synchronization

### mtx_init

```c
int mtx_init(mtx_t *mtx, int type)
```

Initializes a mutex with the given `type` (e.g., `mtx_plain`).

### mtx_destroy

```c
void mtx_destroy(mtx_t *mtx)
```

Destroys a mutex.

**Example:**

```c
#include <threads.h>

int main(void) {
    mtx_t mtx;
    mtx_init(&mtx, mtx_plain);
    // Use mutex...
    mtx_destroy(&mtx);
    return 0;
}
```

### mtx_trylock

```c
int mtx_trylock(mtx_t *mtx)
```

Tries to lock a mutex without blocking.

**Example:**

```c
#include <threads.h>

int main(void) {
    mtx_t mtx;
    mtx_init(&mtx, mtx_plain);
    if (mtx_trylock(&mtx) == thrd_success) {
        // Locked
        mtx_unlock(&mtx);
    }
    mtx_destroy(&mtx);
    return 0;
}
```

### mtx_lock

```c
int mtx_lock(mtx_t *mtx)
```

Locks a mutex to protect a critical section.

**Example:**

```c
#include <threads.h>

mtx_t mtx;
int shared = 0;

int increment(void *arg) {
    mtx_lock(&mtx);
    shared++;
    mtx_unlock(&mtx);
    return 0;
}

int main(void) {
    mtx_init(&mtx, mtx_plain);
    thrd_t t;
    thrd_create(&t, increment, NULL);
    thrd_join(t, NULL);
    mtx_destroy(&mtx);
    return 0;
}
```

### mtx_timedlock

```c
int mtx_timedlock(mtx_t *restrict mtx, const struct timespec *restrict ts)
```

Tries to lock a timed mutex, blocking until the timeout.

### mtx_unlock

```c
int mtx_unlock(mtx_t *mtx)
```

Unlocks a mutex.

### cnd_init

```c
int cnd_init(cnd_t *cond)
```

Initializes a condition variable.

### cnd_destroy

```c
void cnd_destroy(cnd_t *cond)
```

Destroys a condition variable.

### cnd_wait

```c
int cnd_wait(cnd_t *cond, mtx_t *mtx)
```

Waits on a condition variable, atomically unlocking the mutex.

**Example:**

```c
#include <threads.h>

int main(void) {
    cnd_t cnd;
    mtx_t mtx;
    cnd_init(&cnd);
    mtx_init(&mtx, mtx_plain);
    mtx_lock(&mtx);
    cnd_wait(&cnd, &mtx);
    mtx_unlock(&mtx);
    cnd_destroy(&cnd);
    mtx_destroy(&mtx);
    return 0;
}
```

### cnd_timedwait

```c
int cnd_timedwait(cnd_t *restrict cond, mtx_t *restrict mtx, const struct timespec *restrict ts)
```

Waits on a condition variable with a timeout.

### cnd_broadcast

```c
int cnd_broadcast(cnd_t *cond)
```

Signals all waiting threads on the condition variable.

### cnd_signal

```c
int cnd_signal(cnd_t *cond)
```

Signals one waiting thread on the condition variable.

**Example:**

```c
#include <threads.h>

int main(void) {
    cnd_t cnd;
    cnd_init(&cnd);
    cnd_signal(&cnd);
    cnd_destroy(&cnd);
    return 0;
}
```
