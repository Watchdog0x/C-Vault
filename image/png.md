# png.h - PNG Image Library

The `png.h` header provides the interface for libpng, the official reference library for the PNG format.

## Facilities Overview

| Facility Category | Key Symbols | Description |
|-------------------|-------------|-------------|
| Read Structs | png_create_read_struct | Initializing the library for decoding. |
| Info Structs | png_create_info_struct | Managing image metadata and header info. |
| I/O Initialization| png_init_io | Connecting the library to a `FILE` stream. |
| Reading | png_read_image, png_read_info | Decoding image data. |
| Cleanup | png_destroy_read_struct | Freeing library resources. |

---

## Types & Variables

### png_structp

An opaque handle for the internal library state for reading or writing.

### png_infop

An opaque handle to the structure holding image information.

---

## Functions

### Reading PNGs

#### png_create_read_struct

```c
png_structp png_create_read_struct(const char *ver, void *err, ...)
```

Allocates and initializes a `png_struct` for reading.

**Example:**

```c
#include <png.h>

int main(void) {
    png_structp png = png_create_read_struct(PNG_LIBPNG_VER_STRING, NULL, NULL, NULL);
    if (png) {
        png_destroy_read_struct(&png, NULL, NULL);
    }
    return 0;
}
```

#### png_create_info_struct

```c
png_infop png_create_info_struct(png_structp png_ptr)
```

Allocates and initializes a `png_info` structure.

#### png_init_io

```c
void png_init_io(png_structp png_ptr, FILE *fp)
```

---

### Cleanup

#### png_destroy_read_struct

```c
void png_destroy_read_struct(png_structpp png_ptr, png_infopp info_ptr, png_infopp end_info_ptr)
```