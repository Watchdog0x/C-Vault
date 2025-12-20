# glib.h - GLib Core Utility Library

The `glib.h` header is the entry point for GLib, a general-purpose utility library that provides advanced data structures and abstractions.

## Facilities Overview

| Facility Category | Key Symbols | Description |
|-------------------|-------------|-------------|
| Linked Lists | GList, GSList, g_list_append | Doubly and singly linked lists. |
| Hash Tables | GHashTable, g_hash_table_new | Key-value associative arrays. |
| Event Loop | GMainLoop, g_main_loop_run | Main event loop for async programming. |
| Strings | GString, g_string_new | Dynamically growing byte strings. |
| Error Handling | GError, g_set_error | Standardized error reporting mechanism. |
| Memory | g_malloc, g_free | Efficient memory management. |

---

## Types & Variables

### GList

Structure for a doubly linked list.

### GSList

Structure for a singly linked list.

### GHashTable

Opaque structure representing a hash table.

### GMainLoop

Opaque structure representing a main event loop.

### GString

```c
struct GString {
    gchar *str;   /* Null-terminated string data */
    gsize len;    /* Length of the string */
    gsize allocated_len;
};
```

### GError

```c
struct GError {
  GQuark       domain;
  gint         code;
  gchar       *message;
};
```

---

## Functions

### Linked Lists

#### g_list_append

```c
GList *g_list_append(GList *list, gpointer data)
```

Adds a new element to the end of the list.

**Example:**

```c
#include <glib.h>
#include <stdio.h>

int main(void) {
    GList *list = NULL;
    list = g_list_append(list, "Hello");
    list = g_list_append(list, "GLib");
    
    for (GList *l = list; l != NULL; l = l->next) {
        printf("%s\n", (char *)l->data);
    }
    
    g_list_free(list);
    return 0;
}
```

### Event Loop

#### g_main_loop_new

```c
GMainLoop *g_main_loop_new(GMainContext *context, gboolean is_running)
```

Creates a new GMainLoop structure.

#### g_main_loop_run

```c
void g_main_loop_run(GMainLoop *loop)
```

Runs a main loop until `g_main_loop_quit()` is called.

### Error Handling

#### g_set_error

```c
void g_set_error(GError **err, GQuark domain, gint code, const gchar *format, ...)
```

Sets the error if `err` is non-NULL.