# Wayland Protocol Concepts

**Category:** 📂 wayland/ (Wayland Protocol)  
**Scope:** Understanding Wayland architecture

This document provides an overview of core Wayland protocol concepts essential for building compositors and clients.

## Core Concepts

### Display and Connection

The `wl_display` represents the connection between a client and the compositor. All Wayland communication happens through this object.

- **Server side:** Manages the display, accepts connections, routes messages
- **Client side:** Connects to display, sends requests, receives events

### Objects and Interfaces

Wayland communication is based on objects that implement interfaces:

- **Interfaces** define the protocol messages (requests and events)
- **Objects** are instances of interfaces
- Each object has a unique ID within the connection

### Global Objects

Global objects are advertised by the compositor and can be bound by clients:

- Compositor advertises globals during connection setup
- Clients bind globals to create local proxy objects
- Examples: `wl_compositor`, `wl_shm`, `wl_seat`

### Requests and Events

- **Requests:** Messages sent from client to server (e.g., create surface)
- **Events:** Messages sent from server to client (e.g., pointer motion)

### Surfaces and Buffers

- **Surfaces** represent windows or drawing areas
- **Buffers** contain the actual pixel data
- Clients attach buffers to surfaces for display
- Compositor composites surfaces onto outputs

## Compositor Architecture

### Core Components

1. **Display Management**
   - Accept client connections
   - Route protocol messages
   - Manage global objects

2. **Surface Management**
   - Track client surfaces
   - Handle buffer attachments
   - Perform compositing

3. **Input Handling**
   - Route input events to appropriate surfaces
   - Manage focus and grabs

4. **Output Management**
   - Configure display outputs
   - Handle mode setting

### Event Loop Integration

Compositors integrate with an event loop:

```c
#include <wayland-server.h>

int main(void) {
    struct wl_display *display = wl_display_create();
    struct wl_event_loop *loop = wl_display_get_event_loop(display);

    // Add file descriptors, timers, etc. to the event loop
    // wl_event_loop_add_fd(loop, ...)

    wl_display_run(display);
    wl_display_destroy(display);
    return 0;
}
```

### Client Architecture

### Connection Setup

```c
#include <wayland-client.h>

int main(void) {
    struct wl_display *display = wl_display_connect(NULL);
    struct wl_registry *registry = wl_display_get_registry(display);

    // Bind to globals...

    while (wl_display_dispatch(display) != -1) {
        // Handle events
    }

    wl_display_disconnect(display);
    return 0;
}
```

### Surface Creation

```c
#include <wayland-client.h>

struct wl_surface *create_surface(struct wl_compositor *compositor) {
    return wl_compositor_create_surface(compositor);
}
```

## Protocol Extensions

Wayland is extensible through protocol extensions:

- Additional interfaces for specific functionality
- Examples: XDG shell, Linux DMABUF, presentation time
- Extensions are separate from core protocol

## Security Model

- Clients are untrusted by default
- Compositor mediates all access to system resources
- No direct hardware access for clients
- All rendering goes through compositor

## Buffer Management

### Shared Memory (SHM)

- Clients allocate shared memory pools
- Create buffers from pools
- Attach buffers to surfaces

### DMA-BUF (Direct Rendering)

- Zero-copy buffer sharing with GPU
- Used for hardware-accelerated rendering
- More efficient than SHM for GPU operations

## Input Handling

### Seats

- Represent a group of input devices
- One seat per user
- Contains pointer, keyboard, touch devices

### Pointer Events

- Enter/leave surface events
- Motion events with surface coordinates
- Button press/release events

### Keyboard Events

- Focus changes
- Key press/release events
- Modifier state updates
