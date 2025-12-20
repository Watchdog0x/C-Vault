# string.h - String Handling

**Category:** 📂 std/ (C Standard Library)  
**Header:** `<string.h>`  
**Scope:** C89, C99, C11

The `string.h` header defines functions that manipulate arrays of characters and other objects treated as arrays of characters.

## Facilities Overview

| Facility Category | Key Symbols | Description |
|-------------------|-------------|-------------|
| [Copying](#copying) | [strcpy](#strcpy), [strncpy](#strncpy), [memcpy](#memcpy), [memmove](#memmove) | Duplicating data between memory regions. |
| [Concatenation](#concatenation) | [strcat](#strcat), [strncat](#strncat) | Appending one string to another. |
| [Comparison](#comparison) | [strcmp](#strcmp), [strncmp](#strncmp), [memcmp](#memcmp) | Testing equality and ordering. |
| [Search](#search) | [strchr](#strchr), [strrchr](#strrchr), [strstr](#strstr), [strtok](#strtok) | Locating characters or substrings. |
| [Measurement](#measurement) | [strlen](#strlen) | Calculating the length of strings. |
| [Miscellaneous](#miscellaneous) | [strerror](#strerror), [memset](#memset) | Error reporting and memory filling. |

---

## Types & Variables

### size_t

```c
typedef unsigned long size_t;
```

Unsigned integer type used for representing sizes of objects and return values for string operations.

---

## Functions

### Copying

### strcpy

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
    printf("dest: %s\n", dest); // Output: dest: Hello
    return 0;
}
```

### strncpy

```c
char *strncpy(char *dest, const char *src, size_t n)
```

Copies at most `n` characters from `src` to `dest`. If `src` is shorter than `n`, the remainder is padded with null bytes.

**Example:**

```c
#include <string.h>
#include <stdio.h>

int main(void) {
    char src[] = "Hello World";
    char dest[6];
    strncpy(dest, src, 5);
    dest[5] = '\0'; // Ensure null termination
    printf("dest: %s\n", dest); // Output: dest: Hello
    return 0;
}
```

### memcpy

```c
void *memcpy(void *dest, const void *src, size_t n)
```

Copies `n` bytes from `src` to `dest`. The memory areas **must not overlap**.

**Example:**

```c
#include <string.h>
#include <stdio.h>

int main(void) {
    int src[] = {1, 2, 3, 4, 5};
    int dest[5];
    memcpy(dest, src, 5 * sizeof(int));
    for (int i = 0; i < 5; i++) {
        printf("%d ", dest[i]); // Output: 1 2 3 4 5
    }
    printf("\n");
    return 0;
}
```

### memmove

```c
void *memmove(void *dest, const void *src, size_t n)
```

Copies `n` bytes from `src` to `dest`. Memory areas **may overlap**. Safer than `memcpy` for overlapping regions.

**Example:**

```c
#include <string.h>
#include <stdio.h>

int main(void) {
    char str[] = "Hello World";
    // Shift string 6 characters to the left
    memmove(str, str + 6, 6); // Can overlap safely
    printf("%s\n", str); // Output: World
    return 0;
}
```

---

### Concatenation

### strcat

```c
char *strcat(char *dest, const char *src)
```

Appends the string `src` to the end of `dest`. The destination buffer must be large enough to hold the result.

**Example:**

```c
#include <string.h>
#include <stdio.h>

int main(void) {
    char dest[20] = "Hello ";
    char src[] = "World";
    strcat(dest, src);
    printf("%s\n", dest); // Output: Hello World
    return 0;
}
```

### strncat

```c
char *strncat(char *dest, const char *src, size_t n)
```

Appends at most `n` characters from `src` to `dest`, then adds a null terminator.

**Example:**

```c
#include <string.h>
#include <stdio.h>

int main(void) {
    char dest[20] = "Hello ";
    char src[] = "World!!!";
    strncat(dest, src, 5);
    printf("%s\n", dest); // Output: Hello World
    return 0;
}
```

---

### Comparison

### strcmp

```c
int strcmp(const char *s1, const char *s2)
```

Compares two strings lexicographically. Returns:
* `0` if strings are equal
* Negative value if `s1` < `s2`
* Positive value if `s1` > `s2`

**Example:**

```c
#include <string.h>
#include <stdio.h>

int main(void) {
    const char *s1 = "apple";
    const char *s2 = "banana";
    int result = strcmp(s1, s2);
    
    if (result < 0) {
        printf("%s comes before %s\n", s1, s2); // This will execute
    } else if (result > 0) {
        printf("%s comes after %s\n", s1, s2);
    } else {
        printf("Strings are equal\n");
    }
    return 0;
}
```

### strncmp

```c
int strncmp(const char *s1, const char *s2, size_t n)
```

Compares at most `n` characters of two strings.

**Example:**

```c
#include <string.h>
#include <stdio.h>

int main(void) {
    const char *s1 = "Hello World";
    const char *s2 = "Hello Earth";
    
    if (strncmp(s1, s2, 5) == 0) {
        printf("First 5 characters are equal\n"); // This will execute
    }
    return 0;
}
```

### memcmp

```c
int memcmp(const void *s1, const void *s2, size_t n)
```

Compares the first `n` bytes of memory areas `s1` and `s2`.

**Example:**

```c
#include <string.h>
#include <stdio.h>

int main(void) {
    int arr1[] = {1, 2, 3, 4, 5};
    int arr2[] = {1, 2, 3, 0, 0};
    
    if (memcmp(arr1, arr2, 3 * sizeof(int)) == 0) {
        printf("First 3 elements are equal\n"); // This will execute
    }
    return 0;
}
```

---

### Search

### strchr

```c
char *strchr(const char *s, int c)
```

Returns a pointer to the first occurrence of character `c` in string `s`, or `NULL` if not found.

**Example:**

```c
#include <string.h>
#include <stdio.h>

int main(void) {
    const char *str = "Hello World";
    char *ptr = strchr(str, 'o');
    
    if (ptr != NULL) {
        printf("Found 'o' at position: %ld\n", ptr - str); // Output: 4
        printf("Substring from 'o': %s\n", ptr); // Output: o World
    }
    return 0;
}
```

### strrchr

```c
char *strrchr(const char *s, int c)
```

Returns a pointer to the last occurrence of character `c` in string `s`, or `NULL` if not found.

**Example:**

```c
#include <string.h>
#include <stdio.h>

int main(void) {
    const char *str = "Hello World";
    char *ptr = strrchr(str, 'o');
    
    if (ptr != NULL) {
        printf("Last 'o' at position: %ld\n", ptr - str); // Output: 7
        printf("Substring from last 'o': %s\n", ptr); // Output: orld
    }
    return 0;
}
```

### strstr

```c
char *strstr(const char *haystack, const char *needle)
```

Finds the first occurrence of the substring `needle` in `haystack`.

**Example:**

```c
#include <string.h>
#include <stdio.h>

int main(void) {
    const char *haystack = "Hello World";
    const char *needle = "World";
    char *ptr = strstr(haystack, needle);
    
    if (ptr != NULL) {
        printf("Found substring: %s\n", ptr); // Output: World
    } else {
        printf("Substring not found\n");
    }
    return 0;
}
```

### strtok

```c
char *strtok(char *str, const char *delim)
```

Breaks string `str` into a series of tokens separated by `delim`. Modifies the original string.

**Example:**

```c
#include <string.h>
#include <stdio.h>

int main(void) {
    char str[] = "apple,banana,cherry";
    const char *delim = ",";
    
    char *token = strtok(str, delim);
    while (token != NULL) {
        printf("%s\n", token);
        token = strtok(NULL, delim); // Continue tokenizing
    }
    // Output:
    // apple
    // banana
    // cherry
    return 0;
}
```

---

### Measurement

### strlen

```c
size_t strlen(const char *s)
```

Returns the number of characters in the string `s`, excluding the null terminator.

**Example:**

```c
#include <string.h>
#include <stdio.h>

int main(void) {
    const char *str = "Hello World";
    size_t len = strlen(str);
    printf("Length of '%s' is %zu\n", str, len); // Output: 11
    return 0;
}
```

---

### Miscellaneous

### memset

```c
void *memset(void *s, int c, size_t n)
```

Fills the first `n` bytes of memory area `s` with the constant byte `c`. Commonly used to zero out arrays.

**Example:**

```c
#include <string.h>
#include <stdio.h>

int main(void) {
    char buffer[10];
    memset(buffer, 0, sizeof(buffer)); // Zero out the buffer
    
    int arr[5];
    memset(arr, 0, sizeof(arr)); // Zero out the array
    
    for (int i = 0; i < 5; i++) {
        printf("%d ", arr[i]); // Output: 0 0 0 0 0
    }
    printf("\n");
    return 0;
}
```

### strerror

```c
char *strerror(int errnum)
```

Returns a pointer to the textual representation of system error `errnum`. Useful with `errno`.

**Example:**

```c
#include <string.h>
#include <stdio.h>
#include <errno.h>

int main(void) {
    FILE *fp = fopen("nonexistent.txt", "r");
    if (fp == NULL) {
        printf("Error: %s\n", strerror(errno));
        // Output: Error: No such file or directory
    }
    return 0;
}
```
