# threads.h - C11 Threads

The `threads.h` header (C11) provides a standard facility for multithreading.

## Facilities Overview

| Facility Category | Key Symbols | Description |
|-------------------|-------------|-------------|
| Thread Control | thrd_create, thrd_join, thrd_exit | Thread management. |
| Synchronization | mtx_lock, mtx_unlock, cnd_wait, cnd_signal | Mutexes and condition variables. |

---

## Types & Variables

### thrd_t

An opaque type representing a thread.

### mtx_t

An opaque type representing a mutex.

### cnd_t

An opaque type representing a condition variable.

---

## Functions

### Thread Control

#### thrd_create

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

#### thrd_join

```c
int thrd_join(thrd_t thr, int *res)
```

Blocks until the thread `thr` terminates.

---

### Synchronization

#### mtx_lock

```c
int mtx_lock(mtx_t *mtx)
```

Locks a mutex to protect a critical section.

#### mtx_unlock

```c
int mtx_unlock(mtx_t *mtx)
```

Unlocks a mutex.