# stdio.h - Standard Input/Output

**Category:** 📂 std/ (C Standard Library)  
**Header:** `<stdio.h>`  
**Scope:** C89, C99, C11

The `stdio.h` header defines types and macros for handling input and output, and declares functions for performing file and stream operations.

## Facilities Overview

| Facility Category | Key Symbols | Description |
|-------------------|-------------|-------------|
| [File Access](#file-access) | [fopen](#fopen), [fclose](#fclose), [fflush](#fflush) | Lifecycle management of streams and buffers. |
| [Formatted Output](#formatted-output) | [printf](#printf), [fprintf](#fprintf), [snprintf](#snprintf) | Converting internal binary types to text. |
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
Opaque type representing a stream. Used with all file I/O functions to track the current state of a file.

### size_t
Unsigned integer type used for representing sizes of objects and return values for block I/O.

### Standard Streams
* **`stdin`**: Standard input stream (usually the keyboard).
* **`stdout`**: Standard output stream (usually the terminal console).
* **`stderr`**: Standard error stream used for diagnostic messages.

---

## Functions

### Formatted Output

#### printf
```c
int printf(const char *format, ...)
```

Writes formatted output to the standard output stream (`stdout`).

**Example:**

```c
#include <stdio.h>

int main(void) {
    const char *name = "Kim";
    int age = 22;
    printf("%s is %d years old.\n", name, age);
    return 0;
}
```

#### fprintf

```c
int fprintf(FILE *stream, const char *format, ...)
```

Writes formatted output to the specified `stream`. Common for writing to files or `stderr`.

**Example:**

```c
#include <stdio.h>

int main(void) {
    FILE *fp = fopen("output.txt", "w");
    if (fp) {
        fprintf(fp, "Name: %s, Age: %d\n", "Alice", 30);
        fclose(fp);
    }
    return 0;
}
```

#### snprintf

```c
int snprintf(char *str, size_t size, const char *format, ...)
```

Writes formatted output to a character buffer `str` with a maximum `size`. This is the safe alternative to `sprintf`.

**Example:**

```c
char buffer[50];
int score = 95;
snprintf(buffer, sizeof(buffer), "Score: %d", score);

```

---

### Formatted Input

#### scanf

```c
int scanf(const char *format, ...)
```

Reads formatted input from `stdin`.

**Example:**

```c
int val;
printf("Enter a number: ");
scanf("%d", &val); // Note the use of & (address-of) operator
```

#### sscanf

```c
int sscanf(const char *str, const char *format, ...)
```

Reads formatted data from a string instead of a stream.

**Example:**

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

---

### File Access

#### fopen

```c
FILE *fopen(const char *filename, const char *mode)

```

Opens the file and associates a stream with it.

* `"r"`: Read
* `"w"`: Write (overwrites file)
* `"a"`: Append (adds to end of file)

**Example:**

```c
FILE *fp = fopen("test.txt", "w");
if (fp != NULL) {
    fputs("Hello C", fp);
    fclose(fp);
}

```

#### fclose

```c
int fclose(FILE *stream)

```

Flushes the stream and closes the file. Returns `0` on success.

#### fflush

```c
int fflush(FILE *stream)

```

Forces a write of all buffered data for the given output `stream`.

---

### Character I/O

#### fgetc

```c
int fgetc(FILE *stream)
```

Reads a single character from a stream.

#### fputc

```c
int fputc(int c, FILE *stream)
```

Writes a single character to a stream.

#### fgets

```c
char *fgets(char *s, int n, FILE *stream)
```

Reads a line/string from a stream safely (limits length).

#### fputs

```c
int fputs(const char *s, FILE *stream)
```

Writes a string to a stream.

**Example:**

```c
char buffer[100];
printf("Enter a line: ");
fgets(buffer, 100, stdin);
fputs(buffer, stdout);
```

---

### Direct / Block I/O

#### fread

```c
size_t fread(void *ptr, size_t size, size_t nmemb, FILE *stream)
```

Reads binary data from a stream.

**Example:**

```c
#include <stdio.h>

int main(void) {
    // Reading
    int read_arr[5];
    FILE *fp = fopen("data.bin", "rb");
    fread(read_arr, sizeof(int), 5, fp);
    fclose(fp);
    return 0;
}
```

#### fwrite

```c
size_t fwrite(const void *ptr, size_t size, size_t nmemb, FILE *stream)
```

Writes binary data to a stream.

**Example:**

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

---

### Positioning

#### fseek

```c
int fseek(FILE *stream, long int offset, int whence)
```

Moves the file pointer. `whence` options: `SEEK_SET` (start), `SEEK_CUR` (current), `SEEK_END` (end).

#### ftell

```c
long int ftell(FILE *stream)
```

Returns the current byte position of the file pointer.

#### rewind

```c
void rewind(FILE *stream)
```

Moves the file pointer back to the very beginning of the file.

---

### Error Handling

#### perror

```c
void perror(const char *s)
```

Prints a system error message to `stderr` describing the last error encountered.

#### feof

```c
int feof(FILE *stream)
```

Returns non-zero if the End-of-File has been reached.

#### ferror

```c
int ferror(FILE *stream)
```

Returns non-zero if an error occurred on the stream.

---

### File System

#### remove

```c
int remove(const char *filename)
```

Deletes the file specified.

#### rename

```c
int rename(const char *old_filename, const char *new_filename)
```

Renames a file or directory.
