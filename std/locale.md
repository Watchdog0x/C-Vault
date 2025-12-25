# locale.h - Localization

**Category:** 📂 std/ (C Standard Library)  
**Header:** `<locale.h>`  
**Scope:** C89, C99, C11

The `locale.h` header defines structures and functions for controlling various locale-specific settings.

## Facilities Overview

| Facility Category | Key Symbols | Description |
|-------------------|-------------|-------------|
| Locale Control | setlocale | Setting or querying the current locale. |
| Formatting Info | localeconv | Retrieving formatting details for the current locale. |
| Constants | LC_ALL, LC_NUMERIC, LC_TIME | Category selectors for `setlocale`. |

---

## Types & Variables

### struct lconv

Significant fields include:
- `decimal_point`: Character used for decimal point.
- `thousands_sep`: Character used for thousands separator.
- `currency_symbol`: Local currency symbol.
- `int_curr_symbol`: International currency symbol.

<details><summary>Example</summary>

```c
#include <locale.h>
#include <stdio.h>

int main(void) {
    struct lconv *lc = localeconv();
    printf("Decimal point: '%s'\n", lc->decimal_point);
    return 0;
}
```

</details>

---

## Functions

### Locale Control

### setlocale

```c
char *setlocale(int category, const char *locale)
```

Sets or queries the program's current locale. 
- `category`: `LC_ALL`, `LC_COLLATE`, `LC_CTYPE`, `LC_MONETARY`, `LC_NUMERIC`, `LC_TIME`.
- `locale`: e.g., `"C"` (default), `""` (system default), `"en_US.UTF-8"`.

**Returns:** A string pointer to the locale name associated with the category, or `NULL` if the request cannot be honored.

<details><summary>Example</summary>

```c
#include <locale.h>
#include <stdio.h>

int main(void) {
    // Attempt to set to system default
    char *res = setlocale(LC_ALL, "");
    
    if (res == NULL) {
        fprintf(stderr, "Failed to set system locale. Fallback to C.\n");
        // setlocale failure usually leaves locale unchanged (C)
    } else {
        printf("Locale set to: %s\n", res);
    }
    return 0;
}
```

</details>

### Formatting Info

### localeconv

```c
struct lconv *localeconv(void)
```

Returns a pointer to a `struct lconv` that contains details about numeric and monetary formatting.
