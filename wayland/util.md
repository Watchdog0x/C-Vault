# wayland-util.h - Wayland Utilities

The `wayland-util.h` header defines low-level utility types and functions used by both the Wayland client and server libraries.

## Facilities Overview

| Facility Category | Key Symbols | Description |
|-------------------|-------------|-------------|
| Linked Lists | wl_list_init, wl_list_insert, wl_list_remove | Doubly-linked list implementation. |
| Fixed Point | wl_fixed_to_double, wl_fixed_from_double | Support for fixed-point arithmetic. |
| Arrays | wl_array_init, wl_array_add | Dynamic array management. |
| Protocol Support | wl_argument, wl_interface | Low-level protocol message handling. |

---

## Types & Variables

### struct wl_list

A doubly-linked list structure.

### wl_fixed_t

A 24.8 signed fixed-point number.

### struct wl_array

A resizable buffer of bytes.

---

## Functions

### Linked Lists

#### wl_list_init

```c
void wl_list_init(struct wl_list *list)
```

Initializes a list as an empty list (pointing to itself).

**Example:**

```c
#include <wayland-util.h>

int main(void) {
    struct wl_list my_list;
    wl_list_init(&my_list);
    return 0;
}
```

#### wl_list_insert

```c
void wl_list_insert(struct wl_list *list, struct wl_list *elm)
```

Inserts `elm` into the list after the pointed-to `list` element.

#### wl_list_remove

```c
void wl_list_remove(struct wl_list *elm)
```

Removes `elm` from the list.

---

### Fixed Point Arithmetic

#### wl_fixed_to_double

```c
double wl_fixed_to_double(wl_fixed_t f)
```

#### wl_fixed_from_double

```c
wl_fixed_t wl_fixed_from_double(double d)
```

---

### Dynamic Arrays

#### wl_array_init

```c
void wl_array_init(struct wl_array *array)
```