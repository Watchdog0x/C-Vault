# 🛠️ C & Linux Developer Reference

A comprehensive, professional-grade documentation library covering the ISO C Standard Library, POSIX/Linux System Calls, and Tier-1 Third-Party Libraries. 

> [!NOTE]
> **Documentation Philosophy**: This library is inspired by the **Go Standard Library**. It prioritizes concise signatures, high-density information tables, and small, runnable code examples over verbose prose.

---

## 📚 Library Contents

### 📂 [std/](std/) - C Standard Library (ISO C)
*The core language foundations from C89 to C11.*

| Module | Header | Description |
|:---|:---|:---|
| [io](std/io.md) | stdio.h | Standard Input/Output, Formatted I/O, File access. |
| [lib](std/lib.md) | stdlib.h | Memory management (`malloc`), Process control, Conversions. |
| [string](std/string.md) | string.h | String manipulation and memory block operations. |
| [math](std/math.md) | math.h | Floating-point arithmetic. |
| [complex](std/complex.md) | complex.h | Complex number arithmetic. |
| [threads](std/threads.md) | threads.h | Standard multithreading (C11). |
| [atomic](std/atomic.md) | stdatomic.h | Atomic operations. |
| [time](std/time.md) | time.h | System time, Intervals, and Calendar conversion. |
| [wchar](std/wchar.md) | wchar.h | Wide character and multibyte string handling. |
| [signal](std/signal.md) | signal.h | Signal handling and standard signal types. |
| [errno](std/errno.md) | errno.h | Error reporting macros and constants. |

### 📑 Specialized Utilities
*Support headers and type-specific definitions.*

| Module | Header | Description |
|:---|:---|:---|
| [int](std/int.md) | stdint.h | Fixed-width integers. |
| [inttypes](std/inttypes.md) | inttypes.h | Format conversion for integers. |
| [bool](std/bool.md) | stdbool.h | Boolean types. |
| [iso646](std/iso646.md) | iso646.h | Alternative operator spellings. |
| [assert](std/assert.md) | assert.h | Diagnostics. |
| [ctype](std/ctype.md) | ctype.h | Character classification. |
| [locale](std/locale.md) | locale.h | Localization. |
| [float](std/float.md) | float.h | Implementation-defined floating-point limits. |
| [limits](std/limits.md) | limits.h | Implementation-defined integer limits. |
| [def](std/def.md) | stddef.h | Common definitions. |
| [arg](std/arg.md) | stdarg.h | Variable arguments. |
| [setjmp](std/setjmp.md) | setjmp.h | Non-local jumps. |
| [fenv](std/fenv.md) | fenv.h | Floating-point environment. |
| [tgmath](std/tgmath.md) | tgmath.h | Type-generic math. |
| [align](std/align.md) | stdalign.h | Alignment support. |

---

### 📂 [sys/](sys/) - Linux & POSIX API
*Low-level operating system interfaces for systems programming.*

| Module | Header | Description |
|:---|:---|:---|
| [unistd](sys/unistd.md) | unistd.h | System calls for I/O, Fork, and process control. |
| [fcntl](sys/fcntl.md) | fcntl.h | File control operations. |
| [ioctl](sys/ioctl.md) | sys/ioctl.h | Device control operations. |
| [socket](sys/socket.md) | sys/socket.h | TCP/UDP and Unix Domain networking. |
| [pthread](sys/pthread.md) | pthread.h | POSIX threads. |
| [stat](sys/stat.md) | sys/stat.h | Filesystem metadata. |
| [dirent](sys/dirent.md) | dirent.h | Directory traversal. |
| [mman](sys/mman.md) | sys/mman.h | Memory mapping and protection. |
| [wait](sys/wait.md) | sys/wait.h | Process synchronization. |
| [poll](sys/poll.md) | poll.h | I/O multiplexing. |
| [utsname](sys/utsname.md) | sys/utsname.h | Kernel and version information. |

---

### 📂 [Graphics & Display](wayland/)
*High-performance display protocols and rendering APIs.*

| Module | Header | Description |
|:---|:---|:---|
| [client](wayland/client.md) | wayland-client.h | Wayland application API. |
| [server](wayland/server.md) | wayland-server.h | Wayland compositor API. |
| [util](wayland/util.md) | wayland-util.h | Wayland shared utilities. |
| [core](drm/core.md) | xf86drm.h | DRM device access. |
| [mode](drm/mode.md) | xf86drmMode.h | DRM mode-setting. |
| [gl](gl/gl.md) | GL/gl.h | OpenGL GPU rendering pipeline. |
| [sdl](sdl/sdl.md) | SDL2/SDL.h | Multimedia, audio, and events. |

---

### 📂 [Third-Party Ecosystem](glib/)
*Essential toolkits for modern C application development.*

| Module | Header | Description |
|:---|:---|:---|
| [glib](glib/glib.md) | glib.h | Data structures and event loop. |
| [ssl](ssl/ssl.md) | openssl/ssl.h | Secure I/O. |
| [crypto](ssl/crypto.md) | openssl/crypto.h | EVP cryptography. |
| [curl](curl/curl.md) | curl/curl.h | HTTP and transfer client. |
| [sqlite3](sql/sqlite3.md) | sqlite3.h | Embedded database. |
| [zlib](zlib/zlib.md) | zlib.h | DEFLATE compression. |
| [png](image/png.md) | png.h | PNG image handling. |
| [jpeg](image/jpeg.md) | jpeglib.h | JPEG image handling. |

---

## 🏛️ Document Structure
Every document in this library adheres to a strict conditional convention:

1. **Facilities Overview**: High-level categorization table.
2. **Types & Variables**: Present ONLY if the header defines structs, opaque handles, or data-carrying macros.
3. **Functions**: Present ONLY if the header defines callable operations. Grouped by category where density is high.

---

*This reference is generated and maintained for professional Linux systems development.*
