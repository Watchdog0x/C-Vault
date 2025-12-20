# string.h - String Handling

The `string.h` header defines functions that manipulate arrays of characters and other objects treated as arrays of characters.

## Facilities Overview

| Facility Category | Key Symbols | Description |
|-------------------|-------------|-------------|
| Copying | strcpy, strncpy, memcpy, memmove | Duplicating data between memory regions. |
| Concatenation | strcat, strncat | Appending one string to another. |
| Comparison | strcmp, strncmp, memcmp | Testing equality and ordering. |
| Search | strchr, strrchr, strstr, strtok | Locating characters or substrings. |
| Measurement | strlen | Calculating the length of strings. |
| Miscellaneous | strerror, memset | Error reporting and memory filling. |

---

## Types & Variables

### size_t

Unsigned integer type used for representing sizes of objects.

---

## Functions

### Copying

#### strcpy

```c
char *strcpy(char *dest, const char *src)
```

Copies the string pointed to by `src` (including the null terminator) to the buffer pointed to by `dest`.

**Example:**

```c
#include <string.h>
#include <stdio.h>

int main(void) {
    char src[] = "Hello";
    char dest[10];
    strcpy(dest, src);
    printf("dest: %s\n", dest);
    return 0;
}
```

#### strncpy

```c
char *strncpy(char *dest, const char *src, size_t n)
```

Copies at most `n` characters from `src` to `dest`.

#### memcpy

```c
void *memcpy(void *dest, const void *src, size_t n)
```

Copies `n` bytes from `src` to `dest`. The memory areas **must not overlap**.

#### memmove

```c
void *memmove(void *dest, const void *src, size_t n)
```

Copies `n` bytes from `src` to `dest`. Memory areas **may overlap**.

---

### Comparison

#### strcmp

```c
int strcmp(const char *s1, const char *s2)
```

Compares two strings lexicographically.

#### strncmp

```c
int strncmp(const char *s1, const char *s2, size_t n)
```

Compares at most `n` characters of two strings.

---

### Search

#### strchr

```c
char *strchr(const char *s, int c)
```

Returns a pointer to the first occurrence of character `c` in string `s`.

#### strstr

```c
char *strstr(const char *haystack, const char *needle)
```

Finds the first occurrence of the substring `needle` in `haystack`.

---

### Measurement

#### strlen

```c
size_t strlen(const char *s)
```

Returns the number of characters in the string `s`, excluding the null terminator.

---

### Miscellaneous

#### memset

```c
void *memset(void *s, int c, size_t n)
```

Fills the first `n` bytes of memory area `s` with the constant byte `c`.

#### strerror

```c
char *strerror(int errnum)
```

Returns a pointer to the textual representation of system error `errnum`.