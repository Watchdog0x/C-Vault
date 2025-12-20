# zlib.h - Data Compression Library

The `zlib.h` header provides functions for in-memory compression and decompression using the DEFLATE algorithm.

## Facilities Overview

| Facility Category | Key Symbols | Description |
|-------------------|-------------|-------------|
| Stream Mgmt | deflateInit, inflateInit | Initializing the compression/decompression state. |
| Processing | deflate, inflate | Compressing or decompressing data. |
| Utility | compress, uncompress | Simple, all-in-one buffer functions. |
| Cleanup | deflateEnd, inflateEnd | Releasing internal buffers. |

---

## Types & Variables

### z_stream

The core structure for communicating with zlib, containing buffers for input and output.

```c
typedef struct z_stream_s {
    z_const Bytef *next_in;     /* next input byte */
    uInt     avail_in;          /* bytes available at next_in */
    Bytef    *next_out;         /* next output byte */
    uInt     avail_out;         /* remaining space at next_out */
    uLong    total_out;         /* total bytes output so far */
    // ...
} z_stream;
```

---

## Functions

### Stream Compression (Deflate)

#### deflateInit

```c
int deflateInit(z_streamp strm, int level)
```

Initializes the internal stream state for compression.

**Example:**

```c
#include <zlib.h>

int main(void) {
    z_stream strm = {0};
    if (deflateInit(&strm, Z_DEFAULT_COMPRESSION) == Z_OK) {
        deflateEnd(&strm);
    }
    return 0;
}
```

#### deflate

```c
int deflate(z_streamp strm, int flush)
```

Compresses as much data as possible.

#### deflateEnd

```c
int deflateEnd(z_streamp strm)
```

---

### Stream Decompression (Inflate)

#### inflateInit

```c
int inflateInit(z_streamp strm)
```

Initializes the internal stream state for decompression.

#### inflate

```c
int inflate(z_streamp strm, int flush)
```