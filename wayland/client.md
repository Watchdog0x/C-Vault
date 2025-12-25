# wayland-client.h - Wayland Client Library

**Category:** 📂 wayland/ (Wayland)
**Header:** `<wayland-client.h>`
**Scope:** Client-side Wayland applications

The `wayland-client.h` header is the primary interface for applications (clients) to communicate with a Wayland compositor. It handles connection setup, event dispatching, and object management.

## Facilities Overview

| Facility Category | Key Symbols | Description |
|-------------------|-------------|-------------|
| [Display Control](#display-control) | [wl_display_connect](#wl_display_connect), [wl_display_disconnect](#wl_display_disconnect) | Establishing and closing connections. |
| [Registry & Globals](#registry--globals)| [wl_display_get_registry](#wl_display_get_registry) | Discovering available compositor interfaces. |
| [Event Loop](#event-loop) | [wl_display_dispatch](#wl_display_dispatch), [wl_display_flush](#wl_display_flush) | Managing the communication event loop. |
| [Roundtrip](#synchronization) | [wl_display_roundtrip](#wl_display_roundtrip) | Synchronizing with the compositor. |

---

## Types & Variables

### struct wl_display

```c
struct wl_display;
```

An opaque structure representing a connection to a Wayland compositor. It encapsulates the socket and the event queue.

### struct wl_registry

```c
struct wl_registry;
```

An opaque structure representing the global registry. Clients use this to discover global objects (like the compositor, shell, or seat) advertised by the server.

### struct wl_interface

```c
struct wl_interface;
```

Defines the messages (events and requests) for a Wayland protocol interface.

### struct wl_event_queue

```c
struct wl_event_queue;
```

Represents an event queue for organizing protocol messages. This allows threading where different threads handle different queues.

---

## Functions

### Display Control

### wl_display_connect

```c
struct wl_display *wl_display_connect(const char *name)
```

Connects to a Wayland display. If `name` is NULL, it uses the `WAYLAND_DISPLAY` environment variable.

**Returns:** A pointer to a `wl_display` object on success. Returns `NULL` on failure (e.g., if the socket cannot be opened).

<details><summary>Example</summary>

```c
#include <wayland-client.h>
#include <stdio.h>

int main(void) {
    struct wl_display *display = wl_display_connect(NULL);
    if (!display) {
        perror("Failed to connect to Wayland display");
        return 1;
    }
    
    printf("Connected to Wayland!\n");
    
    wl_display_disconnect(display);
    return 0;
}
```

</details>

### wl_display_disconnect

```c
void wl_display_disconnect(struct wl_display *display)
```

Closes the connection to the compositor and releases the display object.

**Returns:** None.

---

### Registry & Globals

### wl_display_get_registry

```c
struct wl_registry *wl_display_get_registry(struct wl_display *display)
```

Gets the global registry object for the display. This is typically the first object a client creates to discover other globals.

**Returns:** A pointer to a `wl_registry` object. (Note: The actual registry events are delivered asynchronously).

---

### Event Loop

### wl_display_dispatch

```c
int wl_display_dispatch(struct wl_display *display)
```

Processes incoming events from the compositor. This function blocks until events are available and then dispatches them to their respective handlers.

**Returns:** The number of bytes read from the display connection on success. Returns -1 on failure (e.g., connection disconnect).

<details><summary>Example</summary>

```c
#include <wayland-client.h>
#include <stdio.h>

int main(void) {
    struct wl_display *display = wl_display_connect(NULL);
    if (!display) return 1;

    // Main event loop
    while (wl_display_dispatch(display) != -1) {
        // Events are handled by callbacks registered elsewhere
    }
    
    if (wl_display_get_error(display)) {
        perror("Wayland display error");
    }

    wl_display_disconnect(display);
    return 0;
}
```

</details>

### wl_display_dispatch_pending

```c
int wl_display_dispatch_pending(struct wl_display *display)
```

Dispatches any pending events currently in the queue without blocking.

**Returns:** The number of dispatched events. Returns -1 on failure.

### wl_display_flush

```c
int wl_display_flush(struct wl_display *display)
```

Flushes pending requests to the compositor. This ensures that buffered requests are written to the socket.

**Returns:** The number of bytes written to the socket on success. Returns -1 on failure.

<details><summary>Example</summary>

```c
#include <wayland-client.h>

void send_urgent_request(struct wl_display *display) {
    // ... logic to send request ...
    
    // Force write to socket immediately
    if (wl_display_flush(display) == -1) {
        // Handle error (e.g., disconnected)
    }
}
```

</details>

### wl_display_prepare_read

```c
int wl_display_prepare_read(struct wl_display *display)
```

Prepares the display for reading events without blocking. Used when integrating Wayland into an external event loop (like poll/epoll).

**Returns:** 0 on success. Returns -1 if the queue is not empty (events should be dispatched first).

### wl_display_read_events

```c
int wl_display_read_events(struct wl_display *display)
```

Reads events from the display socket after `wl_display_prepare_read` has been successfully called.

**Returns:** 0 on success. Returns -1 on failure.

---

### Synchronization

### wl_display_roundtrip

```c
int wl_display_roundtrip(struct wl_display *display)
```

Performs a roundtrip with the compositor, ensuring all previous requests are processed and all resulting events are handled.

**Returns:** The number of dispatched events on success. Returns -1 on failure.

<details><summary>Example</summary>

```c
#include <wayland-client.h>
#include <stdio.h>

int main(void) {
    struct wl_display *display = wl_display_connect(NULL);
    if (!display) return 1;

    // Send initial requests (e.g. get registry)
    // ...
    
    // Wait for compositor to process them and reply
    if (wl_display_roundtrip(display) == -1) {
        fprintf(stderr, "Roundtrip failed\n");
        return 1;
    }

    wl_display_disconnect(display);
    return 0;
}
```

</details>

### wl_display_roundtrip_queue

```c
int wl_display_roundtrip_queue(struct wl_display *display, struct wl_event_queue *queue)
```

Performs a roundtrip involving a specific event queue.

**Returns:** The number of dispatched events on success. Returns -1 on failure.

