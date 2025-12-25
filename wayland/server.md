# wayland-server.h - Wayland Server Library

**Category:** 📂 wayland/ (Wayland Protocol)
**Header:** `<wayland-server.h>`
**Scope:** Wayland compositors

The `wayland-server.h` header is the primary interface for building a Wayland compositor. It allows creating the display, managing clients, and handling the event loop.

## Facilities Overview

| Facility Category | Key Symbols | Description |
|-------------------|-------------|-------------|
| [Display Management](#display-management) | [wl_display_create](#wl_display_create), [wl_display_destroy](#wl_display_destroy) | Managing the core compositor display. |
| [Client Management](#client-management)  | [wl_client_create](#wl_client_create), [wl_client_destroy](#wl_client_destroy) | Handling connections from Wayland clients. |
| [Registry & Globals](#registry--globals) | [wl_global_create](#wl_global_create) | Advertising interfaces to connected clients. |
| [Event Loop](#event-loop-integration)         | [wl_display_get_event_loop](#wl_display_get_event_loop) | Integrating with the system event loop. |
| [Resource Mgmt](#resource-management)      | [wl_resource_create](#wl_resource_create), [wl_resource_post_event](#wl_resource_post_event)| Managing protocol objects for clients. |

---

## Types & Variables

struct wl_display;
```

The core object representing the Wayland server.

### struct wl_client

```c
struct wl_client;
```

Represents a connected client (application).

### struct wl_resource

```c
struct wl_resource;
```

Represents a client-side object (like a surface or buffer) as seen by the server.

### struct wl_global

```c
struct wl_global;
```

Represents a global object advertised to clients via the registry.

### struct wl_event_loop

```c
struct wl_event_loop;
```

Represents an event loop for handling I/O and timers.

---

## Functions

### Display Management

### wl_display_create

```c
struct wl_display *wl_display_create(void)
```

Creates a new Wayland display object. This is the root of the compositor.

**Returns:** A pointer to a new `wl_display` object on success, or `NULL` on failure.

<details><summary>Example</summary>

```c
#include <wayland-server.h>
#include <stdio.h>

int main(void) {
    struct wl_display *display = wl_display_create();
    if (!display) {
        fprintf(stderr, "Failed to create display\n");
        return 1;
    }
    
    // Setup socket, run loop...
    
    wl_display_destroy(display);
    return 0;
}
```

</details>

### wl_display_add_socket

```c
int wl_display_add_socket(struct wl_display *display, const char *name)
```

Adds a Unix socket to the display for clients to connect to. If `name` is `NULL`, it defaults to "wayland-0".

**Returns:** 0 on success. On failure, returns -1.

### wl_display_destroy

```c
void wl_display_destroy(struct wl_display *display)
```

Destroys the display and frees all associated resources.

**Returns:** None.

### Execution

### wl_display_run

```c
void wl_display_run(struct wl_display *display)
```

Starts the compositor's event loop, processing events until `wl_display_terminate()` is called.

**Returns:** None.

<details><summary>Example</summary>

```c
#include <wayland-server.h>

int main(void) {
    struct wl_display *display = wl_display_create();
    if (!display) return 1;

    // Add socket
    if (wl_display_add_socket(display, NULL)) {
        wl_display_destroy(display);
        return 1;
    }

    wl_display_run(display); // Blocks until termination
    
    wl_display_destroy(display);
    return 0;
}
```

</details>

### wl_display_get_event_loop

```c
struct wl_event_loop *wl_display_get_event_loop(struct wl_display *display)
```

Retrieves the event loop associated with the display. This allows adding custom file descriptors or timers to the loop.

**Returns:** A pointer to the `wl_event_loop`.

### wl_display_terminate

```c
void wl_display_terminate(struct wl_display *display)
```

Terminates the display's event loop. This causes `wl_display_run()` to return.

**Returns:** None.

---

### Client Management

### wl_client_create

```c
struct wl_client *wl_client_create(struct wl_display *display, int fd)
```

Creates a client object from an already accepted socket file descriptor.

**Returns:** A pointer to the new `wl_client`, or `NULL` on failure.

### wl_client_destroy

```c
void wl_client_destroy(struct wl_client *client)
```

Destroys a client object and closes its connection.

**Returns:** None.

### wl_client_get_credentials

```c
void wl_client_get_credentials(struct wl_client *client, pid_t *pid, uid_t *uid, gid_t *gid)
```

Retrieves the process ID, user ID, and group ID of the client process.

**Returns:** None (output via pointers).

<details><summary>Example</summary>

```c
#include <wayland-server.h>
#include <unistd.h>
#include <stdio.h>

void log_client_info(struct wl_client *client) {
    pid_t pid;
    uid_t uid;
    gid_t gid;

    wl_client_get_credentials(client, &pid, &uid, &gid);
    printf("Client connected: PID=%d, UID=%d\n", pid, uid);
}
```

</details>

---

### Registry & Globals

### wl_global_create

```c
struct wl_global *wl_global_create(struct wl_display *display, const struct wl_interface *interface, int version, void *data, wl_global_bind_func_t bind)
```

Creates a global object that is advertised to all clients.

**Returns:** A pointer to the new `wl_global`, or `NULL` on failure.

### wl_global_destroy

```c
void wl_global_destroy(struct wl_global *global)
```

Destroys a global object. Clients will no longer see it advertised.

**Returns:** None.

<details><summary>Example</summary>

```c
#include <wayland-server.h>

static void bind_compositor(struct wl_client *client, void *data, uint32_t version, uint32_t id) {
    // Implementation of binding logic (create resource, etc.)
}

void register_globals(struct wl_display *display) {
    wl_global_create(display, &wl_compositor_interface, 4, NULL, bind_compositor);
}
```

</details>

---

### Resource Management

### wl_resource_create

```c
struct wl_resource *wl_resource_create(struct wl_client *client, const struct wl_interface *interface, int version, uint32_t id)
```

Creates a resource object for a client. This is typically done inside the bind function of a global.

**Returns:** A pointer to the new `wl_resource`, or `NULL` on failure.

### wl_resource_destroy

```c
void wl_resource_destroy(struct wl_resource *resource)
```

Destroys a resource object.

**Returns:** None.

### wl_resource_post_event

```c
void wl_resource_post_event(struct wl_resource *resource, uint32_t opcode, ...)
```

Posts an event to a client resource. The arguments must match the event signature defined in the protocol.

**Returns:** None.

### wl_resource_post_error

```c
void wl_resource_post_error(struct wl_resource *resource, uint32_t code, const char *msg, ...)
```

Posts a protocol error to a client resource. This typically causes the client to be disconnected.

**Returns:** None.

---

### Event Loop Integration

### wl_event_loop_add_fd

```c
struct wl_event_source *wl_event_loop_add_fd(struct wl_event_loop *loop, int fd, uint32_t mask, wl_event_loop_fd_func_t func, void *data)
```

Adds a file descriptor to the event loop.

**Returns:** A pointer to the `wl_event_source`, or `NULL` on failure.

### wl_event_source_remove

```c
int wl_event_source_remove(struct wl_event_source *source)
```

Removes an event source from the event loop.

**Returns:** 0 on success.

