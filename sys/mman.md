# sys/mman.h - Memory Management

The `sys/mman.h` header defines functions for memory mapping.

## Facilities Overview

| Facility Category | Key Symbols | Description |
|-------------------|-------------|-------------|
| Mapping | mmap, munmap | Mapping files or devices into memory. |
| Protection | mprotect | Changing access protections on memory. |
| Synchronization | msync | Flushing memory changes to disk. |

---

## Types & Variables

### Protection Flags

- `PROT_READ`: Pages may be read.
- `PROT_WRITE`: Pages may be written.
- `PROT_EXEC`: Pages may be executed.
- `PROT_NONE`: Pages may not be accessed.

### Mapping Flags

- `MAP_SHARED`: Share this mapping.
- `MAP_PRIVATE`: Create a private copy-on-write mapping.
- `MAP_ANONYMOUS`: Mapping is not backed by any file.
- `MAP_FIXED`: Interpret addr exactly.

---

## Functions

### Memory Mapping

#### mmap

```c
void *mmap(void *addr, size_t length, int prot, int flags, int fd, off_t offset)
```

Creates a new mapping in the virtual address space of the calling process.

#### munmap

```c
int munmap(void *addr, size_t length)
```

Deletes the mappings for the specified address range.

**Example:**

```c
#include <sys/mman.h>
#include <unistd.h>
#include <stdio.h>

int main(void) {
    void *ptr = mmap(NULL, 4096, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0);
    if (ptr == MAP_FAILED) {
        perror("mmap");
        return 1;
    }
    // Use memory...
    munmap(ptr, 4096);
    return 0;
}
```