# drm_fourcc.h - DRM Pixel Formats

**Category:** 📂 drm/ (Direct Rendering Manager)
**Header:** `<drm/drm_fourcc.h>`
**Scope:** Graphics programming with DRM

The `drm_fourcc.h` header defines pixel formats used in DRM (Direct Rendering Manager) for describing buffer layouts. These formats are based on the FourCC (Four Character Code) standard.

## Facilities Overview

| [Format Definition](#common-pixel-formats) | DRM_FORMAT_* | Predefined pixel format constants. |
| [Format Utilities](#format-macros) | DRM_FORMAT_* macros | Format querying and manipulation. |
| [Format Modifiers](#format-modifiers) | DRM_FORMAT_MOD_* | Buffer layout modifiers for tiling/compression. |

---

## Common Pixel Formats

### RGB Formats

#### DRM_FORMAT_XRGB8888

```c
#define DRM_FORMAT_XRGB8888 875713112
```

32-bit RGB with ignored alpha channel.

**Layout:** XRGB (8:8:8:8)
**Bytes per pixel:** 4

#### DRM_FORMAT_ARGB8888

```c
#define DRM_FORMAT_ARGB8888 875713089
```

32-bit RGBA with alpha channel.

**Layout:** ARGB (8:8:8:8)
**Bytes per pixel:** 4

#### DRM_FORMAT_RGB565

```c
#define DRM_FORMAT_RGB565 1346520914
```

16-bit RGB format.

**Layout:** RGB (5:6:5)
**Bytes per pixel:** 2

---

## YUV Formats

### DRM_FORMAT_NV12

YUV 4:2:0 format with semi-planar layout.

**Planes:**
- Plane 0: Y (luma)
- Plane 1: UV (chroma)

**Bytes per pixel:** 1.5

### DRM_FORMAT_YUYV

YUV 4:2:2 packed format.

**Layout:** YUYV (8:8:8:8)
**Bytes per pixel:** 2

---

## Format Macros

#### DRM_FORMAT_RGB

```c
#define DRM_FORMAT_RGB 0
```

Tests if a format is RGB.

#### DRM_FORMAT_YUV

```c
#define DRM_FORMAT_YUV 0
```

Tests if a format is YUV.

---

## Format Modifiers

Format modifiers describe buffer layout variations:

### DRM_FORMAT_MOD_LINEAR

Linear buffer layout (no tiling).

### DRM_FORMAT_MOD_INVALID

Invalid modifier.

---

## Usage Examples

### Creating Framebuffers

<details><summary>Example</summary>

```c
#include <xf86drm.h>
#include <xf86drmMode.h>
#include <drm/drm_fourcc.h>

int create_framebuffer(int fd, uint32_t width, uint32_t height) {
    uint32_t format = DRM_FORMAT_XRGB8888;
    uint32_t bo_handle = 1; // Placeholder handle
    uint32_t handles[4] = { bo_handle, 0, 0, 0 };
    uint32_t pitches[4] = { width * 4, 0, 0, 0 };
    uint32_t offsets[4] = { 0, 0, 0, 0 };
    uint32_t fb_id;

    int ret = drmModeAddFB2(fd, width, height, format, handles, pitches, offsets, &fb_id, 0);
    if (ret < 0) {
        return ret;
    }

    return fb_id;
}
```

</details>

### Checking Format Support

<details><summary>Example</summary>

```c
#include <drm/drm_fourcc.h>

int is_rgb_format(uint32_t format) {
    // Note: This is an illustrative macro check, actual logic may vary
    return (format == DRM_FORMAT_XRGB8888 || format == DRM_FORMAT_ARGB8888); 
}
```

</details>

### Buffer Allocation

<details><summary>Example</summary>

```c
#include <drm/drm_fourcc.h>

size_t calculate_buffer_size(uint32_t format, uint32_t width, uint32_t height) {
    switch (format) {
    case DRM_FORMAT_XRGB8888:
    case DRM_FORMAT_ARGB8888:
        return width * height * 4;
    case DRM_FORMAT_RGB565:
    case DRM_FORMAT_YUYV:
        return width * height * 2;
    case DRM_FORMAT_NV12:
        return width * height * 3 / 2; // 1.5 bytes per pixel
    default:
        return 0;
    }
}
```

</details>

---

## Common Format Values

| Format | Value | Description |
|--------|-------|-------------|
| DRM_FORMAT_C8 | 0x20203843 | 8-bit color index |
| DRM_FORMAT_RGB332 | 0x38424752 | 8-bit RGB |
| DRM_FORMAT_BGR233 | 0x33424752 | 8-bit BGR |
| DRM_FORMAT_XRGB4444 | 0x32315258 | 16-bit xRGB |
| DRM_FORMAT_XBGR4444 | 0x32314258 | 16-bit xBGR |
| DRM_FORMAT_RGBX4444 | 0x32315852 | 16-bit RGBx |
| DRM_FORMAT_BGRX4444 | 0x32315842 | 16-bit BGRx |
| DRM_FORMAT_ARGB4444 | 0x32315241 | 16-bit ARGB |
| DRM_FORMAT_ABGR4444 | 0x32314241 | 16-bit ABGR |
| DRM_FORMAT_RGBA4444 | 0x32314152 | 16-bit RGBA |
| DRM_FORMAT_BGRA4444 | 0x32314142 | 16-bit BGRA |
| DRM_FORMAT_XRGB1555 | 0x35315258 | 16-bit xRGB |
| DRM_FORMAT_XBGR1555 | 0x35314258 | 16-bit xBGR |
| DRM_FORMAT_RGBX5551 | 0x35315852 | 16-bit RGBx |
| DRM_FORMAT_BGRX5551 | 0x35315842 | 16-bit BGRx |
| DRM_FORMAT_ARGB1555 | 0x35315241 | 16-bit ARGB |
| DRM_FORMAT_ABGR1555 | 0x35314241 | 16-bit ABGR |
| DRM_FORMAT_RGBA5551 | 0x35314152 | 16-bit RGBA |
| DRM_FORMAT_BGRA5551 | 0x35314142 | 16-bit BGRA |
| DRM_FORMAT_RGB565 | 0x36314752 | 16-bit RGB |
| DRM_FORMAT_BGR565 | 0x36314742 | 16-bit BGR |
| DRM_FORMAT_RGB888 | 0x34324752 | 24-bit RGB |
| DRM_FORMAT_BGR888 | 0x34324742 | 24-bit BGR |
| DRM_FORMAT_XRGB8888 | 0x34325258 | 32-bit xRGB |
| DRM_FORMAT_XBGR8888 | 0x34324258 | 32-bit xBGR |
| DRM_FORMAT_RGBX8888 | 0x34325852 | 32-bit RGBx |
| DRM_FORMAT_BGRX8888 | 0x34325842 | 32-bit BGRx |
| DRM_FORMAT_ARGB8888 | 0x34325241 | 32-bit ARGB |
| DRM_FORMAT_ABGR8888 | 0x34324241 | 32-bit ABGR |
| DRM_FORMAT_RGBA8888 | 0x34324152 | 32-bit RGBA |
| DRM_FORMAT_BGRA8888 | 0x34324142 | 32-bit BGRA |
| DRM_FORMAT_XRGB2101010 | 0x30335258 | 32-bit xRGB |
| DRM_FORMAT_XBGR2101010 | 0x30334258 | 32-bit xBGR |
| DRM_FORMAT_RGBX1010102 | 0x30335852 | 32-bit RGBx |
| DRM_FORMAT_BGRX1010102 | 0x30335842 | 32-bit BGRx |
| DRM_FORMAT_ARGB2101010 | 0x30335241 | 32-bit ARGB |
| DRM_FORMAT_ABGR2101010 | 0x30334241 | 32-bit ABGR |
| DRM_FORMAT_RGBA1010102 | 0x30334152 | 32-bit RGBA |
| DRM_FORMAT_BGRA1010102 | 0x30334142 | 32-bit BGRA |
