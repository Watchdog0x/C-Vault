# sys/socket.h - Core Socket API

The `sys/socket.h` header defines the structures and functions necessary for creating and using network sockets.

## Facilities Overview

| Facility Category | Key Symbols | Description |
|-------------------|-------------|-------------|
| Socket Management | socket, bind, listen, accept | Lifecycle of a server-side socket. |
| Client Connection | connect | Establishing a connection to a remote host. |
| Data Transfer | send, recv, sendto, recvfrom | Communicating over a socket. |
| Configuration | setsockopt, getsockopt | Modifying socket behavior and timeouts. |

---

## Types & Variables

### struct sockaddr

Generic structure for socket addresses.

```c
struct sockaddr {
    sa_family_t sa_family;      /* address family, AF_xxx */
    char        sa_data[14];    /* 14 bytes of protocol address */
};
```

---

## Functions

### Socket Management

#### socket

```c
int socket(int domain, int type, int protocol)
```

Creates an endpoint for communication.

#### bind

```c
int bind(int sockfd, const struct sockaddr *addr, socklen_t addrlen)
```

Assigns a local address to the socket `sockfd`.

#### accept

```c
int accept(int sockfd, struct sockaddr *addr, socklen_t *addrlen)
```

Extracts the first connection request on the queue and returns a new file descriptor.

**Example:**

```c
#include <sys/socket.h>
#include <netinet/in.h>
#include <unistd.h>

int main(void) {
    int srv = socket(AF_INET, SOCK_STREAM, 0);
    struct sockaddr_in addr = {0};
    addr.sin_family = AF_INET;
    addr.sin_port = htons(8080);
    
    bind(srv, (struct sockaddr *)&addr, sizeof(addr));
    listen(srv, 10);
    
    // int client = accept(srv, NULL, NULL);
    // close(srv);
    return 0;
}
```