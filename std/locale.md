# locale.h - Localization

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

**Example:**

```c
#include <locale.h>
#include <stdio.h>

int main(void) {
    struct lconv *lc = localeconv();
    printf("Decimal point: '%s'\n", lc->decimal_point);
    return 0;
}
```

---

## Functions

### Locale Control

#### setlocale

```c
char *setlocale(int category, const char *locale)
```

Sets or queries the program's current locale. 
- `category`: `LC_ALL`, `LC_COLLATE`, `LC_CTYPE`, `LC_MONETARY`, `LC_NUMERIC`, `LC_TIME`.
- `locale`: e.g., `"C"` (default), `""` (system default), `"en_US.UTF-8"`.

**Example:**

```c
#include <locale.h>
#include <stdio.h>

int main(void) {
    setlocale(LC_ALL, ""); // Use system default locale
    printf("Current locale: %s\n", setlocale(LC_ALL, NULL));
    return 0;
}
```

### Formatting Info

#### localeconv

```c
struct lconv *localeconv(void)
```

Returns a pointer to a `struct lconv` that contains details about numeric and monetary formatting.