# xf86drm.h - Direct Rendering Manager (Core)

The `xf86drm.h` header provides the basic interface for accessing the Direct Rendering Manager (DRM) kernel module.

## Facilities Overview

| Facility Category | Key Symbols | Description |
|-------------------|-------------|-------------|
| Device Management | drmOpen, drmClose, drmGetDevices | Accessing graphics card device files. |
| Environment | drmGetCap, drmSetCap | Querying and setting driver capabilities. |
| Authentication| drmAuthMagic, drmGetMagic | Handling DRM authentication. |

---

## Types & Variables

### drmDevice

Structure describing a DRM device, including its bus type and node paths.

---

## Functions

### Device Management

#### drmOpen

```c
int drmOpen(const char *name, const char *busid)
```

Opens a DRM device.

**Example:**

```c
#include <xf86drm.h>
#include <stdio.h>

int main(void) {
    int fd = drmOpen("renderD128", NULL);
    if (fd >= 0) {
        printf("Successfully opened DRM device!\n");
        drmClose(fd);
    }
    return 0;
}
```

#### drmClose

```c
int drmClose(int fd)
```

Closes a DRM device.

#### drmGetDevices

```c
int drmGetDevices(drmDevicePtr devices[], int max_devices)
```

Returns a list of all available DRM devices on the system.