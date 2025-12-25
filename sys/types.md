# sys/types.h - System Data Types

**Category:** 📂 sys/ (Linux & POSIX API)
**Header:** `<sys/types.h>`
**Scope:** POSIX.1-2008

The `sys/types.h` header defines data types used in system source code. These types are often used as return types or arguments for system calls to ensure portability across different architectures.

## Facilities Overview

| Facility Category | Description |
|-------------------|-------------|
| [Process Types](#process-ids) | `pid_t`, `id_t` |
| [File Types](#file-attributes) | `dev_t`, `ino_t`, `mode_t`, `nlink_t` |
| [User/Group IDs](#user-and-group-ids) | `uid_t`, `gid_t` |
| [Sizes and Time](#sizes-and-time) | `off_t`, `ssize_t`, `time_t`, `clock_t` |
| [IPC Keys](#ipc) | `key_t` |

---

## Types & Variables

### Process IDs

#### pid_t

```c
typedef signed integer pid_t;
```

Used for process IDs and process group IDs. Returned by `fork()`, `getpid()`.

### User and Group IDs

#### uid_t

```c
typedef integer uid_t;
```

Used for user IDs.

#### gid_t

```c
typedef integer gid_t;
```

Used for group IDs.

### File Attributes

#### dev_t

```c
typedef integer dev_t;
```

Used for device IDs.

#### ino_t

```c
typedef unsigned integer ino_t;
```

Used for file serial numbers (inodes).

#### mode_t

```c
typedef integer mode_t;
```

Used for some file attributes (permissions and type).

#### nlink_t

```c
typedef integer nlink_t;
```

Used for link counts.

### Sizes and Time

#### off_t

```c
typedef signed integer off_t;
```

Used for file sizes and offsets.

#### ssize_t

```c
typedef signed integer ssize_t;
```

Used for a count of bytes or an error indication (can be -1).

### IPC

#### key_t

```c
typedef integer key_t;
```

Used for XSI interprocess communication IDs (SysV IPC).
