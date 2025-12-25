# wayland-cursor.h - Wayland Cursor Management

**Category:** 📂 wayland/ (Wayland Protocol)
**Header:** `<wayland-cursor.h>`
**Scope:** Wayland clients

The `wayland-cursor.h` header provides utilities for loading and managing cursor themes. It simplifies the process of finding cursor images, loading them into shared memory, and animating them.

## Facilities Overview

| Facility Category | Key Symbols | Description |
|-------------------|-------------|-------------|
| [Theme Loading](#theme-management) | [wl_cursor_theme_load](#wl_cursor_theme_load), [wl_cursor_theme_destroy](#wl_cursor_theme_destroy) | Loading cursor themes from the filesystem. |
| [Cursor Access](#theme-management) | [wl_cursor_theme_get_cursor](#wl_cursor_theme_get_cursor) | Retrieving specific cursors from a theme. |
| [Image Management](#cursor-images) | [wl_cursor_image_get_buffer](#wl_cursor_image_get_buffer) | Accessing pixel data for cursors. |
| [Animation](#cursor-images) | [wl_cursor_frame](#wl_cursor_frame) | Handling animated cursors. |

---

## Types & Variables

### struct wl_cursor_theme

```c
struct wl_cursor_theme;
```

Structure representing a loaded cursor theme (a collection of cursors).

### struct wl_cursor

```c
struct wl_cursor {
    unsigned int image_count;
    struct wl_cursor_image **images;
    char *name;
};
```

Represents a cursor, which may consist of multiple images (frames) for animation.

### struct wl_cursor_image

```c
struct wl_cursor_image {
    uint32_t width;
    uint32_t height;
    uint32_t hotspot_x;
    uint32_t hotspot_y;
    uint32_t delay;
};
```

Represents a single frame of a cursor animation, containing dimensions and hotspot coordinates.

---

## Functions

### Theme Management

### wl_cursor_theme_load

```c
struct wl_cursor_theme *wl_cursor_theme_load(const char *name, int size, struct wl_shm *shm)
```

Loads a cursor theme from the filesystem.

**Returns:** A pointer to the loaded theme on success, or `NULL` on failure negotiation.

<details><summary>Example</summary>

```c
#include <wayland-client.h>
#include <wayland-cursor.h>
#include <stdio.h>

int main(void) {
    // external: shm (available from registry)
    struct wl_shm *shm = get_shm(); 
    
    // Load default theme, size 24
    struct wl_cursor_theme *theme = wl_cursor_theme_load(NULL, 24, shm);
    if (!theme) {
        fprintf(stderr, "Failed to load cursor theme\n");
        return 1;
    }

    // Use theme...

    wl_cursor_theme_destroy(theme);
    return 0;
}
```

</details>

### wl_cursor_theme_destroy

```c
void wl_cursor_theme_destroy(struct wl_cursor_theme *theme)
```

Destroys a cursor theme and frees associated resources.

**Returns:** None.

### wl_cursor_theme_get_cursor

```c
struct wl_cursor *wl_cursor_theme_get_cursor(struct wl_cursor_theme *theme, const char *name)
```

Retrieves a specific cursor (e.g., "left_ptr") from the loaded theme.

**Returns:** A pointer to the `wl_cursor` structure. Returns `NULL` if the cursor is not found in the theme.

<details><summary>Example</summary>

```c
#include <wayland-cursor.h>

struct wl_cursor *get_arrow(struct wl_cursor_theme *theme) {
    struct wl_cursor *cursor = wl_cursor_theme_get_cursor(theme, "left_ptr");
    if (!cursor) {
        // Fallback or error handling
        cursor = wl_cursor_theme_get_cursor(theme, "default");
    }
    return cursor;
}
```

</details>

### wl_cursor_theme_get_buffer

```c
struct wl_buffer *wl_cursor_image_get_buffer(struct wl_cursor_image *image)
```

Retrieves the `wl_buffer` containing the pixel data for a specific cursor image. This buffer can be attached to a surface.

**Returns:** A pointer to a valid `wl_buffer` on success.

<details><summary>Example</summary>

```c
#include <wayland-client.h>
#include <wayland-cursor.h>

void set_cursor_surface(struct wl_surface *surface, struct wl_cursor *cursor, int frame_index) {
    struct wl_cursor_image *image = cursor->images[frame_index];
    struct wl_buffer *buffer = wl_cursor_image_get_buffer(image);

    if (buffer) {
        wl_surface_attach(surface, buffer, 0, 0);
        wl_surface_damage(surface, 0, 0, image->width, image->height);
        wl_surface_commit(surface);
    }
}
```

</details>

---

### Animation

### wl_cursor_frame

```c
int wl_cursor_frame(struct wl_cursor *cursor, uint32_t time)
```

Determines which frame of the cursor animation should be displayed for the given timestamp.

**Returns:** The index of the image in `cursor->images` that should be displayed.

<details><summary>Example</summary>

```c
#include <wayland-cursor.h>

void animate_cursor(struct wl_cursor *cursor, uint32_t current_time) {
    int frame_idx = wl_cursor_frame(cursor, current_time);
    struct wl_cursor_image *image = cursor->images[frame_idx];
    // Display this image...
}
```

</details>

### wl_cursor_frame_and_duration

```c
int wl_cursor_frame_and_duration(struct wl_cursor *cursor, uint32_t time, uint32_t *duration)
```

Similar to `wl_cursor_frame`, but also retrieves the duration that the current frame should be displayed.

**Returns:** The index of the current frame.

