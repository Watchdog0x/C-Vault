# SDL2/SDL_timer.h - Time Management

**Category:** 📂 sdl/ (Graphics & Display)
**Header:** `<SDL2/SDL_timer.h>`
**Scope:** Timing & Delays

The `SDL_timer.h` header provides functions for getting the current time, waiting for a specified duration, and creating timer callbacks.

## Facilities Overview

| Facility Category | Key Symbols | Description |
|-------------------|-------------|-------------|
| [Core Timing](#core-functions) | [SDL_GetTicks](#sdl_getticks), [SDL_Delay](#sdl_delay) | Millisecond precision timing. |
| [Performance](#high-performance-counters) | [SDL_GetPerformanceCounter](#sdl_getperformancecounter) | High-resolution CPU ticks. |
| [Callbacks](#callbacks) | [SDL_AddTimer](#sdl_addtimer) | Executing code after a delay. |

---

## Functions

### Core Functions

### SDL_GetTicks

```c
Uint32 SDL_GetTicks(void)
```

Returns the number of milliseconds since the SDL library initialization. This value wraps if the program runs for more than ~49 days.

**Returns:** Milliseconds since init.

### SDL_Delay

```c
void SDL_Delay(Uint32 ms)
```

Wait a specified number of milliseconds before returning.
*   `ms`: Duration in milliseconds.

**Returns:** None.

---

### High Performance Counters

### SDL_GetPerformanceCounter

```c
Uint64 SDL_GetPerformanceCounter(void)
```

Get the current value of the high resolution counter.

**Returns:** Ticks.

### SDL_GetPerformanceFrequency

```c
Uint64 SDL_GetPerformanceFrequency(void)
```

Get the count per second of the high resolution counter.

**Returns:** Frequency in Hz.

<details><summary>Example: Profiling</summary>

```c
#include <SDL2/SDL.h>
#include <stdio.h>

int main(void) {
    if (SDL_Init(0) < 0) return 1;

    Uint64 start = SDL_GetPerformanceCounter();
    
    SDL_Delay(50); // Sleep for 50ms
    
    Uint64 end = SDL_GetPerformanceCounter();
    float seconds = (end - start) / (float)SDL_GetPerformanceFrequency();
    
    printf("Slept for %f seconds\n", seconds);
    
    SDL_Quit();
    return 0;
}
```

</details>

---

### Callbacks

### SDL_AddTimer

```c
SDL_TimerID SDL_AddTimer(Uint32 interval, SDL_TimerCallback callback, void *param)
```

Add a timer which will call `callback` after `interval` milliseconds.

**Returns:** Timer ID, or 0 on error.
