# stdio.h - Standard Input/Output

**Category:** 📂 std/ (C Standard Library)  
**Header:** `<stdio.h>`  
**Scope:** C89, C99, C11

The `stdio.h` header defines types and macros for handling input and output, and declares functions for performing file and stream operations.

## Facilities Overview

| Facility Category | Key Symbols | Description |
|-------------------|-------------|-------------|
| [File Access](#file-access) | [fopen](#fopen), [fclose](#fclose), [fflush](#fflush) | Lifecycle management of streams and buffers. |
| [Formatted Output](#formatted-output) | [sprintf](#sprintf), [printf](#printf), [fprintf](#fprintf), [snprintf](#snprintf) | Converting internal binary types to text. |
| [Formatted Input](#formatted-input) | [scanf](#scanf), [sscanf](#sscanf) | Parsing text streams. |
| [Character I/O](#character-io) | [fgetc](#fgetc), [fgets](#fgets), [fputc](#fputc), [fputs](#fputs) | Reading and writing characters or strings. |
| [Direct/Block I/O](#direct--block-io) | [fread](#fread), [fwrite](#fwrite) | Transferring raw binary data blocks. |
| [File Positioning](#positioning) | [fseek](#fseek), [ftell](#ftell), [rewind](#rewind) | Manipulating the file position cursor. |
| [Error Handling](#error-handling) | [perror](#perror), [feof](#feof), [ferror](#ferror) | Diagnosing stream states. |
| [File System](#file-system) | [remove](#remove), [rename](#rename) | Operations on the file system namespace. |

---

## Format Specifiers

Format specifiers are used in functions like `printf` and `scanf` to tell the compiler what type of data is being processed.



| Specifier | Data Type | Description |
|-----------|-----------|-------------|
| `%d` or `%i` | `int` | Signed decimal integer. |
| `%u` | `unsigned int` | Unsigned decimal integer. |
| `%f` | `float` / `double` | Decimal floating point. |
| `%c` | `char` | Single character. |
| `%s` | `char *` | String of characters. |
| `%p` | `void *` | Pointer address (hexadecimal). |
| `%x` / `%X` | `int` | Unsigned hexadecimal (lowercase/uppercase). |
| `%ld` | `long int` | Signed long decimal integer. |
| `%%` | None | Prints a literal percent sign. |

---

## Types & Variables

### FILE

```c
typedef struct _IO_FILE FILE;
```

Opaque type representing a stream. Used with all file I/O functions to track the current state of a file. You cannot access members of this struct directly; you must use the provided functions (e.g., `feof`, `ferror`).

### size_t

```c
typedef unsigned long size_t;
```

Unsigned integer type used for representing sizes of objects and return values for block I/O.

### Standard Streams

* **`stdin`** (`FILE *`): Standard input stream (usually the keyboard).
* **`stdout`** (`FILE *`): Standard output stream (usually the terminal console).
* **`stderr`** (`FILE *`): Standard error stream used for diagnostic messages (unbuffered).

---

## Functions

### Formatted Output

### sprintf

```c
int sprintf(char *str, const char *format, ...)
```

Writes formatted output to the string `str`.

**Returns:** The number of characters written (excluding the null byte).

> [!WARNING]
> **UNSAFE**: `sprintf` does not check for buffer overflows. Use [snprintf](#snprintf) instead.

<details><summary>Example (Unsafe)</summary>

```c
#include <stdio.h>

int main(void) {
    char buf[10];
    // UNSAFE: Overflow if formatted string > 9 chars
    sprintf(buf, "Value: %d", 123456789);
    return 0;
}
```

</details>

### printf
```c
int printf(const char *format, ...)
```

Writes formatted output to the standard output stream (`stdout`).

**Returns:** The number of characters printed (excluding the null byte), or a negative value if an output error occurs.

<details><summary>Example</summary>

```c
#include <stdio.h>

int main(void) {
    const char *user = "Admin";
    int id = 101;
    
    // Check return value to ensure output was successful
    if (printf("User: %s (ID: %d)\n", user, id) < 0) {
        perror("Failed to write to stdout");
        return 1;
    }
    return 0;
}
```

</details>

### fprintf

```c
int fprintf(FILE *stream, const char *format, ...)
```

Writes formatted output to the specified `stream`. Common for writing to files or `stderr`.

**Returns:** The number of characters written, or a negative value if an error occurs.

<details><summary>Example</summary>

```c
#include <stdio.h>

int main(void) {
    FILE *fp = fopen("log.txt", "a");
    if (fp == NULL) {
        perror("Failed to open log file");
        return 1;
    }

    // Write to file and check for success
    if (fprintf(fp, "Log entry: %s\n", "System started") < 0) {
        perror("Failed to write to file");
        fclose(fp);
        return 1;
    }

    fclose(fp);
    return 0;
}
```

</details>

### snprintf

```c
int snprintf(char *str, size_t size, const char *format, ...)
```

**Returns:** The number of characters that *would* have been written (excluding null terminator) if the buffer were large enough. If return value >= `size`, the output was truncated.

<details><summary>Example</summary>

```c
#include <stdio.h>

int main(void) {
    char buf[16]; // Small buffer
    int val = 123456789;
    
    // Safe formatting: ensures no buffer overflow
    int needed = snprintf(buf, sizeof(buf), "Value: %d", val);
    
    if (needed >= sizeof(buf)) {
        // Handle truncation
        fprintf(stderr, "Warning: String truncated! Needed %d bytes.\n", needed);
    } else if (needed < 0) {
        perror("Encoding error");
    } else {
        printf("Buffer content: %s\n", buf);
    }
    return 0;
}
```

</details>

---

### Formatted Input

### scanf

```c
int scanf(const char *format, ...)
```

Reads formatted input from `stdin`.

**Returns:** The number of input items successfully matched and assigned. Returns `EOF` if read failure occurs before any conversion.

> [!WARNING]
> **UNSAFE**: Using `%s` without a width specifier (e.g. `%s` instead of `%99s`) allows buffer overflows.

<details><summary>Example</summary>

```c
#include <stdio.h>

int main(void) {
    int id;
    float value;

    printf("Enter ID and Value: ");
    // Validate that specifically 2 items were parsed
    if (scanf("%d %f", &id, &value) != 2) {
        fprintf(stderr, "Invalid input format.\n");
        return 1;
    }

    printf("Accepted: ID=%d, Val=%.2f\n", id, value);
    return 0;
}
```

</details>

### sscanf

```c
int sscanf(const char *str, const char *format, ...)
```

Reads formatted data from a string instead of a stream.

<details><summary>Example</summary>

```c
#include <stdio.h>

int main(void) {
    const char *str = "Age: 25";
    int age;
    sscanf(str, "Age: %d", &age);
    printf("Parsed age: %d\n", age);
    return 0;
}
```

</details>

---

### File Access

### fopen

```c
FILE *fopen(const char *filename, const char *mode)

```

Opens the file and associates a stream with it.

* `"r"`: Read (file must exist).
* `"w"`: Write (creates file, or truncates existing).
* `"a"`: Append (creates file, or writes to end).
* `"r+"`: Read and Write (file must exist).

**Returns:** A pointer to the `FILE` object, or `NULL` if the file could not be opened (checks `errno`).

<details><summary>Example</summary>

```c
#include <stdio.h>
#include <errno.h>

int main(void) {
    // "r" mode: Fails if file doesn't exist
    FILE *fp = fopen("config.txt", "r");
    
    if (fp == NULL) {
        // ALWAYS check for NULL
        perror("Failed to open config.txt");
        return 1;
    }
    
    // File operations here...
    
    fclose(fp);
    return 0;
}
```

</details>

### fclose

```c
int fclose(FILE *stream)

```

Flushes the stream and closes the file. Returns `0` on success.

### fflush

```c
int fflush(FILE *stream)

```

Forces a write of all buffered data for the given output `stream`.

---

### Character I/O

### fgetc

```c
int fgetc(FILE *stream)
```

Reads a single character from a stream.

### fputc

```c
int fputc(int c, FILE *stream)
```

Writes a single character to a stream.

### fgets

```c
char *fgets(char *s, int n, FILE *stream)
```

Reads a string from a stream into `s`. Reads at most `n-1` characters. Stops at newline (which is retained) or EOF. Always null-terminates.

**Returns:** `s` on success, or `NULL` on error or if end-of-file occurs while no characters have been read.

> [!WARNING]
> **UNSAFE**: `gets` is removed from C11 because it causes buffer overflows. Use `fgets` instead.

<details><summary>Example</summary>

```c
#include <stdio.h>

int main(void) {
    char buf[256];
    printf("Enter text: ");
    
    // fgets is safer than gets() because it enforces buffer limit
    if (fgets(buf, sizeof(buf), stdin) != NULL) {
        printf("You typed: %s", buf);
    } else {
        if (feof(stdin)) {
            printf("\nEnd of input reached.\n");
        } else {
            perror("Error reading input");
        }
    }
    return 0;
}
```

</details>

### fputs

```c
int fputs(const char *s, FILE *stream)
```

Writes a string to a stream (does not write the null terminator).

**Returns:** Non-negative value on success, `EOF` on error.

---

### Direct / Block I/O

### fread

```c
size_t fread(void *ptr, size_t size, size_t nmemb, FILE *stream)
```

Reads binary data from a stream.

**Returns:** The number of elements successfully read (not bytes). If this is less than `nmemb`, either EOF was reached or a read error occurred.

<details><summary>Example</summary>

```c
#include <stdio.h>

int main(void) {
    struct Point { int x, y; } pts[5];
    
    FILE *fp = fopen("data.bin", "rb");
    if (!fp) return 1;

    // Check strict element count
    size_t got = fread(pts, sizeof(struct Point), 5, fp);
    if (got != 5) {
        if (feof(fp)) printf("Unexpected EOF after %lu records.\n", got);
        if (ferror(fp)) perror("Read error");
    }

    fclose(fp);
    return 0;
}
```

</details>

### fwrite

```c
size_t fwrite(const void *ptr, size_t size, size_t nmemb, FILE *stream)
```

Writes binary data to a stream.

<details><summary>Example</summary>

```c
#include <stdio.h>

int main(void) {
    // Writing
    int arr[5] = {1, 2, 3, 4, 5};
    FILE *fp = fopen("data.bin", "wb");
    fwrite(arr, sizeof(int), 5, fp);
    fclose(fp);
    return 0;
}
```

</details>

---

### Positioning

### fseek

```c
int fseek(FILE *stream, long int offset, int whence)
```

Moves the file pointer. `whence` options: `SEEK_SET` (start), `SEEK_CUR` (current), `SEEK_END` (end).

### ftell

```c
long int ftell(FILE *stream)
```

Returns the current byte position of the file pointer.

### rewind

```c
void rewind(FILE *stream)
```

Moves the file pointer back to the very beginning of the file.

---

### Error Handling

### perror

```c
void perror(const char *s)
```

Prints a system error message to `stderr` describing the last error encountered.

### feof

```c
int feof(FILE *stream)
```

Returns non-zero if the End-of-File has been reached.

### ferror

```c
int ferror(FILE *stream)
```

Returns non-zero if an error occurred on the stream.

---

### File System

### remove

```c
int remove(const char *filename)
```

Deletes the file specified.

### rename

```c
int rename(const char *old_filename, const char *new_filename)
```

Renames a file or directory.
