# xf86drm.h - Direct Rendering Manager (Core)

**Category:** 📂 drm/ (Direct Rendering Manager)
**Header:** `<xf86drm.h>`
**Scope:** Linux / BSD

The `xf86drm.h` header provides the basic interface for accessing the Direct Rendering Manager (DRM) kernel module. It facilitates device discovery, authentication, and capability management.

## Facilities Overview

| Facility Category | Key Symbols | Description |
|-------------------|-------------|-------------|
| [Device Management](#device-management) | [drmOpen](#drmopen), [drmClose](#drmclose), [drmGetDevices](#drmgetdevices) | Accessing graphics card device files. |
| [Environment](#environment) | [drmGetCap](#drmgetcap), [drmSetCap](#drmsetcap) | Querying and setting driver capabilities. |
| [Authentication](#authentication) | [drmAuthMagic](#drmauthmagic), [drmGetMagic](#drmgetmagic) | Handling DRM authentication. |

---

## Types & Variables

### drmDevice

```c
typedef struct _drmDevice drmDevice;
```

Structure describing a DRM device, including its bus type (PCI, USB, Platform, Host1x) and node paths (Primary, Control, Render).

### drm_magic_t

```c
typedef unsigned int drm_magic_t;
```

32-bit magic token type used for DRM authentication handshakes between the master and clients.

### DRM_NODE_PRIMARY

```c
#define DRM_NODE_PRIMARY 0
```

Index for the Primary device node (e.g., `/dev/dri/card0`). Used for modesetting and display control.

### DRM_NODE_CONTROL

```c
#define DRM_NODE_CONTROL 1
```

Index for the Control device node. (Legacy/Deprecated).

### DRM_NODE_RENDER

```c
#define DRM_NODE_RENDER 2
```

Index for the Render device node (e.g., `/dev/dri/renderD128`). Used for GPGPU and offscreen rendering.

---

## Functions

### Device Management

### drmOpen

```c
int drmOpen(const char *name, const char *busid)
```

Opens a DRM device, attempting to load the driver if necessary.

**Returns:** A file descriptor for the DRM device (non-negative integer) on success. On failure, returns -1 and sets `errno` to indicate the error (e.g., `ENOENT` if device not found, `EPERM` if permission denied).

<details><summary>Example</summary>

```c
#include <xf86drm.h>
#include <stdio.h>
#include <errno.h>

int main(void) {
    // Attempt to open a render node by name
    int fd = drmOpen("renderD128", NULL);
    
    if (fd < 0) {
        perror("Failed to open DRM device");
        return 1;
    }
    
    printf("Successfully opened DRM device! FD: %d\n", fd);
    
    // Always close resources
    if (drmClose(fd) < 0) {
        perror("Failed to close DRM device");
    }
    
    return 0;
}
```

</details>

### drmClose

```c
int drmClose(int fd)
```

Closes a DRM device file descriptor. This is a wrapper around the system `close()`.

**Returns:** 0 on success. On failure, returns a negative value and sets `errno`.

<details><summary>Example</summary>

```c
#include <xf86drm.h>
#include <unistd.h>

void cleanup_drm(int fd) {
    if (fd >= 0) {
        drmClose(fd);
    }
}
```

</details>

### drmGetDevices

```c
int drmGetDevices(drmDevicePtr devices[], int max_devices)
```

Populates an array with pointers to `drmDevice` structures for all available DRM devices on the system.

**Returns:** The number of devices found on success. On failure, returns a negative error code.

<details><summary>Example</summary>

```c
#include <xf86drm.h>
#include <stdio.h>

int main(void) {
    drmDevicePtr devices[64];
    int count = drmGetDevices(devices, 64);

    if (count < 0) {
        perror("drmGetDevices failed");
        return 1;
    }

    printf("Found %d DRM devices:\n", count);
    for (int i = 0; i < count; i++) {
        // Check for render node availability
        if (devices[i]->available_nodes & (1 << DRM_NODE_RENDER)) {
            printf("  Device %d: Render Node -> %s\n", 
                   i, devices[i]->nodes[DRM_NODE_RENDER]);
        }
    }

    drmFreeDevices(devices, count);
    return 0;
}
```

</details>

### drmFreeDevices

```c
void drmFreeDevices(drmDevicePtr devices[], int count)
```

Frees the memory allocated for the device list returned by `drmGetDevices`.

**Returns:** None.

### drmGetDeviceNameFromFd

```c
char *drmGetDeviceNameFromFd(int fd)
```

Retrieves the device node path (e.g., "/dev/dri/card0") associated with the given file descriptor.

**Returns:** A dynamically allocated string containing the device path. The caller must free this string. Returns `NULL` on failure.

---

### Capabilities

### drmGetCap

```c
int drmGetCap(int fd, uint64_t capability, uint64_t *value)
```

Queries the DRM driver for a specific capability (e.g., `DRM_CAP_DUMB_BUFFER`).

**Returns:** 0 on success. On failure, returns a negative value (typically -EINVAL if the capability is unknown or unsupported).

### drmSetCap

```c
int drmSetCap(int fd, uint64_t capability, uint64_t value)
```

Sets a client capability (e.g., `DRM_CLIENT_CAP_UNIVERSAL_PLANES`) to inform the kernel driver of the client's feature support.

**Returns:** 0 on success. On failure, returns a negative value.

---

### Authentication

### drmAuthMagic

```c
int drmAuthMagic(int fd, drm_magic_t magic)
```

Authenticates a client's magic token. This is typically called by the DRM Master (e.g., the display server) to authorize a client that has requested authentication.

**Returns:** 0 on success. On failure, returns a negative value.

### drmGetMagic

```c
int drmGetMagic(int fd, drm_magic_t *magic)
```

Retrieves a unique magic token for the file descriptor. The client sends this token to the DRM Master for authentication.

**Returns:** 0 on success. On failure, returns a negative value.

---

### Device Information

### drmGetVersion

```c
drmVersionPtr drmGetVersion(int fd)
```

Retrieves version information about the DRM driver associated with the file descriptor.

**Returns:** A pointer to a `drmVersion` structure. The caller must free this structure using `drmFreeVersion`. Returns `NULL` on failure.

### drmFreeVersion

```c
void drmFreeVersion(drmVersionPtr version)
```

Frees the memory associated with a `drmVersion` structure.

**Returns:** None.

### drmGetStats

```c
int drmGetStats(int fd, drmStatsT *stats)
```

Retrieves performance statistics from the DRM driver.

**Returns:** 0 on success. On failure, returns a non-zero error code.

