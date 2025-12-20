# wayland-client.h - Wayland Client Library

The `wayland-client.h` header is the primary interface for applications (clients) to communicate with a Wayland compositor.

## Facilities Overview

| Facility Category | Key Symbols | Description |
|-------------------|-------------|-------------|
| Display Control | wl_display_connect, wl_display_disconnect | Establishing and closing connections. |
| Registry & Globals| wl_display_get_registry | Discovering available compositor interfaces. |
| Event Loop | wl_display_dispatch, wl_display_flush | Managing the communication event loop. |
| Roundtrip | wl_display_roundtrip | Synchronizing with the compositor. |
| Proxies | wl_proxy_marshal, wl_proxy_destroy | Base interface for Wayland protocol objects. |

---

## Types & Variables

### struct wl_display

An opaque structure representing a connection to a Wayland compositor.

### struct wl_registry

An opaque structure representing the global registry.

### struct wl_interface

Defines the messages (events and requests) for a Wayland protocol interface.

---

## Functions

### Display Control

#### wl_display_connect

```c
struct wl_display *wl_display_connect(const char *name)
```

Connects to a Wayland display. If `name` is NULL, it uses the `WAYLAND_DISPLAY` environment variable.

**Example:**

```c
#include <wayland-client.h>
#include <stdio.h>

int main(void) {
    struct wl_display *display = wl_display_connect(NULL);
    if (!display) {
        return 1;
    }
    printf("Connected to Wayland!\n");
    wl_display_disconnect(display);
    return 0;
}
```

#### wl_display_disconnect

```c
void wl_display_disconnect(struct wl_display *display)
```

Closes the connection to the compositor and releases the display object.

---

### Registry & Globals

#### wl_display_get_registry

```c
struct wl_registry *wl_display_get_registry(struct wl_display *display)
```

Gets the global registry object for the display.

---

### Event Loop

#### wl_display_dispatch

```c
int wl_display_dispatch(struct wl_display *display)
```

#### wl_display_flush

```c
int wl_display_flush(struct wl_display *display)
```

---

### Synchronization

#### wl_display_roundtrip

```c
int wl_display_roundtrip(struct wl_display *display)
```