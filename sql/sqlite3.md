# sqlite3.h - SQLite Embedded Database

**Category:** 📂 sql/ (Third-Party Ecosystem)
**Header:** `<sqlite3.h>`
**Scope:** Embedded SQL Database

The `sqlite3.h` header is the interface for SQLite, a C-language library that implements a small, fast, self-contained, high-reliability, full-featured, SQL database engine.

## Facilities Overview

| Facility Category | Key Symbols | Description |
|-------------------|-------------|-------------|
| [Database Mgmt](#connection-management) | [sqlite3_open](#sqlite3_open), [sqlite3_close](#sqlite3_close) | Opening and closing database files. |
| [Execution](#direct-execution) | [sqlite3_exec](#sqlite3_exec) | Executing one or more SQL statements directly. |
| [Prepared Stmts](#prepared-statements) | [sqlite3_prepare_v2](#sqlite3_prepare_v2), [sqlite3_step](#sqlite3_step) | Compiling SQL and iterating over results. |
| [Column Access](#column-access) | [sqlite3_column_text](#sqlite3_column_text), [sqlite3_column_int](#sqlite3_column_int) | Retrieving data from a query row. |
| [Error Handling](#error-handling) | [sqlite3_errmsg](#sqlite3_errmsg) | Retrieving error description strings. |

---

## Types & Variables

### sqlite3

```c
typedef struct sqlite3 sqlite3;
```

An opaque handle representing a database connection.

### sqlite3_stmt

```c
typedef struct sqlite3_stmt sqlite3_stmt;
```

An opaque handle representing a single compiled SQL statement.

---

## Functions

### Connection Management

### sqlite3_open

```c
int sqlite3_open(const char *filename, sqlite3 **ppDb)
```

Opens a new connection to a SQLite database.

**Returns:** `SQLITE_OK` on success. On failure, returns an error code (and `*ppDb` may still be allocated, requiring close).

<details><summary>Example</summary>

```c
#include <sqlite3.h>
#include <stdio.h>

int main(void) {
    sqlite3 *db;
    int rc = sqlite3_open(":memory:", &db);
    
    if (rc != SQLITE_OK) {
        fprintf(stderr, "Cannot open database: %s\n", sqlite3_errmsg(db));
        sqlite3_close(db);
        return 1;
    }
    
    printf("Database opened successfully.\n");
    sqlite3_close(db);
    return 0;
}
```

</details>

### sqlite3_close

```c
int sqlite3_close(sqlite3 *db)
```

Closes a database connection.

**Returns:** `SQLITE_OK` on success.

---

### Direct Execution

### sqlite3_exec

```c
int sqlite3_exec(sqlite3 *db, const char *sql, int (*callback)(void*,int,char**,char**), void *arg, char **errmsg)
```

Compiles and executes one or more SQL statements (often used for schema creation).

**Returns:** `SQLITE_OK` on success.

---

### Prepared Statements

### sqlite3_prepare_v2

```c
int sqlite3_prepare_v2(sqlite3 *db, const char *zSql, int nByte, sqlite3_stmt **ppStmt, const char **pzTail)
```

Compiles a SQL statement into a byte-code program.

**Returns:** `SQLITE_OK` on success.

### sqlite3_step

```c
int sqlite3_step(sqlite3_stmt *pStmt)
```

Evaluates a prepared statement.

**Returns:**
*   `SQLITE_ROW`: A row of data is ready.
*   `SQLITE_DONE`: Execution completed.
*   Other error codes.

### sqlite3_finalize

```c
int sqlite3_finalize(sqlite3_stmt *pStmt)
```

Destroys a prepared statement object.

**Returns:** `SQLITE_OK` on success.

---

### Column Access

### sqlite3_column_text

```c
const unsigned char *sqlite3_column_text(sqlite3_stmt *pStmt, int iCol)
```

Returns the text value of the column `iCol` (0-indexed) in the current row.

**Returns:** Pointer to the text string, or `NULL`.

### sqlite3_column_int

```c
int sqlite3_column_int(sqlite3_stmt *pStmt, int iCol)
```

Returns the integer value of the column `iCol`.

**Returns:** The integer value.

---

### Error Handling

### sqlite3_errmsg

```c
const char *sqlite3_errmsg(sqlite3 *db)
```

Returns a description of the error that caused the most recent call to fail.

**Returns:** A pointer to a static error string.