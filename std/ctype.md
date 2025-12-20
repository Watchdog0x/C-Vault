# ctype.h - Character Classification

The `ctype.h` header declares several functions useful for testing and mapping characters.

## Facilities Overview

| Facility Category | Key Symbols | Description |
|-------------------|-------------|-------------|
| Classification | isalpha, isdigit, isalnum, isspace | Testing character properties (alphabetic, numeric, whitespace). |
| Transformation | tolower, toupper | Mapping characters between cases. |
| Other Checks | ispunct, iscntrl, isxdigit, isprint | Testing for punctuation, control characters, and hex digits. |

---

## Functions

### Classification Functions

#### isalpha

```c
int isalpha(int c)
```

Checks if `c` is an alphabetic character.

#### isdigit

```c
int isdigit(int c)
```

Checks if `c` is a decimal digit ('0' through '9').

#### isalnum

```c
int isalnum(int c)
```

Checks if `c` is an alphanumeric character (either a letter or a digit).

#### isspace

```c
int isspace(int c)
```

Checks if `c` is a whitespace character (space, form feed, line feed, carriage return, horizontal tab, vertical tab).

**Example:**

```c
#include <ctype.h>
#include <stdio.h>

int main(void) {
    char s[] = "123 Go!";
    for (int i = 0; s[i]; i++) {
        if (isdigit(s[i])) printf("'%c' is a digit\n", s[i]);
        if (isalpha(s[i])) printf("'%c' is a letter\n", s[i]);
    }
    return 0;
}
```

### Transformation Functions

#### tolower

```c
int tolower(int c)
```

Converts `c` to its lowercase equivalent if it's an uppercase letter; otherwise, returns `c`.

#### toupper

```c
int toupper(int c)
```

Converts `c` to its uppercase equivalent if it's a lowercase letter; otherwise, returns `c`.

**Example:**

```c
#include <ctype.h>
#include <stdio.h>

int main(void) {
    char c = 'a';
    printf("Upper case of '%c' is '%c'\n", c, toupper(c));
    return 0;
}
```

---

### Technical Details

All functions in `ctype.h` return a non-zero value (true) if the character meets the condition, and zero (false) otherwise. The behavior of these functions is affected by the current **locale**.