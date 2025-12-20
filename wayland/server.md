# wayland-server.h - Wayland Server Library

The `wayland-server.h` header is the primary interface for building a Wayland compositor.

## Facilities Overview

| Facility Category | Key Symbols | Description |
|-------------------|-------------|-------------|
| Display Management | wl_display_create, wl_display_destroy | Managing the core compositor display. |
| Client Management  | wl_client_create, wl_client_destroy | Handling connections from Wayland clients. |
| Registry & Globals | wl_global_create | Advertising interfaces to connected clients. |
| Event Loop         | wl_display_get_event_loop | Integrating with the system event loop. |
| Resource Mgmt      | wl_resource_create, wl_resource_post_event| Managing protocol objects for clients. |

---

## Types & Variables

### struct wl_display

The core object representing the Wayland server.

### struct wl_client

Represents a connected client.

### struct wl_resource

Represents a client-side object on the server.

---

## Functions

### Display Management

#### wl_display_create

```c
struct wl_display *wl_display_create(void)
```

Creates a new Wayland display object.

**Example:**

```c
#include <wayland-server.h>
#include <stdio.h>

int main(void) {
    struct wl_display *display = wl_display_create();
    if (display) {
        wl_display_destroy(display);
    }
    return 0;
}
```

#### wl_display_add_socket

```c
const char *wl_display_add_socket(struct wl_display *display, const char *name)
```

Adds a Unix socket to the display for clients to connect to.

#### wl_display_destroy

```c
void wl_display_destroy(struct wl_display *display)
```

---

### Registry & Globals

#### wl_global_create

```c
struct wl_global *wl_global_create(struct wl_display *display, const struct wl_interface *interface, int version, void *data, wl_global_bind_func_t bind)
```

---

### Execution

#### wl_display_run

```c
void wl_display_run(struct wl_display *display)
```

Starts the compositor's event loop.