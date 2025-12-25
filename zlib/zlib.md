# zlib.h - Data Compression Library

**Category:** 📂 zlib/ (Third-Party Ecosystem)
**Header:** `<zlib.h>`
**Scope:** DEFLATE Compression

The `zlib.h` header provides functions for in-memory compression and decompression using the DEFLATE algorithm (LZ77 + Huffman coding). It is the standard compression library for PNG, HTTP (gzip), and many other formats.

## Facilities Overview

| Facility Category | Key Symbols | Description |
|-------------------|-------------|-------------|
| [Stream Mgmt](#stream-management) | [deflateInit](#deflateinit), [inflateInit](#inflateinit) | Initializing the compression/decompression state. |
| [Processing](#processing) | [deflate](#deflate), [inflate](#inflate) | Compressing or decompressing data chunks. |
| [Cleanup](#cleanup) | [deflateEnd](#deflateend), [inflateEnd](#inflateend) | Releasing internal buffers. |
| [Utility](#utility) | [compress](#compress), [uncompress](#uncompress) | Simple, single-buffer convenience functions. |

---

## Types & Variables

### z_stream

```c
typedef struct z_stream_s {
    z_const Bytef *next_in;     /* next input byte */
    uInt     avail_in;          /* bytes available at next_in */
    uLong    total_in;          /* total number of input bytes read so far */
    
    Bytef    *next_out;         /* next output byte should be put there */
    uInt     avail_out;         /* remaining free space at next_out */
    uLong    total_out;         /* total number of bytes output so far */
    
    char     *msg;              /* last error message, NULL if no error */
    // ... internal state ...
} z_stream;
```

The core structure for communicating with zlib, containing buffers for input and output.

---

## Functions

### Stream Management

### deflateInit

```c
int deflateInit(z_streamp strm, int level)
```

Initializes the internal stream state for compression.
*   `level`: 0 (no compression) to 9 (best compression), or `Z_DEFAULT_COMPRESSION` (-1).

**Returns:** `Z_OK` on success. `Z_MEM_ERROR` if memory could not be allocated.

<details><summary>Example</summary>

```c
#include <zlib.h>
#include <stdio.h>
#include <string.h>

int main(void) {
    z_stream strm = {0};
    // Initialize for default compression
    if (deflateInit(&strm, Z_DEFAULT_COMPRESSION) != Z_OK) {
        fprintf(stderr, "deflateInit failed\n");
        return 1;
    }
    
    deflateEnd(&strm);
    return 0;
}
```

</details>

### inflateInit

```c
int inflateInit(z_streamp strm)
```

Initializes the internal stream state for decompression.

**Returns:** `Z_OK` on success.

---

### Processing

### deflate

```c
int deflate(z_streamp strm, int flush)
```

Compresses as much data as possible, and stops when the input buffer becomes empty or the output buffer becomes full.

*   `flush`: `Z_NO_FLUSH`, `Z_SYNC_FLUSH`, `Z_FINISH`, etc.

**Returns:** `Z_OK` if some progress has been made. `Z_STREAM_END` if all input has been consumed and all output has been produced (only when `flush` is `Z_FINISH`).

### inflate

```c
int inflate(z_streamp strm, int flush)
```

Decompresses data.

**Returns:** `Z_OK` on progress. `Z_STREAM_END` if the end of the compressed data has been reached.

---

### Cleanup

### deflateEnd

```c
int deflateEnd(z_streamp strm)
```

Frees all dynamically allocated data structures for this stream.

**Returns:** `Z_OK` on success.

### inflateEnd

```c
int inflateEnd(z_streamp strm)
```

Frees all dynamically allocated data structures for this stream.

**Returns:** `Z_OK` on success.

---

### Utility

### compress

```c
int compress(Bytef *dest, uLongf *destLen, const Bytef *source, uLong sourceLen)
```

Compresses the source buffer into the destination buffer.

**Returns:** `Z_OK` on success.