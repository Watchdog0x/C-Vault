# jpeglib.h - JPEG Image Library

The `jpeglib.h` header provides the interface for libjpeg, used for compressing and decompressing JPEG images.

## Facilities Overview

| Facility Category | Key Symbols | Description |
|-------------------|-------------|-------------|
| Decompression | jpeg_create_decompress | Initializing a decoder. |
| Source Management | jpeg_stdio_src | Setting the input file. |
| Header Parsing | jpeg_read_header | Retrieving image dimensions. |
| Scanline Reading | jpeg_read_scanlines | Decoding the image row by row. |
| Cleanup | jpeg_destroy_decompress | Releasing resources. |

---

## Types & Variables

### jpeg_decompress_struct

Structure holding the state for a decompression operation.

### jpeg_error_mgr

Structure for handling errors and warnings.

---

## Functions

### Decompressing JPEGs

#### jpeg_create_decompress

```c
void jpeg_create_decompress(struct jpeg_decompress_struct *cinfo)
```

Initializes the decompression object.

**Example:**

```c
#include <stdio.h>
#include <jpeglib.h>

int main(void) {
    struct jpeg_decompress_struct cinfo;
    struct jpeg_error_mgr jerr;
    cinfo.err = jpeg_std_error(&jerr);
    jpeg_create_decompress(&cinfo);
    jpeg_destroy_decompress(&cinfo);
    return 0;
}
```

#### jpeg_stdio_src

```c
void jpeg_stdio_src(struct jpeg_decompress_struct *cinfo, FILE *infile)
```

Sets the input file for the decompressor.

#### jpeg_read_header

```c
int jpeg_read_header(struct jpeg_decompress_struct *cinfo, boolean require_image)
```

---

### Cleanup

#### jpeg_destroy_decompress

```c
void jpeg_destroy_decompress(struct jpeg_decompress_struct *cinfo)
```