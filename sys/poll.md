# poll.h - Input/Output Multiplexing

The `poll.h` header defines the `poll` function and the structure used to wait for events on multiple file descriptors.

## Facilities Overview

| Facility Category | Key Symbols | Description |
|-------------------|-------------|-------------|
| Waiting | poll | Waiting for events on multiple file descriptors. |
| Structures | struct pollfd | Structure defining the file descriptor and events to watch. |
| Event Flags | POLLIN, POLLOUT, POLLERR | Bitmasks for specifying events. |

---

## Types & Variables

### struct pollfd

Structure defining a file descriptor to be monitored:

```c
struct pollfd {
    int   fd;         /* file descriptor */
    short events;     /* requested events */
    short revents;    /* returned events */
};
```

### Event Flags

- `POLLIN`: There is data to read.
- `POLLOUT`: Writing is now possible.
- `POLLERR`: Error condition.
- `POLLHUP`: Hang up.

---

## Functions

### I/O Multiplexing

#### poll

```c
int poll(struct pollfd *fds, nfds_t nfds, int timeout)
```

Waits for one of a set of file descriptors to become ready to perform I/O.

**Example:**

```c
#include <poll.h>
#include <stdio.h>
#include <unistd.h>

int main(void) {
    struct pollfd fds[1];
    fds[0].fd = STDIN_FILENO;
    fds[0].events = POLLIN;

    int ret = poll(fds, 1, 5000); // 5 second timeout
    if (ret > 0 && (fds[0].revents & POLLIN)) {
        printf("Data is available to read!\n");
    }
    return 0;
}
```