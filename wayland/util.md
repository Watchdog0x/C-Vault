# wayland-util.h - Wayland Utilities

**Category:** 📂 wayland/ (Wayland Protocol)
**Header:** `<wayland-util.h>`
**Scope:** Client and Server shared utilities

The `wayland-util.h` header defines low-level utility types and functions used by both the Wayland client and server libraries. It includes a doubly-linked list implementation, fixed-point arithmetic support, and dynamic arrays.

## Facilities Overview

| Facility Category | Key Symbols | Description |
|-------------------|-------------|-------------|
| [Linked Lists](#linked-lists) | [wl_list_init](#wl_list_init), [wl_list_insert](#wl_list_insert), [wl_list_remove](#wl_list_remove) | Doubly-linked list implementation. |
| [Fixed Point](#fixed-point-arithmetic) | [wl_fixed_to_double](#wl_fixed_to_double), [wl_fixed_from_double](#wl_fixed_from_double) | Support for fixed-point arithmetic. |
| [Arrays](#dynamic-arrays) | [wl_array_init](#wl_array_init), [wl_array_add](#wl_array_add) | Dynamic array management. |
| [Protocol Support](#protocol-support) | [wl_argument](#wl_argument), [wl_interface](#wl_interface) | Low-level protocol message handling. |

---

## Types & Variables

### struct wl_list

```c
struct wl_list {
    struct wl_list *prev;
    struct wl_list *next;
};
```

A transparent doubly-linked list structure. Clients and servers allow embedding this into their own structures to create lists of objects.

### wl_fixed_t

```c
typedef int32_t wl_fixed_t;
```

A 24.8 signed fixed-point number.

### struct wl_array

```c
struct wl_array {
    size_t size;
    size_t alloc;
    void *data;
};
```

A resizable buffer of bytes.

---

## Functions

### Linked Lists

### wl_list_init

```c
void wl_list_init(struct wl_list *list)
```

Initializes a list as an empty list (pointing to itself).

**Returns:** None.

<details><summary>Example</summary>

```c
#include <wayland-util.h>

int main(void) {
    struct wl_list my_list;
    wl_list_init(&my_list);
    return 0;
}
```

</details>

### wl_list_insert

```c
void wl_list_insert(struct wl_list *list, struct wl_list *elm)
```

Inserts `elm` into the list after the pointed-to `list` element.

**Returns:** None.

<details><summary>Example</summary>

```c
#include <wayland-util.h>
#include <stdlib.h>

struct my_item {
    struct wl_list link;
    int data;
};

int main(void) {
    struct wl_list list;
    wl_list_init(&list);

    struct my_item *item = malloc(sizeof(struct my_item));
    item->data = 42;
    wl_list_insert(&list, &item->link);

    // cleanup...
    return 0;
}
```

</details>

### wl_list_remove

```c
void wl_list_remove(struct wl_list *elm)
```

Removes `elm` from the list. It leaves `elm` in an undefined state.

**Returns:** None.

<details><summary>Example</summary>

```c
#include <wayland-util.h>
#include <stdlib.h>

void remove_item(struct my_item *item) {
    wl_list_remove(&item->link);
    free(item);
}
```

</details>

### wl_list_empty

```c
int wl_list_empty(const struct wl_list *list)
```

Checks if the list is empty.

**Returns:** 1 if empty, 0 otherwise.

### wl_list_length

```c
int wl_list_length(const struct wl_list *list)
```

Counts the number of elements in the list.

**Returns:** The number of elements.

---

### Fixed Point Arithmetic

### wl_fixed_to_double

```c
double wl_fixed_to_double(wl_fixed_t f)
```

Converts a fixed-point number to a double.

**Returns:** The double definition of the fixed-point value.

<details><summary>Example</summary>

```c
#include <wayland-util.h>
#include <stdio.h>

int main(void) {
    wl_fixed_t fixed = wl_fixed_from_double(1.5);
    double d = wl_fixed_to_double(fixed);
    printf("Fixed to double: %f\n", d); // Output: 1.500000
    return 0;
}
```

</details>

### wl_fixed_from_double

```c
wl_fixed_t wl_fixed_from_double(double d)
```

Converts a double to a fixed-point number.

**Returns:** The fixed-point equivalent of `d`.

### wl_fixed_to_int

```c
int wl_fixed_to_int(wl_fixed_t f)
```

Converts a fixed-point number to an integer (truncates).

**Returns:** The integer part of `f`.

### wl_fixed_from_int

```c
wl_fixed_t wl_fixed_from_int(int i)
```

Converts an integer to a fixed-point number.

**Returns:** The fixed-point equivalent of `i`.

---

### Dynamic Arrays

### wl_array_init

```c
void wl_array_init(struct wl_array *array)
```

Initializes an array as empty.

**Returns:** None.

<details><summary>Example</summary>

```c
#include <wayland-util.h>

int main(void) {
    struct wl_array array;
    wl_array_init(&array);

    // Use array...

    wl_array_release(&array);
    return 0;
}
```

</details>

### wl_array_release

```c
void wl_array_release(struct wl_array *array)
```

Releases the memory used by the array.

**Returns:** None.

### wl_array_add

```c
void *wl_array_add(struct wl_array *array, size_t size)
```

Adds an element of `size` bytes to the end of the array.

**Returns:** A pointer to the newly allocated space, or `NULL` on failure.

### wl_array_copy

```c
int wl_array_copy(struct wl_array *array, struct wl_array *source)
```

Copies the contents of one array to another.

**Returns:** 0 on success, -1 on failure.

