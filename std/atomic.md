# stdatomic.h - Atomic Operations

**Category:** 📂 std/ (C Standard Library)  
**Header:** `<stdatomic.h>`  
**Scope:** C11

The `stdatomic.h` header (C11) defines types and functions for performing atomic operations.

## Facilities Overview

| Facility Category | Key Symbols | Description |
|-------------------|-------------|-------------|
| [Types](#types--variables) | [atomic_int](#atomic_int), [atomic_bool](#atomic_bool) | Types that support atomic operations. |
| [Operations](#atomic-operations) | [atomic_store](#atomic_store), [atomic_load](#atomic_load), [atomic_fetch_add](#atomic_fetch_add) | Mutating data without race conditions. |
| [Synchronization](#synchronization) | [atomic_thread_fence](#atomic_thread_fence) | Memory order and synchronization barriers. |

---

## Types & Variables

### atomic_int

```c
typedef _Atomic int atomic_int;
```

Standard atomic integer type.

### atomic_bool

```c
typedef _Atomic _Bool atomic_bool;
```

Atomic boolean type.

### memory_order

```c
typedef enum memory_order {
    memory_order_relaxed,
    memory_order_consume,
    memory_order_acquire,
    memory_order_release,
    memory_order_acq_rel,
    memory_order_seq_cst
} memory_order;
```

Enumerated type identifying memory ordering constraints:

- `memory_order_relaxed`: No synchronization or ordering constraints.
- `memory_order_consume`: Consume operation (deprecated).
- `memory_order_acquire`: Acquire operation.
- `memory_order_release`: Release operation.
- `memory_order_acq_rel`: Acquire and release operation.
- `memory_order_seq_cst`: Sequentially consistent operation (default).

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

Atomically subtracts `operand` from `object` and returns the previous value.

**Example:**

```c
#include <stdatomic.h>

int main(void) {
    atomic_int val = 10;
    int prev = atomic_fetch_sub(&val, 3);
    // prev = 10, val = 7
    return 0;
}
```

#### atomic_exchange

```c
C atomic_exchange(volatile A *object, M desired)
```

Atomically replaces the value of `object` with `desired` and returns the previous value.

**Example:**

```c
#include <stdatomic.h>
#include <stdio.h>

int main(void) {
    atomic_int val = 10;
    int old = atomic_exchange(&val, 20);
    printf("Old: %d, New: %d\n", old, atomic_load(&val)); // Old: 10, New: 20
    return 0;
}
```

#### atomic_compare_exchange_strong

```c
_Bool atomic_compare_exchange_strong(volatile A *object, C *expected, M desired)
```

Atomically compares `object` with `*expected`, and if equal, replaces with `desired`. Returns true if successful.

---

### Synchronization

#### atomic_thread_fence

```c
void atomic_thread_fence(memory_order order)
```

Establishes a memory synchronization order between threads.

**Example:**

```c
#include <stdatomic.h>

int main(void) {
    atomic_thread_fence(memory_order_acquire);
    // Operations after fence are not reordered before
    return 0;
}
```

#### atomic_signal_fence

```c
void atomic_signal_fence(memory_order order)
```

Establishes a memory synchronization order within a thread for signal handlers.

**Example:**

```c
#include <stdatomic.h>

int main(void) {
    atomic_signal_fence(memory_order_seq_cst);
    // Synchronizes with signal handlers
    return 0;
}
```
