# sqlite3.h - SQLite Embedded Database

The `sqlite3.h` header is the interface for SQLite, a high-reliability, full-featured, SQL database engine.

## Facilities Overview

| Facility Category | Key Symbols | Description |
|-------------------|-------------|-------------|
| Database Mgmt | sqlite3_open, sqlite3_close | Opening and closing database files. |
| Execution | sqlite3_exec | Executing one or more SQL statements. |
| Prepared Stmts| sqlite3_prepare, sqlite3_step | Compiling SQL and iterating over results. |
| Parameter Binding| sqlite3_bind_int, sqlite3_bind_text | Safe passing of values into SQL. |
| Column Access | sqlite3_column_text, sqlite3_column_int| Retrieving query results. |

---

## Types & Variables

### sqlite3

An opaque handle representing a database connection.

### sqlite3_stmt

An opaque handle representing a single compiled SQL statement.

---

## Functions

### Connection Management

#### sqlite3_open

```c
int sqlite3_open(const char *filename, sqlite3 **ppDb)
```

Opens a new connection to a SQLite database.

**Example:**

```c
#include <sqlite3.h>
#include <stdio.h>

int main(void) {
    sqlite3 *db;
    if (sqlite3_open(":memory:", &db) == SQLITE_OK) {
        printf("Memory database opened.\n");
        sqlite3_close(db);
    }
    return 0;
}
```

#### sqlite3_close

```c
int sqlite3_close(sqlite3 *db)
```

Closes a database connection.

---

### Prepared Statements

#### sqlite3_prepare_v2

```c
int sqlite3_prepare_v2(sqlite3 *db, const char *zSql, int nByte, sqlite3_stmt **ppStmt, const char **pzTail)
```

Compiles a SQL statement into a prepared statement.

#### sqlite3_step

```c
int sqlite3_step(sqlite3_stmt *pStmt)
```

Evaluates a prepared statement.

#### sqlite3_finalize

```c
int sqlite3_finalize(sqlite3_stmt *pStmt)
```

Destroys a prepared statement object.