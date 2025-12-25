# SDL2/SDL.h - Simple DirectMedia Layer

**Category:** 📂 sdl/ (Graphics & Display)
**Header:** `<SDL2/SDL.h>`
**Scope:** Multimedia / Windowing

The `SDL2/SDL.h` header is the main entry point for SDL2. It is a "meta-header" that includes definitions for all SDL subsystems (Video, Audio, Input, Timers, Threading, etc.).

## Facilities Overview

| Facility Category | Key Symbols | Description |
|-------------------|-------------|-------------|
| [Initialization](#initialization) | [SDL_Init](#sdl_init), [SDL_Quit](#sdl_quit) | Library startup and shutdown. |
| [Window Mgmt](#window-management) | [SDL_CreateWindow](#sdl_createwindow) | Creating application windows. |
| [Rendering](#rendering) | [SDL_CreateRenderer](#sdl_createrenderer) | Hardware-accelerated 2D graphics. |
| [Input Events](#event-handling) | [SDL_PollEvent](#sdl_pollevent) | Processing event queues (keyboard, mouse, quit). |
| [Input State](#input-state) | [SDL_GetKeyboardState](#sdl_getkeyboardstate) | polling current hardware state directly. |
| [Time](#time-management) | [SDL_GetTicks](#sdl_getticks), [SDL_Delay](#sdl_delay) | Game loops and delays. |
| [Geometry](#geometry-types) | [SDL_Rect](#sdl_rect), [SDL_Point](#sdl_point) | 2D structure definitions. |

> [!NOTE]
> **Other Modules**: `SDL.h` also includes support for **Audio** (`SDL_audio.h`), **Threading** (`SDL_thread.h`), **Game Controllers** (`SDL_gamecontroller.h`), and **File I/O** (`SDL_rwops.h`).

---

## Types & Variables

### Geometry Types

#### SDL_Rect

```c
typedef struct SDL_Rect {
    int x, y;
    int w, h;
} SDL_Rect;
```

A rectangle, with the origin at the upper left.

#### SDL_Point

```c
typedef struct SDL_Point {
    int x, y;
} SDL_Point;
```

A 2D point.

### Opaque Handles

*   **`SDL_Window`**: Handle to a window.
*   **`SDL_Renderer`**: Handle to a rendering context.
*   **`SDL_Texture`**: Handle to an efficient image representation.
*   **`SDL_Surface`**: Handle to a raw CPU-side image buffer.

### SDL_Event

```c
typedef union SDL_Event SDL_Event;
```

A union containing event-specific structures (e.g., `SDL_KeyboardEvent`, `SDL_MouseButtonEvent`).

---

## Functions

### Initialization

### SDL_Init

```c
int SDL_Init(Uint32 flags)
```

Initializes the library.
*   `flags`: `SDL_INIT_VIDEO`, `SDL_INIT_AUDIO`, `SDL_INIT_JOYSTICK`, `SDL_INIT_EVERYTHING`.

**Returns:** `0` on success, `<0` on error.

### SDL_Quit

```c
void SDL_Quit(void)
```

Cleans up all initialized subsystems.

---

### Window Management

### SDL_CreateWindow

```c
SDL_Window *SDL_CreateWindow(const char *title, int x, int y, int w, int h, Uint32 flags)
```

Create a window.
*   `flags`: `SDL_WINDOW_SHOWN`, `SDL_WINDOW_BORDERLESS`, `SDL_WINDOW_FULLSCREEN`.

**Returns:** Pointer to window or `NULL`.

### SDL_DestroyWindow

```c
void SDL_DestroyWindow(SDL_Window *window)
```

Destroy a window.

---

### Rendering

### SDL_CreateRenderer

```c
SDL_Renderer *SDL_CreateRenderer(SDL_Window *window, int index, Uint32 flags)
```

Create a 2D rendering context.
*   `flags`: `SDL_RENDERER_ACCELERATED`, `SDL_RENDERER_PRESENTVSYNC`.

### SDL_SetRenderDrawColor

```c
int SDL_SetRenderDrawColor(SDL_Renderer *renderer, Uint8 r, Uint8 g, Uint8 b, Uint8 a)
```

Set the color for drawing operations.

### SDL_RenderClear

```c
int SDL_RenderClear(SDL_Renderer *renderer)
```

Clear the screen with the current draw color.

### SDL_RenderFillRect

```c
int SDL_RenderFillRect(SDL_Renderer *renderer, const SDL_Rect *rect)
```

Fill a rectangle with the current draw color.

### SDL_RenderPresent

```c
void SDL_RenderPresent(SDL_Renderer *renderer)
```

Update the screen.

---

### Input State

### SDL_GetKeyboardState

```c
const Uint8 *SDL_GetKeyboardState(int *numkeys)
```

Get a snapshot of the current state of the keyboard.

*   `numkeys`: If non-NULL, receives the length of the returned array.

**Returns:** A pointer to an internal array of key states. Check values with `SDL_SCANCODE_*`.

<details><summary>Example</summary>

```c
const Uint8 *state = SDL_GetKeyboardState(NULL);
if (state[SDL_SCANCODE_RIGHT]) {
    // Move character right
}
```

</details>

### SDL_GetMouseState

```c
Uint32 SDL_GetMouseState(int *x, int *y)
```

Retrieve the current state of the mouse.

**Returns:** A bitmask of the current button state (`SDL_BUTTON(X)`).

---

### Time Management

### SDL_GetTicks

```c
Uint32 SDL_GetTicks(void)
```

Returns the number of milliseconds since the SDL library initialization. Useful for frame timing.

### SDL_Delay

```c
void SDL_Delay(Uint32 ms)
```

Wait a specified number of milliseconds before returning.

---

### Event Handling

### SDL_PollEvent

```c
int SDL_PollEvent(SDL_Event *event)
```

Polls for currently pending events.

**Returns:** `1` if event pending, `0` otherwise.

---

### Error Handling

### SDL_GetError

```c
const char *SDL_GetError(void)
```

Retrieve a message about the last error.

<details><summary>Example: Full Application</summary>

```c
#include <SDL2/SDL.h>
#include <stdio.h>

int main(void) {
    if (SDL_Init(SDL_INIT_VIDEO) < 0) return 1;

    SDL_Window *win = SDL_CreateWindow("Game Loop", 
        SDL_WINDOWPOS_CENTERED, SDL_WINDOWPOS_CENTERED, 800, 600, 0);
    SDL_Renderer *ren = SDL_CreateRenderer(win, -1, SDL_RENDERER_ACCELERATED);

    int running = 1;
    SDL_Event e;
    
    // Main Loop
    while (running) {
        Uint32 start = SDL_GetTicks();

        // Input
        while (SDL_PollEvent(&e)) {
            if (e.type == SDL_QUIT) running = 0;
        }

        // Update
        const Uint8 *currKeyStates = SDL_GetKeyboardState(NULL);
        if (currKeyStates[SDL_SCANCODE_SPACE]) {
            printf("Space held down!\n");
        }

        // Render
        SDL_SetRenderDrawColor(ren, 0, 0, 0, 255); // Black
        SDL_RenderClear(ren);

        SDL_Rect r = { 50, 50, 100, 100 };
        SDL_SetRenderDrawColor(ren, 255, 0, 255, 255); // Magenta
        SDL_RenderFillRect(ren, &r);

        SDL_RenderPresent(ren);
        
        // Frame limiting (approx 60 FPS)
        Uint32 elapsed = SDL_GetTicks() - start;
        if (elapsed < 16) SDL_Delay(16 - elapsed);
    }
    
    SDL_DestroyRenderer(ren);
    SDL_DestroyWindow(win);
    SDL_Quit();
    return 0;
}
```

</details>