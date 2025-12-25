# sys/stat.h - File Status and Metadata

**Category:** 📂 sys/ (Linux & POSIX API)
**Header:** `<sys/stat.h>`
**Scope:** POSIX.1-2008

The `sys/stat.h` header defines the structure of data returned by the functions `fstat`, `lstat`, and `stat`. It provides the mechanism to query file metadata (size, permissions, timestamps, type).

## Facilities Overview

| Facility Category | Key Symbols | Description |
|-------------------|-------------|-------------|
| [File Status](#file-status-functions) | [stat](#stat), [fstat](#fstat), [lstat](#lstat) | Retrieving information about a file. |
| [Directory Ops](#directory-operations) | [mkdir](#mkdir), [chmod](#chmod) | Creating directories and changing permissions. |
| [Macros](#types--variables) | [S_ISREG](#struct-stat), [S_ISDIR](#struct-stat) | Macros for checking file types. |

---

## Types & Variables

### struct stat

```c
struct stat {
    dev_t     st_dev;     /* ID of device containing file */
    ino_t     st_ino;     /* Inode number */
    mode_t    st_mode;    /* File type and mode */
    nlink_t   st_nlink;   /* Number of hard links */
    uid_t     st_uid;     /* User ID of owner */
    gid_t     st_gid;     /* Group ID of owner */
    dev_t     st_rdev;    /* Device ID (if special file) */
    off_t     st_size;    /* Total size, in bytes */
    time_t    st_atime;   /* Time of last access */
    time_t    st_mtime;   /* Time of last modification */
    time_t    st_ctime;   /* Time of last status change */
};
```

Contains metadata about a file.

**File Type Macros:**
Used with `st_mode`:
*   `S_ISREG(m)`: Regular file?
*   `S_ISDIR(m)`: Directory?
*   `S_ISCHR(m)`: Character device?
*   `S_ISBLK(m)`: Block device?
*   `S_ISFIFO(m)`: FIFO (named pipe)?
*   `S_ISLNK(m)`: Symbolic link?
*   `S_ISSOCK(m)`: Socket?

---

## Functions

### File Status Functions

### stat

```c
int stat(const char *pathname, struct stat *statbuf)
```

Retrieves information about the file pointed to by `pathname`. If `pathname` is a symbolic link, it returns information about the target.

**Returns:** `0` on success. Returns `-1` on error and sets `errno`.

<details><summary>Example</summary>

```c
#include <sys/stat.h>
#include <stdio.h>
#include <time.h>

int main(void) {
    struct stat sb;
    
    if (stat("file.txt", &sb) == -1) {
        perror("stat failed");
        return 1;
    }
    
    printf("File size: %lld bytes\n", (long long)sb.st_size);
    printf("Permissions: %o\n", sb.st_mode & 0777);
    
    if (S_ISDIR(sb.st_mode)) {
        printf("This is a directory.\n");
    }
    return 0;
}
```

</details>

### fstat

```c
int fstat(int fd, struct stat *statbuf)
```

Identical to `stat`, but retrieves information about the open file referred to by the file descriptor `fd`.

**Returns:** `0` on success. Returns `-1` on error.

### lstat

```c
int lstat(const char *pathname, struct stat *statbuf)
```

Identical to `stat`, except that if `pathname` is a symbolic link, then the link itself is stat-ed, not the file that it refers to.

**Returns:** `0` on success. Returns `-1` on error.

### Directory Operations

### mkdir

```c
int mkdir(const char *pathname, mode_t mode)
```

Attempts to create a directory named `pathname`. `mode` specifies the permissions (e.g., `0755`).

**Returns:** `0` on success. Returns `-1` on error.

<details><summary>Example</summary>

```c
#include <sys/stat.h>
#include <stdio.h>

int main(void) {
    if (mkdir("new_dir", 0755) == -1) {
        perror("mkdir failed");
        return 1;
    }
    printf("Directory created.\n");
    return 0;
}
```

</details>

### chmod

```c
int chmod(const char *pathname, mode_t mode)
```

Changes the permissions of a file.

**Returns:** `0` on success. Returns `-1` on error.