# SDL.h - Simple DirectMedia Layer

The `SDL.h` header is the entry point for SDL2, a cross-platform development library for audio, keyboard, mouse, and graphics.

## Facilities Overview

| Facility Category | Key Symbols | Description |
|-------------------|-------------|-------------|
| Initialization | SDL_Init, SDL_Quit | Managing library subsystems. |
| Video & Windowing| SDL_CreateWindow, SDL_DestroyWindow | Creating and managing display windows. |
| Rendering | SDL_CreateRenderer, SDL_RenderPresent| hardware-accelerated 2D graphics. |
| Events | SDL_PollEvent, SDL_Event | Handling user input and system messages. |
| Memory | SDL_malloc, SDL_free | Cross-platform memory allocation. |

---

## Types & Variables

### SDL_Window

Opaque handle to a window.

### SDL_Renderer

Hardware-accelerated rendering context.

### SDL_Event

A union structure representing different types of input and system events.

---

## Functions

### Initialization

#### SDL_Init

```c
int SDL_Init(Uint32 flags)
```

Initializes the SDL library.

**Example:**

```c
#include <SDL.h>

int main(void) {
    if (SDL_Init(SDL_INIT_VIDEO) < 0) {
        return 1;
    }
    SDL_Quit();
    return 0;
}
```

#### SDL_Quit

```c
void SDL_Quit(void)
```

Cleans up all initialized subsystems.

---

### Windowing and Rendering

#### SDL_CreateWindow

```c
SDL_Window *SDL_CreateWindow(const char *title, int x, int y, int w, int h, Uint32 flags)
```

Creates a window with the specified position, dimensions, and flags.

#### SDL_CreateRenderer

```c
SDL_Renderer *SDL_CreateRenderer(SDL_Window *window, int index, Uint32 flags)
```

Creates a 2D rendering context for a window.

---

### Events

#### SDL_PollEvent

```c
int SDL_PollEvent(SDL_Event *event)
```

Polls for currently pending events.