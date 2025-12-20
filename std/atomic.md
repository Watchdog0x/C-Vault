# stdatomic.h - Atomic Operations

The `stdatomic.h` header (C11) defines types and functions for performing atomic operations.

## Facilities Overview

| Facility Category | Key Symbols | Description |
|-------------------|-------------|-------------|
| Types | atomic_int, atomic_bool | Types that support atomic operations. |
| Operations | atomic_store, atomic_load | Mutating data without race conditions. |
| Synchronization | atomic_thread_fence | Memory order and synchronization barriers. |

---

## Types & Variables

### atomic_int

Standard atomic integer type.

### memory_order

Enumerated type identifying memory ordering constraints.

---

## Functions

### Atomic Operations

#### atomic_store

```c
void atomic_store(volatile A *object, M desired)
```

Atomically replaces the value of `object` with `desired`.

#### atomic_load

```c
C atomic_load(volatile A *object)
```

Atomically returns the value of `object`.

#### atomic_fetch_add

```c
C atomic_fetch_add(volatile A *object, M operand)
```

**Example:**

```c
#include <stdatomic.h>
#include <stdio.h>

int main(void) {
    atomic_int counter = 0;
    atomic_fetch_add(&counter, 1);
    printf("Counter: %d\n", atomic_load(&counter));
    return 0;
}
```

#### atomic_fetch_sub

```c
C atomic_fetch_sub(volatile A *object, M operand)
```

---

### Synchronization

#### atomic_thread_fence

```c
void atomic_thread_fence(memory_order order)
```