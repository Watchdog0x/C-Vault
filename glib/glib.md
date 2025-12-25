# glib.h - GLib Core Utility Library

**Category:** 📂 glib/ (Third-Party Ecosystem)
**Header:** `<glib.h>`
**Scope:** GNOME / Cross-platform C Utility

The `glib.h` header is the entry point for GLib, a general-purpose utility library that provides advanced data structures, type wrappers, and the main event loop used by GTK applications.

## Facilities Overview

| Facility Category | Key Symbols | Description |
|-------------------|-------------|-------------|
| [Memory Mgmt](#memory-management) | [g_malloc](#g_malloc), [g_free](#g_free) | Safe memory allocation wrappers. |
| [Linked Lists](#linked-lists) | [GList](#glist), [g_list_append](#g_list_append), [g_list_free](#g_list_free) | Doubly linked lists. |
| [Strings](#strings) | [GString](#gstring), [g_strdup](#g_strdup), [g_str_equal](#g_str_equal) | String manipulation and dynamic buffers. |
| [Event Loop](#event-loop) | [GMainLoop](#gmainloop), [g_main_loop_run](#g_main_loop_run) | Main event loop for async programming. |
| [Error Handling](#error-handling) | [GError](#gerror), [g_set_error](#g_set_error) | Standardized error reporting mechanism. |

---

## Types & Variables

### GList

```c
typedef struct _GList GList;
struct _GList {
  gpointer data;
  GList *next;
  GList *prev;
};
```

Structure for a doubly linked list.

### GString

```c
typedef struct _GString GString;
struct _GString {
  gchar  *str;
  gsize   len;
  gsize   allocated_len;
};
```

Dynamically growing string buffer.

### GError

```c
typedef struct _GError GError;
struct _GError {
  GQuark       domain;
  gint         code;
  gchar       *message;
};
```

Standard mechanism for returning errors from functions.

### GMainLoop

```c
typedef struct _GMainLoop GMainLoop;
```

Opaque structure representing a main event loop.

---

## Functions

### Memory Management

### g_malloc

```c
gpointer g_malloc(gsize n_bytes)
```

Allocates `n_bytes` of memory. If the allocation fails, the application is terminated.

**Returns:** A pointer to the allocated memory.

### g_free

```c
void g_free(gpointer mem)
```

Frees the memory pointed to by `mem`. If `mem` is NULL, it does nothing.

---

### Linked Lists

### g_list_append

```c
GList *g_list_append(GList *list, gpointer data)
```

Adds a new element to the end of the list.

**Returns:** The new start of the `GList`.

<details><summary>Example</summary>

```c
#include <glib.h>
#include <stdio.h>

int main(void) {
    GList *list = NULL;
    list = g_list_append(list, "Hello");
    list = g_list_append(list, "GLib");
    
    // Iterate
    for (GList *l = list; l != NULL; l = l->next) {
        printf("Item: %s\n", (char *)l->data);
    }
    
    g_list_free(list);
    return 0;
}
```

</details>

### g_list_free

```c
void g_list_free(GList *list)
```

Frees all of the memory used by a `GList`. The freed elements are returned to the slice allocator. (Does not free the data pointers).

---

### Strings

### g_strdup

```c
gchar *g_strdup(const gchar *str)
```

Duplicates a string.

**Returns:** A newly allocated string which matches `str`. usage `g_free()` to release it.

### g_str_equal

```c
gboolean g_str_equal(gconstpointer v1, gconstpointer v2)
```

Compares two strings for equality.

**Returns:** `TRUE` if the strings are the same, `FALSE` otherwise.

---

### Event Loop

### g_main_loop_new

```c
GMainLoop *g_main_loop_new(GMainContext *context, gboolean is_running)
```

Creates a new GMainLoop structure.

**Returns:** A new `GMainLoop`.

### g_main_loop_run

```c
void g_main_loop_run(GMainLoop *loop)
```

Runs a main loop until `g_main_loop_quit()` is called on the loop.

---

### Error Handling

### g_set_error

```c
void g_set_error(GError **err, GQuark domain, gint code, const gchar *format, ...)
```

Sets `err` to a new `GError` created from the arguments.