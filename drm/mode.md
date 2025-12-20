# xf86drmMode.h - Kernel Mode Setting (KMS)

The `xf86drmMode.h` header provides the interface for Direct Rendering Manager (DRM) Kernel Mode Setting (KMS).

## Facilities Overview

| Facility Category | Key Symbols | Description |
|-------------------|-------------|-------------|
| KMS Resources | drmModeGetResources | Retrieving CRTCs, connectors, and encoders. |
| Connectors & Modes| drmModeGetConnector | Querying display outputs. |
| Framebuffers | drmModeAddFB, drmModeRmFB | Managing buffers for display. |
| Mode Setting | drmModeSetCrtc | Applying a display mode. |
| Page Flipping | drmModePageFlip | Scheduling a buffer swap. |

---

## Types & Variables

### drmModeRes

```c
typedef struct _drmModeRes {
    int count_fbs;
    uint32_t *fbs;
    int count_crtcs;
    uint32_t *crtcs;
    int count_connectors;
    uint32_t *connectors;
    int count_encoders;
    uint32_t *encoders;
    uint32_t min_width, max_width;
    uint32_t min_height, max_height;
} drmModeRes, *drmModeResPtr;
```

### drmModeConnector

Represents a physical display output (HDMI, DisplayPort, eDP).

---

## Functions

### KMS Resources

#### drmModeGetResources

```c
drmModeResPtr drmModeGetResources(int fd)
```

Retrieves KMS resources such as CRTCs, connectors, encoders, and framebuffers.

**Example:**

```c
#include <xf86drm.h>
#include <xf86drmMode.h>
#include <stdio.h>

int main(void) {
    int fd = drmOpen(NULL, NULL);
    if (fd >= 0) {
        drmModeRes *res = drmModeGetResources(fd);
        if (res) {
            printf("Connectors: %d\n", res->count_connectors);
            drmModeFreeResources(res);
        }
        drmClose(fd);
    }
    return 0;
}
```

#### drmModeGetConnector

```c
drmModeConnectorPtr drmModeGetConnector(int fd, uint32_t connector_id)
```

#### drmModeFreeResources

```c
void drmModeFreeResources(drmModeResPtr ptr)
```

---

### Framebuffers & Display

#### drmModeAddFB

```c
int drmModeAddFB(int fd, uint32_t width, uint32_t height, uint8_t depth, uint8_t bpp, uint32_t pitch, uint32_t bo_handle, uint32_t *buf_id)
```

#### drmModePageFlip

```c
int drmModePageFlip(int fd, uint32_t crtc_id, uint32_t fb_id, uint32_t flags, void *user_data)
```