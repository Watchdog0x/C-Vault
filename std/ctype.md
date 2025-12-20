# ctype.h - Character Classification

**Category:** 📂 std/ (C Standard Library)  
**Header:** `<ctype.h>`  
**Scope:** C89, C99, C11

The `ctype.h` header declares several functions useful for testing and mapping characters. These functions operate on individual characters and are essential for text processing and parsing.

## Facilities Overview

| Facility Category | Key Symbols | Description |
|-------------------|-------------|-------------|
| [Classification](#classification-functions) | [isalpha](#isalpha), [isdigit](#isdigit), [isalnum](#isalnum), [isspace](#isspace), [isupper](#isupper), [islower](#islower) | Testing character properties (alphabetic, numeric, whitespace). |
| [Transformation](#transformation-functions) | [tolower](#tolower), [toupper](#toupper) | Mapping characters between cases. |
| [Other Checks](#other-check-functions) | [ispunct](#ispunct), [iscntrl](#iscntrl), [isxdigit](#isxdigit), [isprint](#isprint), [isgraph](#isgraph) | Testing for punctuation, control characters, and hex digits. |

---

## Functions

### Classification Functions

### isalpha

```c
int isalpha(int c)
```

Checks if `c` is an alphabetic character (a-z, A-Z).

<details><summary>Example</summary>

```c
#include <ctype.h>
#include <stdio.h>

int main(void) {
    char c1 = 'A', c2 = '5';
    
    if (isalpha(c1)) {
        printf("'%c' is alphabetic\n", c1); // This will execute
    }
    if (!isalpha(c2)) {
        printf("'%c' is not alphabetic\n", c2); // This will execute
    }
    return 0;
}
```

</details>

### isdigit

```c
int isdigit(int c)
```

Checks if `c` is a decimal digit ('0' through '9').

<details><summary>Example</summary>

```c
#include <ctype.h>
#include <stdio.h>

int main(void) {
    char c1 = '5', c2 = 'A';
    
    if (isdigit(c1)) {
        printf("'%c' is a digit\n", c1); // This will execute
    }
    if (!isdigit(c2)) {
        printf("'%c' is not a digit\n", c2); // This will execute
    }
    return 0;
}
```

</details>

### isalnum

```c
int isalnum(int c)
```

Checks if `c` is an alphanumeric character (either a letter or a digit).

<details><summary>Example</summary>

```c
#include <ctype.h>
#include <stdio.h>

int main(void) {
    char str[] = "Hello123!";
    
    for (int i = 0; str[i]; i++) {
        if (isalnum(str[i])) {
            printf("'%c' is alphanumeric\n", str[i]);
        } else {
            printf("'%c' is NOT alphanumeric\n", str[i]);
        }
    }
    return 0;
}
```

</details>

### isspace

```c
int isspace(int c)
```

Checks if `c` is a whitespace character (space, form feed `\f`, line feed `\n`, carriage return `\r`, horizontal tab `\t`, vertical tab `\v`).

<details><summary>Example</summary>

```c
#include <ctype.h>
#include <stdio.h>

int main(void) {
    char str[] = "Hello World\n";
    int space_count = 0;
    
    for (int i = 0; str[i]; i++) {
        if (isspace(str[i])) {
            space_count++;
        }
    }
    printf("Found %d whitespace characters\n", space_count);
    // Output: Found 2 whitespace characters
    return 0;
}
```

</details>

### isupper

```c
int isupper(int c)
```

Checks if `c` is an uppercase letter (A-Z).

<details><summary>Example</summary>

```c
#include <ctype.h>
#include <stdio.h>

int main(void) {
    char c1 = 'A', c2 = 'a';
    
    printf("'%c' is uppercase: %d\n", c1, isupper(c1)); // Output: 1 (true)
    printf("'%c' is uppercase: %d\n", c2, isupper(c2)); // Output: 0 (false)
    return 0;
}
```

</details>

### islower

```c
int islower(int c)
```

Checks if `c` is a lowercase letter (a-z).

<details><summary>Example</summary>

```c
#include <ctype.h>
#include <stdio.h>

int main(void) {
    char c1 = 'a', c2 = 'A';
    
    printf("'%c' is lowercase: %d\n", c1, islower(c1)); // Output: 1 (true)
    printf("'%c' is lowercase: %d\n", c2, islower(c2)); // Output: 0 (false)
    return 0;
}
```

</details>

---

### Transformation Functions

### tolower

```c
int tolower(int c)
```

Converts `c` to its lowercase equivalent if it's an uppercase letter; otherwise, returns `c` unchanged.

<details><summary>Example</summary>

```c
#include <ctype.h>
#include <stdio.h>

int main(void) {
    char str[] = "HELLO World";
    
    for (int i = 0; str[i]; i++) {
        str[i] = tolower(str[i]);
    }
    printf("%s\n", str); // Output: hello world
    return 0;
}
```

</details>

### toupper

```c
int toupper(int c)
```

Converts `c` to its uppercase equivalent if it's a lowercase letter; otherwise, returns `c` unchanged.

<details><summary>Example</summary>

```c
#include <ctype.h>
#include <stdio.h>

int main(void) {
    char str[] = "hello WORLD";
    
    for (int i = 0; str[i]; i++) {
        str[i] = toupper(str[i]);
    }
    printf("%s\n", str); // Output: HELLO WORLD
    return 0;
}
```

</details>

---

### Other Check Functions

### ispunct

```c
int ispunct(int c)
```

Checks if `c` is a punctuation character (any printable character that is not a space or alphanumeric).

<details><summary>Example</summary>

```c
#include <ctype.h>
#include <stdio.h>

int main(void) {
    char str[] = "Hello, World!";
    
    for (int i = 0; str[i]; i++) {
        if (ispunct(str[i])) {
            printf("'%c' is punctuation\n", str[i]);
        }
    }
    // Output:
    // ',' is punctuation
    // '!' is punctuation
    return 0;
}
```

</details>

### iscntrl

```c
int iscntrl(int c)
```

Checks if `c` is a control character (ASCII 0-31 and 127).

<details><summary>Example</summary>

```c
#include <ctype.h>
#include <stdio.h>

int main(void) {
    char c1 = '\n', c2 = 'A';
    
    if (iscntrl(c1)) {
        printf("Newline is a control character\n"); // This will execute
    }
    if (!iscntrl(c2)) {
        printf("'A' is not a control character\n"); // This will execute
    }
    return 0;
}
```

</details>

### isxdigit

```c
int isxdigit(int c)
```

Checks if `c` is a hexadecimal digit (0-9, a-f, A-F).

<details><summary>Example</summary>

```c
#include <ctype.h>
#include <stdio.h>

int main(void) {
    char str[] = "1A9FzQ";
    
    for (int i = 0; str[i]; i++) {
        if (isxdigit(str[i])) {
            printf("'%c' is a hex digit\n", str[i]);
        } else {
            printf("'%c' is NOT a hex digit\n", str[i]);
        }
    }
    return 0;
}
```

</details>

### isprint

```c
int isprint(int c)
```

Checks if `c` is a printable character (including space).

<details><summary>Example</summary>

```c
#include <ctype.h>
#include <stdio.h>

int main(void) {
    char c1 = 'A', c2 = '\n';
    
    printf("'A' is printable: %d\n", isprint(c1)); // Output: 1 (true)
    printf("'\\n' is printable: %d\n", isprint(c2)); // Output: 0 (false)
    return 0;
}
```

</details>

### isgraph

```c
int isgraph(int c)
```

Checks if `c` is a printable character excluding space.

<details><summary>Example</summary>

```c
#include <ctype.h>
#include <stdio.h>

int main(void) {
    char str[] = "A 1!";
    
    for (int i = 0; str[i]; i++) {
        if (isgraph(str[i])) {
            printf("'%c' is a graphic character\n", str[i]);
        } else {
            printf("'%c' is NOT a graphic character\n", str[i]);
        }
    }
    return 0;
}
```

</details>

---

## Technical Details

All functions in `ctype.h` return a non-zero value (true) if the character meets the condition, and zero (false) otherwise. The behavior of these functions is affected by the current **locale** setting.

**Important Notes:**
- The argument to these functions should be representable as `unsigned char` or be equal to `EOF`.
- These functions are often implemented as macros for performance.
- Transformation functions (`tolower`, `toupper`) return the modified character as an `int`.
