# openssl/ssl.h - Secure Sockets Layer

The `openssl/ssl.h` header provides the interface for implementing secure communication protocols like TLS and SSL.

## Facilities Overview

| Facility Category | Key Symbols | Description |
|-------------------|-------------|-------------|
| Context Mgmt | SSL_CTX_new, SSL_CTX_free | Global configuration for secure connections. |
| Session Mgmt | SSL_new, SSL_free | Individual secure connection handles. |
| Connection | SSL_connect, SSL_accept | Performing the TLS handshake. |
| Data Transfer | SSL_read, SSL_write | Encrypted input and output. |
| Error Handling | SSL_get_error | Retrieving SSL-specific error codes. |

---

## Types & Variables

### SSL_CTX

Global context structure for SSL/TLS settings, certificates, and keys.

### SSL

Structure representing a single SSL/TLS connection.

### BIO

Source/Sink abstraction used to wrap sockets or memory for SSL.

---

## Functions

### Connection Management

#### SSL_CTX_new

```c
SSL_CTX *SSL_CTX_new(const SSL_METHOD *method)
```

Creates a new `SSL_CTX` object as a framework for TLS/SSL enabled functions.

**Example:**

```c
#include <openssl/ssl.h>

int main(void) {
    SSL_library_init();
    SSL_CTX *ctx = SSL_CTX_new(TLS_client_method());
    if (ctx) SSL_CTX_free(ctx);
    return 0;
}
```

#### SSL_new

```c
SSL *SSL_new(SSL_CTX *ctx)
```

Creates a new `SSL` structure for a connection.

### Data Transfer

#### SSL_read

```c
int SSL_read(SSL *ssl, void *buf, int num)
```

#### SSL_write

```c
int SSL_write(SSL *ssl, const void *buf, int num)
```

#### SSL_connect

```c
int SSL_connect(SSL *ssl)
```