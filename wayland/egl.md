# wayland-egl.h - Wayland EGL Integration

**Category:** 📂 wayland/ (Wayland Protocol)
**Header:** `<wayland-egl.h>`
**Scope:** Wayland compositors using EGL

The `wayland-egl.h` header provides utilities for integrating EGL with Wayland surfaces, enabling OpenGL ES and OpenGL rendering in Wayland compositors. It serves as the glue layer between the Wayland protocol and the EGL platform windowing system.

## Facilities Overview

| Facility Category | Key Symbols | Description |
|-------------------|-------------|-------------|
| [Window Creation](#window-creation) | [wl_egl_window_create](#wl_egl_window_create), [wl_egl_window_destroy](#wl_egl_window_destroy) | Creating EGL windows from Wayland surfaces. |
| [Window Management](#window-management) | [wl_egl_window_resize](#wl_egl_window_resize), [wl_egl_window_get_attached_size](#wl_egl_window_get_attached_size) | Resizing and querying EGL windows. |

---

## Types & Variables

### struct wl_egl_window

```c
struct wl_egl_window; 
```

An opaque structure representing an native EGL window (`EGLNativeWindowType`) backed by a Wayland surface.

---

## Functions

### Window Creation

### wl_egl_window_create

```c
struct wl_egl_window *wl_egl_window_create(struct wl_surface *surface, int width, int height)
```

Creates an EGL window from a Wayland surface. This object is castable to `EGLNativeWindowType` for use with `eglCreateWindowSurface`.

**Returns:** A pointer to a new `wl_egl_window` on success, or `NULL` on failure (e.g. allocation error).

<details><summary>Example</summary>

```c
#include <wayland-client.h>
#include <wayland-egl.h>
#include <EGL/egl.h>
#include <stdio.h>

int main(void) {
    // Assume we have a valid wl_surface *surface
    
    // Create EGL window (800x600)
    struct wl_egl_window *egl_window = wl_egl_window_create(surface, 800, 600);
    if (!egl_window) {
        fprintf(stderr, "Failed to create EGL window\n");
        return 1;
    }

    // Pass to EGL
    // EGLSurface win = eglCreateWindowSurface(dpy, config, (EGLNativeWindowType)egl_window, NULL);

    // Cleanup
    wl_egl_window_destroy(egl_window);
    return 0;
}
```

</details>

### wl_egl_window_destroy

```c
void wl_egl_window_destroy(struct wl_egl_window *egl_window)
```

Destroys an EGL window and frees associated resources.

**Returns:** None.

### Window Management

### wl_egl_window_resize

```c
void wl_egl_window_resize(struct wl_egl_window *egl_window, int width, int height, int dx, int dy)
```

Resizes an EGL window. The `dx` and `dy` parameters specify the offset within the buffer that the content should be moved to (often 0,0).

**Returns:** None.

<details><summary>Example</summary>

```c
#include <wayland-egl.h>

void handle_resize(struct wl_egl_window *egl_window, int new_width, int new_height) {
    // Resize the underlying EGL window buffer
    wl_egl_window_resize(egl_window, new_width, new_height, 0, 0);
    
    // Note: GL viewport usually needs to be updated separately
}
```

</details>

### wl_egl_window_get_attached_size

```c
void wl_egl_window_get_attached_size(struct wl_egl_window *egl_window, int *width, int *height)
```

Retrieves the size of the buffer currently attached to the surface.

**Returns:** None (output via pointers).

<details><summary>Example</summary>

```c
#include <wayland-egl.h>
#include <stdio.h>

void check_size(struct wl_egl_window *egl_window) {
    int width, height;
    wl_egl_window_get_attached_size(egl_window, &width, &height);
    printf("Attached size: %dx%d\n", width, height);
}
```

</details>
