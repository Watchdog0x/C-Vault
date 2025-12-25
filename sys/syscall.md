# sys/syscall.h - System Call Numbers and Invocation

**Category:** 📂 sys/ (Linux & POSIX API)
**Header:** `<sys/syscall.h>` / `<unistd.h>`
**Scope:** Linux / POSIX

The `sys/syscall.h` header defines the system call numbers (constants starting with `SYS_`). The `syscall()` function (declared in `<unistd.h>`) is used to invoke a system call by number, which is useful for newer system calls that do not yet have a C library wrapper.

## Facilities Overview

| Facility Category | Key Symbols | Description |
|-------------------|-------------|-------------|
| [Invocation](#syscall) | [syscall](#syscall) | Invoking a system call directly by number. |
| [File Ops](#file-operations) | `SYS_open`, `SYS_read` | File system operations. |
| [Process Ops](#process-control) | `SYS_fork`, `SYS_execve` | Process management. |
| [Network Ops](#network-operations) | `SYS_socket`, `SYS_connect` | Networking. |

---

## Functions

### syscall

```c
long syscall(long number, ...);
```

Performs the system call specified by `number` with the given arguments.

**Returns:** The return value of the system call. On error, it returns `-1` and sets `errno`.

<details><summary>Example</summary>

```c
#define _GNU_SOURCE
#include <unistd.h>
#include <sys/syscall.h>
#include <stdio.h>
#include <sys/types.h>

int main(void) {
    // Invoke the 'gettid' system call (which often has no glibc wrapper)
    pid_t tid = syscall(SYS_gettid);
    
    printf("Thread ID: %d\n", tid);
    return 0;
}
```

</details>

---

## System Call Reference (x86_64)

The following tables reference standard Linux system calls (Linux 6.x / x86_64). Architecture-specific numbers vary, but `SYS_` names are consistent.

### File Operations

| Constant | Description |
|:---|:---|
| `SYS_open` | Open and possibly create a file. |
| `SYS_close` | Close a file descriptor. |
| `SYS_read` | Read from a file descriptor. |
| `SYS_write` | Write to a file descriptor. |
| `SYS_stat` | Get file status. |
| `SYS_fstat` | Get file status (by FD). |
| `SYS_lstat` | Get file status (symbolic link). |
| `SYS_lseek` | Reposition read/write file offset. |
| `SYS_pread64` | Read from a file descriptor at a given offset. |
| `SYS_pwrite64` | Write to a file descriptor at a given offset. |
| `SYS_ioctl` | Control device. |
| `SYS_access` | Check user's permissions for a file. |
| `SYS_pipe` | Create pipe. |
| `SYS_dup` | Duplicate a file descriptor. |
| `SYS_dup2` | Duplicate a file descriptor to a specific index. |
| `SYS_sendfile` | Transfer data between file descriptors. |
| `SYS_fcntl` | Manipulate file descriptor. |
| `SYS_mkdir` | Create a directory. |
| `SYS_rmdir` | Delete a directory. |
| `SYS_unlink` | Delete a name and possibly the file it refers to. |
| `SYS_rename` | Change the name or location of a file. |
| `SYS_chmod` | Change permissions of a file. |
| `SYS_chown` | Change ownership of a file. |
| `SYS_getcwd` | Get current working directory. |
| `SYS_chdir` | Change working directory. |
| `SYS_getdents` | Get directory entries (directory listing). |

### Process Control

| Constant | Description |
|:---|:---|
| `SYS_fork` | Create a child process. |
| `SYS_vfork` | Create a child process and block parent. |
| `SYS_execve` | Execute program. |
| `SYS_exit` | Terminate the calling process. |
| `SYS_wait4` | Wait for process to change state. |
| `SYS_kill` | Send signal to a process. |
| `SYS_getpid` | Get process ID. |
| `SYS_getppid` | Get parent process ID. |
| `SYS_getuid` / `SYS_getgid` | Get user/group identity. |
| `SYS_setuid` / `SYS_setgid` | Set user/group identity. |
| `SYS_ptrace` | Process trace. |
| `SYS_sched_yield` | Yield the processor. |
| `SYS_clone` | Create a child process (fundamental thread creation). |

### Network Operations

| Constant | Description |
|:---|:---|
| `SYS_socket` | Create an endpoint for communication. |
| `SYS_connect` | Initiate a connection on a socket. |
| `SYS_accept` | Accept a connection on a socket. |
| `SYS_sendto` | Send a message on a socket. |
| `SYS_recvfrom` | Receive a message from a socket. |
| `SYS_sendmsg` | Send a message on a socket using a message structure. |
| `SYS_recvmsg` | Receive a message from a socket using a message structure. |
| `SYS_shutdown` | Shut down part of a full-duplex connection. |
| `SYS_bind` | Bind a name to a socket. |
| `SYS_listen` | Listen for connections on a socket. |
| `SYS_setsockopt` | Set options on sockets. |
| `SYS_getsockopt` | Get options on sockets. |

### Memory Management

| Constant | Description |
|:---|:---|
| `SYS_brk` | Change data segment size (heap). |
| `SYS_mmap` | Map files or devices into memory. |
| `SYS_munmap` | Unmap files or devices from memory. |
| `SYS_mprotect` | Set protection on a region of memory. |
| `SYS_mremap` | Remap a virtual memory address. |
| `SYS_madvise` | Give advice about use of memory. |
| `SYS_shmget` | Allocate System V shared memory segment. |
| `SYS_shmat` | Attach System V shared memory segment. |

### Time & Timers

| Constant | Description |
|:---|:---|
| `SYS_time` | Get time in seconds. |
| `SYS_gettimeofday` | Get time. |
| `SYS_nanosleep` | High-resolution sleep. |
| `SYS_alarm` | Set an alarm clock for delivery of a signal. |
| `SYS_timer_create` | Create a POSIX per-process timer. |
| `SYS_clock_gettime` | Retrieve the time of the specified clock. |

### Modern / Advanced Linux

| Constant | Description |
|:---|:---|
| `SYS_gettid` | Get thread ID. |
| `SYS_futex` | Fast user-space locking. |
| `SYS_epoll_create` | Open an epoll file descriptor. |
| `SYS_epoll_ctl` | Control interface for an epoll file descriptor. |
| `SYS_epoll_wait` | Wait for an I/O event on an epoll file descriptor. |
| `SYS_signalfd` | Create a file descriptor to accept signals. |
| `SYS_eventfd` | Create a file descriptor for event notification. |
| `SYS_timerfd_create` | Create a timer that delivers notifications via a file descriptor. |
| `SYS_io_uring_setup` | Setup asynchronous I/O ring. |
| `SYS_bpf` | Perform a command on an extended BPF map or program. |
| `SYS_memfd_create` | Create an anonymous file. |
| `SYS_getrandom` | Obtain a series of random bytes. |
| `SYS_pidfd_open` | Obtain a file descriptor that refers to a process. |
