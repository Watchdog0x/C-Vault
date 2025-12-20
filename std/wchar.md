# wchar.h - Wide Character Handling

**Category:** 📂 std/ (C Standard Library)  
**Header:** `<wchar.h>`  
**Scope:** C89, C99, C11

The `wchar.h` header declares functions for manipulating wide character strings and formatted I/O.

## Facilities Overview

| Facility Category | Key Symbols | Description |
|-------------------|-------------|-------------|
| [Wide String Ops](#wide-string-operations) | [wcscpy](#wcscpy), [wcscat](#wcscat), [wcslen](#wcslen) | Wide character equivalents of `string.h`. |
| [Wide I/O](#wide-inputoutput) | [wprintf](#wprintf) | Formatted wide character input and output. |
| [Conversion](#conversion) |  | Converting between multibyte and wide characters. |

---

## Types & Variables

### wchar_t

```c
typedef int wchar_t;
```

An integer type whose range of values can represent distinct codes for all members of the largest extended character set.

### mbstate_t

```c
typedef struct mbstate_t {
    // Implementation-defined
} mbstate_t;
```

An object type used to hold the conversion state information for a multibyte character sequence.

---

## Functions

### Wide String Operations

### wcscpy

```c
wchar_t *wcscpy(wchar_t *dest, const wchar_t *src)
```

Copies the wide string `src` to `dest`.

<details><summary>Example</summary>

```c
#include <wchar.h>

int main(void) {
    wchar_t dest[20];
    wcscpy(dest, L"Hello");
    wprintf(L"%ls\n", dest); // Hello
    return 0;
}
```

</details>

### wcscat

```c
wchar_t *wcscat(wchar_t *dest, const wchar_t *src)
```

Appends the wide string `src` to `dest`.

<details><summary>Example</summary>

```c
#include <wchar.h>

int main(void) {
    wchar_t dest[20] = L"Hello ";
    wcscat(dest, L"World");
    wprintf(L"%ls\n", dest); // Hello World
    return 0;
}
```

</details>

### wcslen

```c
size_t wcslen(const wchar_t *s)
```

Computes the length of the wide string `s`.

<details><summary>Example</summary>

```c
#include <wchar.h>

int main(void) {
    wchar_t *str = L"Wide characters";
    wprintf(L"Length: %zu\n", wcslen(str));
    return 0;
}
```

</details>

### wcscmp

```c
int wcscmp(const wchar_t *s1, const wchar_t *s2)
```

Compares two wide strings.

<details><summary>Example</summary>

```c
#include <wchar.h>

int main(void) {
    if (wcscmp(L"abc", L"abc") == 0) {
        wprintf(L"Strings equal\n");
    }
    return 0;
}
```

</details>

### wcschr

```c
wchar_t *wcschr(const wchar_t *s, wchar_t c)
```

Finds the first occurrence of `c` in wide string `s`.

<details><summary>Example</summary>

```c
#include <wchar.h>

int main(void) {
    wchar_t *ptr = wcschr(L"Hello", L'l');
    if (ptr) wprintf(L"Found at: %ls\n", ptr);
    return 0;
}
```

</details>

### Wide Input/Output

### wprintf

```c
int wprintf(const wchar_t *format, ...)
```

The wide-character equivalent of `printf`.

<details><summary>Example</summary>

```c
#include <wchar.h>

int main(void) {
    wprintf(L"Wide output: %ls\n", L"Hello");
    return 0;
}
```

</details>

### fwprintf

```c
int fwprintf(FILE *restrict stream, const wchar_t *restrict format, ...)
```

Wide-character output to a file stream.

### swprintf

```c
int swprintf(wchar_t *restrict s, size_t n, const wchar_t *restrict format, ...)
```

Wide-character output to a wide string.

### Conversion

### wcstombs

```c
size_t wcstombs(char *restrict s, const wchar_t *restrict pwcs, size_t n)
```

Converts a wide string to a multibyte string.

<details><summary>Example</summary>

```c
#include <wchar.h>

int main(void) {
    char mbs[20];
    size_t len = wcstombs(mbs, L"Hello", 20);
    printf("Converted: %s, Length: %zu\n", mbs, len);
    return 0;
}
```

</details>

### mbstowcs

```c
size_t mbstowcs(wchar_t *restrict pwcs, const char *restrict s, size_t n)
```

Converts a multibyte string to a wide string.

<details><summary>Example</summary>

```c
#include <wchar.h>

int main(void) {
    wchar_t wcs[20];
    size_t len = mbstowcs(wcs, "Hello", 20);
    wprintf(L"Converted length: %zu\n", len);
    return 0;
}
```

</details>

---

### wctype.h - Classification

The `wctype.h` functionality for wide character classification and mapping is also supported via:

- `iswalpha`, `iswdigit`: Wide character classification.
- `towlower`, `towupper`: Wide character case mapping.
