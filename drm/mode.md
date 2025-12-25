# xf86drmMode.h - Kernel Mode Setting (KMS)

**Category:** 📂 drm/ (Direct Rendering Manager)
**Header:** `<xf86drmMode.h>`
**Scope:** Linux / BSD

The `xf86drmMode.h` header provides the interface for Direct Rendering Manager (DRM) Kernel Mode Setting (KMS). It allows applications to query display resources, configure CRTCs, and schedule page flips.

## Facilities Overview

| Facility Category | Key Symbols | Description |
|-------------------|-------------|-------------|
| [KMS Resources](#kms-resources) | [drmModeGetResources](#drmmodegetresources) | Retrieving global display resources (CRTCs, connectors, encoders). |
| [Connectors & Modes](#connectors--modes) | [drmModeGetConnector](#drmmodegetconnector) | Querying display outputs and their supported modes. |
| [Framebuffers](#framebuffers--display) | [drmModeAddFB](#drmmodeaddfb), [drmModeRmFB](#drmmodermfb) | Creating and removing framebuffers. |
| [Mode Setting](#framebuffers--display) | [drmModeSetCrtc](#drmmodesetcrtc) | Applying a specific mode and framebuffer to a CRTC. |
| [Page Flipping](#framebuffers--display) | [drmModePageFlip](#drmmodepageflip) | Scheduling a non-blocking buffer swap (vsync). |

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

Contains lists of all available display resources (Framebuffers, CRTCs, Connectors, Encoders) and the min/max supported resolution.

### drmModeConnector

```c
typedef struct _drmModeConnector drmModeConnector;
```

Represents a physical display output (e.g., HDMI, DisplayPort). Contains connection status, valid modes, and attached encoder info.

### drmModeEncoder

```c
typedef struct _drmModeEncoder drmModeEncoder;
```

Represents a hardware encoder that translates CRTC pixel data into signals for a connector.

### drmModeCrtc

```c
typedef struct _drmModeCrtc drmModeCrtc;
```

Represents a CRT Controller (CRTC), which scans out the framebuffer to the display path.

### drmModeModeInfo

```c
typedef struct _drmModeModeInfo {
    uint32_t clock;
    uint16_t hdisplay, hsync_start, hsync_end, htotal, hskew;
    uint16_t vdisplay, vsync_start, vsync_end, vtotal, vscan;
    uint32_t vrefresh;
    uint32_t flags;
    uint32_t type;
    char name[DRM_DISPLAY_MODE_LEN];
} drmModeModeInfo, *drmModeModeInfoPtr;
```

Describes a specific video mode (resolution, refresh rate, and timings).

---

## Functions

### KMS Resources

### drmModeGetResources

```c
drmModeResPtr drmModeGetResources(int fd)
```

Retrieves KMS resources such as CRTCs, connectors, encoders, and framebuffers.

**Returns:** A pointer to a `drmModeRes` structure on success. Returns `NULL` on failure (e.g., if `fd` is invalid or resources could not be allocated).

<details><summary>Example</summary>

```c
#include <xf86drm.h>
#include <xf86drmMode.h>
#include <stdio.h>
#include <stdlib.h>

int main(void) {
    int fd = drmOpen("renderD128", NULL); 
    if (fd < 0) return 1;

    drmModeRes *res = drmModeGetResources(fd);
    if (!res) {
        perror("Failed to get DRM resources");
        drmClose(fd);
        return 1;
    }

    printf("Connectors: %d, CRTCs: %d\n", res->count_connectors, res->count_crtcs);

    drmModeFreeResources(res);
    drmClose(fd);
    return 0;
}
```

</details>

### drmModeGetConnector

```c
drmModeConnectorPtr drmModeGetConnector(int fd, uint32_t connector_id)
```

Retrieves detailed information about a specific display connector.

**Returns:** A pointer to a `drmModeConnector` structure on success. Returns `NULL` on failure.

<details><summary>Example</summary>

```c
#include <xf86drm.h>
#include <xf86drmMode.h>
#include <stdio.h>

void list_connectors(int fd, drmModeRes *res) {
    for (int i = 0; i < res->count_connectors; i++) {
        drmModeConnector *conn = drmModeGetConnector(fd, res->connectors[i]);
        if (conn) {
            if (conn->connection == DRM_MODE_CONNECTED) {
                printf("Connector %u: CONNECTED (Type: %d)\n", 
                       conn->connector_id, conn->connector_type);
            }
            drmModeFreeConnector(conn);
        }
    }
}
```

</details>

### drmModeFreeConnector

```c
void drmModeFreeConnector(drmModeConnectorPtr ptr)
```

Frees the memory associated with a `drmModeConnector` structure.

**Returns:** None.

### drmModeGetCrtc

```c
drmModeCrtcPtr drmModeGetCrtc(int fd, uint32_t crtc_id)
```

Retrieves information about a specific CRTC.

**Returns:** A pointer to a `drmModeCrtc` structure on success. Returns `NULL` on failure.

### drmModeFreeCrtc

```c
void drmModeFreeCrtc(drmModeCrtcPtr ptr)
```

Frees the memory associated with a `drmModeCrtc` structure.

**Returns:** None.

### drmModeFreeResources

```c
void drmModeFreeResources(drmModeResPtr ptr)
```

Frees the memory associated with a `drmModeRes` structure.

**Returns:** None.

---

### Framebuffers & Display

### drmModeAddFB

```c
int drmModeAddFB(int fd, uint32_t width, uint32_t height, uint8_t depth, uint8_t bpp, uint32_t pitch, uint32_t bo_handle, uint32_t *buf_id)
```

Creates a standard framebuffer from a buffer object handle.

**Returns:** 0 on success. On failure, returns a negative error code (e.g., `-EINVAL` for invalid dimensions).

### drmModeAddFB2

```c
int drmModeAddFB2(int fd, uint32_t width, uint32_t height, uint32_t pixel_format, uint32_t bo_handles[4], uint32_t pitches[4], uint32_t offsets[4], uint32_t *buf_id, uint32_t flags)
```

Creates a framebuffer with advanced parameters, supporting multi-planar formats and modifiers.

**Returns:** 0 on success. On failure, returns a negative error code.

### drmModeRmFB

```c
int drmModeRmFB(int fd, uint32_t bufferId)
```

Destroys a framebuffer. This does not destroy the underlying buffer object, only the DRM framebuffer object.

**Returns:** 0 on success. On failure, returns a negative error code.

### drmModeSetCrtc

```c
int drmModeSetCrtc(int fd, uint32_t crtcId, uint32_t bufferId, uint32_t x, uint32_t y, uint32_t *connectors, int count, drmModeModeInfoPtr mode)
```

Sets the display mode for a CRTC. This creates the association between a CRTC, a Framebuffer, and a set of Connectors.

**Returns:** 0 on success. On failure, returns a negative error code.

<details><summary>Example</summary>

```c
#include <xf86drm.h>
#include <xf86drmMode.h>
#include <errno.h>

int set_mode_simple(int fd, uint32_t crtc_id, uint32_t fb_id, uint32_t conn_id, drmModeModeInfo *mode) {
    uint32_t connectors[] = { conn_id };
    
    int ret = drmModeSetCrtc(fd, crtc_id, fb_id, 0, 0, connectors, 1, mode);
    if (ret < 0) {
        // -errno is returned
        return ret;
    }
    return 0;
}
```

</details>

### drmModePageFlip

```c
int drmModePageFlip(int fd, uint32_t crtc_id, uint32_t fb_id, uint32_t flags, void *user_data)
```

Schedules a page flip to occur during the next vertical blanking interval. This is used for double buffering.

**Returns:** 0 on success. On failure, returns a negative error code (e.g., `-EBUSY` if a flip is already pending).

### drmModeGetEncoder

```c
drmModeEncoderPtr drmModeGetEncoder(int fd, uint32_t encoder_id)
```

Retrieves information about a specific encoder.

**Returns:** A pointer to a `drmModeEncoder` structure on success. Returns `NULL` on failure.

### drmModeFreeEncoder

```c
void drmModeFreeEncoder(drmModeEncoderPtr ptr)
```

Frees the memory associated with a `drmModeEncoder` structure.

**Returns:** None.

