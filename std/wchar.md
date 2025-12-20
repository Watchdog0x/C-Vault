# wchar.h - Wide Character Handling

The `wchar.h` header declares functions for manipulating wide character strings and formatted I/O.

## Facilities Overview

| Facility Category | Key Symbols | Description |
|-------------------|-------------|-------------|
| Wide String Ops | wcscpy, wcscat, wcslen | Wide character equivalents of `string.h`. |
| Wide I/O | wprintf, wscanf | Formatted wide character input and output. |
| Conversion | wcrtomb, mbrtowc | Converting between multibyte and wide characters. |

---

## Types & Variables

### wchar_t

An integer type whose range of values can represent distinct codes for all members of the largest extended character set.

### mbstate_t

An object type used to hold the conversion state information for a multibyte character sequence.

---

## Functions

### Wide String Operations

#### wcslen

```c
size_t wcslen(const wchar_t *s)
```

Computes the length of the wide string `s`.

**Example:**

```c
#include <wchar.h>
#include <stdio.h>

int main(void) {
    wchar_t *str = L"Wide characters";
    wprintf(L"Length: %zu\n", wcslen(str));
    return 0;
}
```

### Wide Input/Output

#### wprintf

```c
int wprintf(const wchar_t *format, ...)
```

The wide-character equivalent of `printf`.

---

### wctype.h - Classification

The `wctype.h` functionality for wide character classification and mapping is also supported via:

- `iswalpha`, `iswdigit`: Wide character classification.
- `towlower`, `towupper`: Wide character case mapping.