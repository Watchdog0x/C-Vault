# inttypes.h - Integer Formatting

**Category:** 📂 std/ (C Standard Library)  
**Header:** `<inttypes.h>`  
**Scope:** C99, C11

The `inttypes.h` header includes `stdint.h` and defines macros for formatting fixed-width integer types.

## Facilities Overview

| Facility Category | Key Symbols | Description |
|-------------------|-------------|-------------|
| [Format Specifiers](#format-specifiers) | PRIu32, PRId64, PRIxPTR | Conversion specifiers for `printf`. |
| [Scan Format Specifiers](#scan-format-specifiers) | SCNu32, SCNd64 | Conversion specifiers for `scanf`. |
| [Extended Conversion](#numeric-conversion) | strtoimax, strtoumax | Converting strings to widest available integers. |

---

## Format Specifiers

Format specifier macros for fixed-width integer types defined in `<inttypes.h>`:

| Macro | Equivalent | Description |
|-------|------------|-------------|
| `PRIu8` | `"u"` | `uint8_t` as unsigned decimal |
| `PRIu16` | `"u"` | `uint16_t` as unsigned decimal |
| `PRIu32` | `"u"` | `uint32_t` as unsigned decimal |
| `PRIu64` | `"llu"` | `uint64_t` as unsigned decimal |
| `PRId8` | `"d"` | `int8_t` as signed decimal |
| `PRId16` | `"d"` | `int16_t` as signed decimal |
| `PRId32` | `"d"` | `int32_t` as signed decimal |
| `PRId64` | `"lld"` | `int64_t` as signed decimal |
| `PRIx8` | `"x"` | `uint8_t` as hexadecimal |
| `PRIx16` | `"x"` | `uint16_t` as hexadecimal |
| `PRIx32` | `"x"` | `uint32_t` as hexadecimal |
| `PRIx64` | `"llx"` | `uint64_t` as hexadecimal |
| `PRIX8` | `"X"` | `uint8_t` as uppercase hexadecimal |
| `PRIX16` | `"X"` | `uint16_t` as uppercase hexadecimal |
| `PRIX32` | `"X"` | `uint32_t` as uppercase hexadecimal |
| `PRIX64` | `"llX"` | `uint64_t` as uppercase hexadecimal |
| `PRIo8` | `"o"` | `uint8_t` as octal |
| `PRIo16` | `"o"` | `uint16_t` as octal |
| `PRIo32` | `"o"` | `uint32_t` as octal |
| `PRIo64` | `"llo"` | `uint64_t` as octal |
| `PRIuPTR` | `"lu"` | `uintptr_t` as unsigned decimal |
| `PRIdPTR` | `"ld"` | `intptr_t` as signed decimal |
| `PRIxPTR` | `"lx"` | `uintptr_t` as hexadecimal |
| `PRIXPTR` | `"lX"` | `uintptr_t` as uppercase hexadecimal |
| `PRIuMAX` | `"llu"` | `uintmax_t` as unsigned decimal |
| `PRIdMAX` | `"lld"` | `intmax_t` as signed decimal |
| `PRIxMAX` | `"llx"` | `uintmax_t` as hexadecimal |
| `PRIXMAX` | `"llX"` | `uintmax_t` as uppercase hexadecimal |

### Scan Format Specifiers

| Macro | Equivalent | Description |
|-------|------------|-------------|
| `SCNu8` | `"u"` | Scan `uint8_t` as unsigned decimal |
| `SCNu16` | `"u"` | Scan `uint16_t` as unsigned decimal |
| `SCNu32` | `"u"` | Scan `uint32_t` as unsigned decimal |
| `SCNu64` | `"llu"` | Scan `uint64_t` as unsigned decimal |
| `SCNd8` | `"d"` | Scan `int8_t` as signed decimal |
| `SCNd16` | `"d"` | Scan `int16_t` as signed decimal |
| `SCNd32` | `"d"` | Scan `int32_t` as signed decimal |
| `SCNd64` | `"lld"` | Scan `int64_t` as signed decimal |
| `SCNx8` | `"x"` | Scan `uint8_t` as hexadecimal |
| `SCNx16` | `"x"` | Scan `uint16_t` as hexadecimal |
| `SCNx32` | `"x"` | Scan `uint32_t` as hexadecimal |
| `SCNx64` | `"llx"` | Scan `uint64_t` as hexadecimal |
| `SCNo8` | `"o"` | Scan `uint8_t` as octal |
| `SCNo16` | `"o"` | Scan `uint16_t` as octal |
| `SCNo32` | `"o"` | Scan `uint32_t` as octal |
| `SCNo64` | `"llo"` | Scan `uint64_t` as octal |
| `SCNuPTR` | `"lu"` | Scan `uintptr_t` as unsigned decimal |
| `SCNdPTR` | `"ld"` | Scan `intptr_t` as signed decimal |
| `SCNxPTR` | `"lx"` | Scan `uintptr_t` as hexadecimal |
| `SCNuMAX` | `"llu"` | Scan `uintmax_t` as unsigned decimal |
| `SCNdMAX` | `"lld"` | Scan `intmax_t` as signed decimal |
| `SCNxMAX` | `"llx"` | Scan `uintmax_t` as hexadecimal |

<details><summary>Example</summary>

```c
#include <inttypes.h>
#include <stdio.h>

int main(void) {
    uint64_t large = 123456789012345ULL;
    printf("Large value: %" PRIu64 "\n", large);
    
    uint32_t value;
    scanf("%" SCNu32, &value);
    printf("Scanned value: %" PRIu32 "\n", value);
    return 0;
}
```

</details>

---

## Types & Variables

### intmax_t

```c
typedef long long intmax_t;
```

Maximum-width signed integer type.

### uintmax_t

```c
typedef unsigned long long uintmax_t;
```

Maximum-width unsigned integer type.

---

## Functions

### Numeric Conversion

### strtoimax

```c
intmax_t strtoimax(const char *nptr, char **endptr, int base)
```

Equivalent to `strtol`, but returns `intmax_t`.

### strtoumax

```c
uintmax_t strtoumax(const char *nptr, char **endptr, int base)
```

Equivalent to `strtoul`, but returns `uintmax_t`.
