# sys/socket.h - Core Socket API

**Category:** 📂 sys/ (Linux & POSIX API)
**Header:** `<sys/socket.h>`
**Scope:** POSIX.1-2008

The `sys/socket.h` header defines the structures and functions necessary for creating and using network sockets. It provides the core API for IPC (Inter-Process Communication) and TCP/IP networking.

## Facilities Overview

| Facility Category | Key Symbols | Description |
|-------------------|-------------|-------------|
| [Socket Management](#socket-management) | [socket](#socket), [bind](#bind), [listen](#listen), [accept](#accept) | Lifecycle of a server-side socket. |
| [Client Connection](#client-connection) | [connect](#connect) | Establishing a connection to a remote host. |
| [Data Transfer](#data-transfer) | [send](#send), [recv](#recv), [sendto](#sendto), [recvfrom](#recvfrom) | Communicating over a socket. |
| [Configuration](#configuration) | [setsockopt](#setsockopt), [getsockopt](#getsockopt), [shutdown](#shutdown) | Modifying socket behavior and timeouts. |

---

## Types & Variables

### struct sockaddr

```c
struct sockaddr {
    sa_family_t sa_family;      /* Address family (AF_INET, AF_UNIX, etc.) */
    char        sa_data[14];    /* Protocol-specific address data */
};
```

Generic structure for socket addresses. Functions like `bind` and `connect` cast specific address structures (like `struct sockaddr_in`) to this type.

### struct sockaddr_storage

```c
struct sockaddr_storage;
```

A structure large enough to hold any socket address (e.g., IPv4 or IPv6), ensuring proper alignment.

### socklen_t

```c
typedef unsigned int socklen_t;
```

Integer type for socket address lengths.

---

## Functions

### Socket Management

### socket

```c
int socket(int domain, int type, int protocol)
```

Creates an endpoint for communication.

*   `domain`: `AF_INET` (IPv4), `AF_INET6` (IPv6), `AF_UNIX` (Local).
*   `type`: `SOCK_STREAM` (TCP), `SOCK_DGRAM` (UDP).
*   `protocol`: Usually `0` (auto-select).

**Returns:** A file descriptor for the new socket on success. Returns `-1` on error and sets `errno`.

### bind

```c
int bind(int sockfd, const struct sockaddr *addr, socklen_t addrlen)
```

Assigns a local protocol address to the socket `sockfd`.

**Returns:** `0` on success. Returns `-1` on error and sets `errno`.

### listen

```c
int listen(int sockfd, int backlog)
```

Marks `sockfd` as a passive socket that will be used to accept incoming connection requests using `accept`. `backlog` is the queue size.

**Returns:** `0` on success. Returns `-1` on error and sets `errno`.

### accept

```c
int accept(int sockfd, struct sockaddr *addr, socklen_t *addrlen)
```

Extracts the first connection request on the queue of listening socket `sockfd`. Creates a **new** socket file descriptor for the connection.

**Returns:** A new file descriptor for the accepted socket. Returns `-1` on error and sets `errno`.

<details><summary>Example</summary>

```c
#include <sys/socket.h>
#include <netinet/in.h>
#include <unistd.h>
#include <stdio.h>
#include <string.h>

int main(void) {
    int srv = socket(AF_INET, SOCK_STREAM, 0);
    if (srv == -1) return 1;

    struct sockaddr_in addr = {0};
    addr.sin_family = AF_INET;
    addr.sin_port = htons(8080);
    addr.sin_addr.s_addr = INADDR_ANY;
    
    if (bind(srv, (struct sockaddr *)&addr, sizeof(addr)) == -1) {
        perror("bind");
        return 1;
    }
    
    if (listen(srv, 10) == -1) {
        perror("listen");
        return 1;
    }
    
    printf("Listening on port 8080...\n");
    // Accept one connection then exit
    // int client = accept(srv, NULL, NULL);
    // close(client);
    
    close(srv);
    return 0;
}
```

</details>

---

### Client Connection

### connect

```c
int connect(int sockfd, const struct sockaddr *addr, socklen_t addrlen)
```

Connects the socket `sockfd` to the address specified by `addr`.

**Returns:** `0` on success. Returns `-1` on error and sets `errno`.

<details><summary>Example</summary>

```c
#include <sys/socket.h>
#include <netinet/in.h>
#include <arpa/inet.h>
#include <unistd.h>
#include <stdio.h>

int main(void) {
    int sock = socket(AF_INET, SOCK_STREAM, 0);
    
    struct sockaddr_in addr = {0};
    addr.sin_family = AF_INET;
    addr.sin_port = htons(8080);
    inet_pton(AF_INET, "127.0.0.1", &addr.sin_addr);
    
    if (connect(sock, (struct sockaddr *)&addr, sizeof(addr)) == -1) {
        perror("connect");
        return 1;
    }
    
    printf("Connected!\n");
    close(sock);
    return 0;
}
```

</details>

---

### Data Transfer

### send

```c
ssize_t send(int sockfd, const void *buf, size_t len, int flags)
```

Sends `len` bytes from `buf` to `sockfd`.

**Returns:** Number of bytes sent. Returns `-1` on error.

### recv

```c
ssize_t recv(int sockfd, void *buf, size_t len, int flags)
```

Receives up to `len` bytes from `sockfd` into `buf`.

**Returns:** Number of bytes received. Returns `0` if the peer has performed an orderly shutdown. Returns `-1` on error.

### setsockopt

```c
int setsockopt(int sockfd, int level, int optname, const void *optval, socklen_t optlen)
```

Sets the value of a socket option.

**Returns:** `0` on success. Returns `-1` on error.