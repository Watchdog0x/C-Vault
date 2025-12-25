# SDL2/SDL_thread.h - Threading Primitives

**Category:** 📂 sdl/ (Graphics & Display)
**Header:** `<SDL2/SDL_thread.h>`
**Scope:** Multithreading & Synchronization

The `SDL_thread.h` header provides cross-platform primitives for thread management, mutexes, semaphores, and condition variables.

## Facilities Overview

| Facility Category | Key Symbols | Description |
|-------------------|-------------|-------------|
| [Threads](#threads) | [SDL_CreateThread](#sdl_createthread), [SDL_WaitThread](#sdl_waitthread) | Creating and managing threads. |
| [Mutexes](#mutexes) | [SDL_CreateMutex](#sdl_createmutex), [SDL_LockMutex](#sdl_lockmutex) | Mutual exclusion locks. |
| [Condition Vars](#condition-variables) | [SDL_CondWait](#sdl_condwait), [SDL_CondSignal](#sdl_condsignal) | Signaling between threads. |
| [Semaphores](#semaphores) | [SDL_SemWait](#sdl_semwait), [SDL_SemPost](#sdl_sempost) | Counting semaphores. |

---

## Types & Variables

### SDL_Thread

```c
typedef struct SDL_Thread SDL_Thread;
```

Opaque handle to a thread.

### SDL_mutex

```c
typedef struct SDL_mutex SDL_mutex;
```

Opaque handle to a mutex.

---

## Functions

### Threads

### SDL_CreateThread

```c
SDL_Thread *SDL_CreateThread(SDL_ThreadFunction fn, const char *name, void *data)
```

Create a new thread.
*   `fn`: Function to call (`int (*)(void *)`).
*   `name`: Thread name (for debugging).
*   `data`: Pointer passed to the function.

**Returns:** Opaque pointer to the new thread object, or NULL on error.

### SDL_WaitThread

```c
void SDL_WaitThread(SDL_Thread *thread, int *status)
```

Wait for a thread to finish.

---

### Mutexes

### SDL_CreateMutex

```c
SDL_mutex *SDL_CreateMutex(void)
```

Create a new mutex.

### SDL_LockMutex

```c
int SDL_LockMutex(SDL_mutex *mutex)
```

Lock the mutex. Blocks until available.

### SDL_UnlockMutex

```c
int SDL_UnlockMutex(SDL_mutex *mutex)
```

Unlock the mutex.

<details><summary>Example: Threaded Counter</summary>

```c
#include <SDL2/SDL.h>
#include <stdio.h>

int counter = 0;
SDL_mutex *lock;

int thread_func(void *data) {
    for (int i = 0; i < 1000; i++) {
        SDL_LockMutex(lock);
        counter++;
        SDL_UnlockMutex(lock);
    }
    return 0;
}

int main(void) {
    SDL_Init(SDL_INIT_VIDEO); // initializes threading subsystem implicitly
    lock = SDL_CreateMutex();
    
    SDL_Thread *t1 = SDL_CreateThread(thread_func, "T1", NULL);
    SDL_Thread *t2 = SDL_CreateThread(thread_func, "T2", NULL);
    
    SDL_WaitThread(t1, NULL);
    SDL_WaitThread(t2, NULL);
    
    printf("Final counter: %d (expected 2000)\n", counter);
    
    SDL_DestroyMutex(lock);
    SDL_Quit();
    return 0;
}
```

</details>
