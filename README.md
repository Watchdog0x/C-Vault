# 🛠️ C & Linux Developer Reference

A comprehensive, professional-grade documentation library covering the ISO C Standard Library, POSIX/Linux System Calls, and Tier-1 Third-Party Libraries. 

> [!NOTE]
> **Documentation Philosophy**: This library is inspired by the **Go Standard Library**. It prioritizes concise signatures, high-density information tables, and small, runnable code examples over verbose prose.

---

## 📚 Library Contents

### 📂 [std/](std/) - C Standard Library (ISO C)
*The core language foundations from C89 to C11.*

| Module | Description |
|:---|:---|
| [io.md](std/io.md) | Standard Input/Output, Formatted I/O, File access. |
| [lib.md](std/lib.md) | Memory management (`malloc`), Process control, Conversions. |
| [string.md](std/string.md) | String manipulation and memory block operations. |
| [math.md](std/math.md) / [complex.md](std/complex.md) | Floating-point and complex number arithmetic. |
| [threads.md](std/threads.md) / [atomic.md](std/atomic.md) | Standard multithreading (C11) and atomic operations. |
| [time.md](std/time.md) | System time, Intervals, and Calendar conversion. |
| [wchar.md](std/wchar.md) | Wide character and multibyte string handling. |
| [signal.md](std/signal.md) | Signal handling and standard signal types. |
| [errno.md](std/errno.md) | Error reporting macros and constants. |

### 📑 Specialized Utilities
*Support headers and type-specific definitions.*

| Header | Description |
|:---|:---|
| [int.md](std/int.md) / [inttypes.md](std/inttypes.md) | Fixed-width integers and format conversion. |
| [bool.md](std/bool.md) / [iso646.md](std/iso646.md) | Boolean types and alternative operator spellings. |
| [assert.md](std/assert.md) / [errno.md](std/errno.md) | Diagnostics and error reporting. |
| [ctype.md](std/ctype.md) / [locale.md](std/locale.md) | Character classification and localization. |
| [float.md](std/float.md) / [limits.md](std/limits.md) | Implementation-defined numeric limits. |
| [def.md](std/def.md) / [arg.md](std/arg.md) | Common definitions and variable arguments. |
| [setjmp.md](std/setjmp.md) / [fenv.md](std/fenv.md) | Non-local jumps and floating-point environment. |
| [tgmath.md](std/tgmath.md) / [align.md](std/align.md) | Type-generic math and alignment support. |

---

### 📂 [sys/](sys/) - Linux & POSIX API
*Low-level operating system interfaces for systems programming.*

- **System Calls**: [unistd.md](sys/unistd.md) (I/O, Fork), [fcntl.md](sys/fcntl.md) (Control), [ioctl.md](sys/ioctl.md) (Device Control)
- **Networking**: [socket.md](sys/socket.md) (TCP/UDP, Unix Domain)
- **Threads**: [pthread.md](sys/pthread.md) (POSIX Threads)
- **Filesystem**: [stat.md](sys/stat.md) (Metadata), [dirent.md](sys/dirent.md) (Directory traversal)
- **Memory**: [mman.md](sys/mman.md) (Memory mapping/protection)
- **IPC & Signals**: [wait.md](sys/wait.md) (Process sync), [poll.md](sys/poll.md) (Multiplexing)
- **System Info**: [utsname.md](sys/utsname.md) (Kernel/Version information)

---

### 📂 [Graphics & Display](wayland/)
*High-performance display protocols and rendering APIs.*

- **Wayland**: [client.md](wayland/client.md) (Application API), [server.md](wayland/server.md) (Compositor API), [util.md](wayland/util.md) (Shared Utilities)
- **DRM/KMS**: [core.md](drm/core.md) (Device access), [mode.md](drm/mode.md) (Mode-setting)
- **OpenGL**: [gl.md](gl/gl.md) (Modern GPU Rendering Pipeline)
- **SDL2**: [sdl.md](sdl/sdl.md) (Multimedia, Audio, Events)

---

### 📂 [Third-Party Ecosystem](glib/)
*Essential toolkits for modern C application development.*

- 🛠️ **GLib**: [glib.md](glib/glib.md) (Data structures, Event loop)
- 🔒 **OpenSSL**: [ssl.md](ssl/ssl.md) (Secure I/O), [crypto.md](ssl/crypto.md) (EVP)
- 🌐 **libcurl**: [curl.md](curl/curl.md) (HTTP/Transfer Client)
- 🗄️ **SQLite**: [sqlite3.md](sql/sqlite3.md) (Embedded Database)
- 📉 **zlib**: [zlib.md](zlib/zlib.md) (DEFLATE Compression)
- 🖼️ **Images**: [png.md](image/png.md), [jpeg.md](image/jpeg.md)

---

## 🏛️ Document Structure
Every document in this library adheres to a strict conditional convention:

1. **Facilities Overview**: High-level categorization table.
2. **Types & Variables**: Present ONLY if the header defines structs, opaque handles, or data-carrying macros.
3. **Functions**: Present ONLY if the header defines callable operations. Grouped by category where density is high.

---

*This reference is generated and maintained for professional Linux systems development.*
