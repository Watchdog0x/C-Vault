# png.h - PNG Image Library

**Category:** 📂 image/ (Third-Party Ecosystem)
**Header:** `<png.h>`
**Scope:** libpng / PNG Decoding

The `png.h` header provides the interface for libpng, the official reference library for the PNG (Portable Network Graphics) format.

## Facilities Overview

| Facility Category | Key Symbols | Description |
|-------------------|-------------|-------------|
| [Read Structs](#reading-pngs) | [png_create_read_struct](#png_create_read_struct) | Initializing the library for decoding. |
| [Info Structs](#info-structures) | [png_create_info_struct](#png_create_info_struct) | Managing image metadata and header info. |
| [Initialization](#io-initialization) | [png_init_io](#png_init_io) | Connecting the library to a `FILE` stream. |
| [Reading](#reading-pngs) | [png_read_image](#png_read_image) | Decoding image data. |
| [Cleanup](#cleanup) | [png_destroy_read_struct](#png_destroy_read_struct) | Freeing library resources. |

---

## Types & Variables

### png_structp

```c
typedef struct png_struct_def *png_structp;
```

An opaque handle for the internal library state for reading or writing.

### png_infop

```c
typedef struct png_info_def *png_infop;
```

An opaque handle to the structure holding image information (width, height, bit depth, etc).

---

## Functions

### Reading PNGs

### png_create_read_struct

```c
png_structp png_create_read_struct(const char *ver, void *err, void *warn, void *user_ver)
```

Allocates and initializes a `png_struct` for reading.
*   `ver`: `PNG_LIBPNG_VER_STRING`.

**Returns:** A pointer to the structure, or `NULL` on failure.

<details><summary>Example</summary>

```c
#include <png.h>
#include <stdio.h>

int main(void) {
    png_structp png = png_create_read_struct(PNG_LIBPNG_VER_STRING, NULL, NULL, NULL);
    if (!png) return 1;
    
    png_infop info = png_create_info_struct(png);
    if (!info) {
        png_destroy_read_struct(&png, NULL, NULL);
        return 1;
    }
    
    // ... use png/info ...
    
    png_destroy_read_struct(&png, &info, NULL);
    return 0;
}
```

</details>

### Info Structures

### png_create_info_struct

```c
png_infop png_create_info_struct(png_structp png_ptr)
```

Allocates and initializes a `png_info` structure.

**Returns:** A pointer to the structure, or `NULL`.

---

### IO Initialization

### png_init_io

```c
void png_init_io(png_structp png_ptr, FILE *fp)
```

Initializes the default input/output functions for the PNG file to standard C streams.

**Returns:** None.

---

### Cleanup

### png_destroy_read_struct

```c
void png_destroy_read_struct(png_structpp png_ptr, png_infopp info_ptr, png_infopp end_info_ptr)
```

Frees the memory associated with the read structure and info structures.

**Returns:** None.