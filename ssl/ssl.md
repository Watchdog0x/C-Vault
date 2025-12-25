# openssl/ssl.h - Secure Sockets Layer

**Category:** 📂 ssl/ (Third-Party Ecosystem)
**Header:** `<openssl/ssl.h>`
**Scope:** OpenSSL / TLS 1.2+

The `openssl/ssl.h` header provides the interface for implementing secure communication protocols like TLS (Transport Layer Security) and SSL (Secure Sockets Layer). It handles encryption, certificate verification, and secure data transfer.

## Facilities Overview

| Facility Category | Key Symbols | Description |
|-------------------|-------------|-------------|
| [Initialization](#initialization) | [SSL_library_init](#ssl_library_init) | Loading algorithms and strings. |
| [Context Mgmt](#context-management) | [SSL_CTX_new](#ssl_ctx_new), [SSL_CTX_free](#ssl_ctx_free) | Global configuration for secure connections. |
| [Session Mgmt](#session-management) | [SSL_new](#ssl_new), [SSL_free](#ssl_free) | Individual secure connection handles. |
| [Connection](#connection-setup) | [SSL_connect](#ssl_connect), [SSL_accept](#ssl_accept) | Performing the TLS handshake. |
| [Data Transfer](#data-transfer) | [SSL_read](#ssl_read), [SSL_write](#ssl_write) | Encrypted input and output. |
| [Shutdown](#shutdown) | [SSL_shutdown](#ssl_shutdown) | Securely closing the connection. |

---

## Types & Variables

### SSL_CTX

```c
typedef struct ssl_ctx_st SSL_CTX;
```

Global context structure used to set up the environment for SSL/TLS settings, certificates, and private keys.

### SSL

```c
typedef struct ssl_st SSL;
```

Structure representing a single SSL/TLS connection. Contains the state of the connection (handshake status, encryption keys, etc).

### SSL_METHOD

```c
typedef struct ssl_method_st SSL_METHOD;
```

Structure describing the protocol version (e.g., TLS 1.2, TLS 1.3).

---

## Functions

### Initialization

### SSL_library_init

```c
int SSL_library_init(void)
```

Registers the available SSL/TLS ciphers and digests. (Explicitly required in older OpenSSL versions; implicit in 1.1.0+).

**Returns:** Always returns `1`.

---

### Context Management

### SSL_CTX_new

```c
SSL_CTX *SSL_CTX_new(const SSL_METHOD *method)
```

Creates a new `SSL_CTX` object as a framework for TLS/SSL enabled functions.

**Returns:** A pointer to the new context. Returns `NULL` on failure.

<details><summary>Example</summary>

```c
#include <openssl/ssl.h>

int main(void) {
    // SSL_library_init(); // Deprecated in 1.1.0
    SSL_CTX *ctx = SSL_CTX_new(TLS_client_method());
    
    if (!ctx) {
        // Handle error
        return 1;
    }
    
    SSL_CTX_free(ctx);
    return 0;
}
```

</details>

### SSL_CTX_free

```c
void SSL_CTX_free(SSL_CTX *ctx)
```

Frees an allocated `SSL_CTX` object.

---

### Session Management

### SSL_new

```c
SSL *SSL_new(SSL_CTX *ctx)
```

Creates a new `SSL` structure for a connection, inheriting settings from `ctx`.

**Returns:** A pointer to the new SSL structure. Returns `NULL` on failure.

### SSL_free

```c
void SSL_free(SSL *ssl)
```

Decrements the reference count of `ssl` and frees it if the reference count is 0.

---

### Connection Setup

### SSL_connect

```c
int SSL_connect(SSL *ssl)
```

Initiates the TLS/SSL handshake with a server. The underlying socket must already be connected.

**Returns:** `1` on success. `0` on shutdown. `<0` on error (use `SSL_get_error`).

### SSL_accept

```c
int SSL_accept(SSL *ssl)
```

Waits for a TLS/SSL handshake from a client.

**Returns:** `1` on success. `0` on shutdown. `<0` on error.

---

### Data Transfer

### SSL_read

```c
int SSL_read(SSL *ssl, void *buf, int num)
```

Reads `num` bytes from the secure connection into `buf`.

**Returns:** The number of bytes read (> 0). Returns `0` if the connection is closed. Returns `< 0` on error.

### SSL_write

```c
int SSL_write(SSL *ssl, const void *buf, int num)
```

Writes `num` bytes from `buf` into the secure connection.

**Returns:** The number of bytes written (> 0). Returns `< 0` on error.

### SSL_shutdown

```c
int SSL_shutdown(SSL *ssl)
```

Shuts down the SSL/TLS connection.

**Returns:** `1` if shutdown is complete. `0` if shutdown is in progress (sent `close_notify`). `<0` on error.