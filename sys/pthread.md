# pthread.h - POSIX Threads

The `pthread.h` header defines the interface for creating and managing threads.

## Facilities Overview

| Facility Category | Key Symbols | Description |
|-------------------|-------------|-------------|
| Thread Management | pthread_create, pthread_join, pthread_detach | Lifecycle of concurrent execution units. |
| Mutexes | pthread_mutex_lock, pthread_mutex_unlock | Mutual exclusion for protecting shared resources. |
| Condition Variables| pthread_cond_wait, pthread_cond_signal | Signaling between threads. |
| Attributes | pthread_attr_init, pthread_attr_destroy | Configuring thread behavior. |

---

## Types & Variables

### pthread_t

An opaque type representing a thread.

### pthread_mutex_t

An opaque type used for mutual exclusion.

---

## Functions

### Thread Management

#### pthread_create

```c
int pthread_create(pthread_t *thread, const pthread_attr_t *attr, void *(*start_routine)(void *), void *arg)
```

**Example:**

```c
#include <pthread.h>

void *run(void *arg) { return NULL; }

int main(void) {
    pthread_t thread;
    pthread_create(&thread, NULL, run, NULL);
    pthread_join(thread, NULL);
    return 0;
}
```

#### pthread_join

```c
int pthread_join(pthread_t thread, void **retval)
```

### Mutexes (Mutual Exclusion)

#### pthread_mutex_lock

```c
int pthread_mutex_lock(pthread_mutex_t *mutex)
```

#### pthread_mutex_unlock

```c
int pthread_mutex_unlock(pthread_mutex_t *mutex)
```