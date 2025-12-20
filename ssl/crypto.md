# openssl/evp.h - Enveleope Cryptography

The `openssl/evp.h` header provides a high-level interface to cryptographic functions.

## Facilities Overview

| Facility Category | Key Symbols | Description |
|-------------------|-------------|-------------|
| Message Digests | EVP_MD_CTX_new, EVP_DigestInit | Calculating hashes (SHA256, MD5, etc.). |
| Encryption | EVP_CIPHER_CTX_new, EVP_EncryptInit | Symmetric encryption (AES, etc.). |
| Public Key | EVP_PKEY_new, EVP_SignInit | Asymmetric cryptography and signatures. |

---

## Types & Variables

### EVP_MD_CTX

Context object for message digest (hash) operations.

### EVP_CIPHER_CTX

Context object for symmetric cipher operations.

### EVP_PKEY

Structure representing a public or private key.

---

## Functions

### Message Digests (Hashing)

#### EVP_DigestInit_ex

```c
int EVP_DigestInit_ex(EVP_MD_CTX *ctx, const EVP_MD *type, ENGINE *impl)
```

Initializes the digest context `ctx` to use `type`.

**Example:**

```c
#include <openssl/evp.h>
#include <stdio.h>

int main(void) {
    EVP_MD_CTX *ctx = EVP_MD_CTX_new();
    const EVP_MD *md = EVP_sha256();
    unsigned char hash[EVP_MAX_MD_SIZE];
    unsigned int len;

    EVP_DigestInit_ex(ctx, md, NULL);
    EVP_DigestUpdate(ctx, "Hello", 5);
    EVP_DigestFinal_ex(ctx, hash, &len);

    for(int i = 0; i < len; i++) printf("%02x", hash[i]);
    printf("\n");

    EVP_MD_CTX_free(ctx);
    return 0;
}
```

#### EVP_DigestUpdate

```c
int EVP_DigestUpdate(EVP_MD_CTX *ctx, const void *d, size_t cnt)
```

#### EVP_DigestFinal_ex

```c
int EVP_DigestFinal_ex(EVP_MD_CTX *ctx, unsigned char *md, unsigned int *s)
```

### Symmetric Encryption

#### EVP_EncryptInit_ex

```c
int EVP_EncryptInit_ex(EVP_CIPHER_CTX *ctx, const EVP_CIPHER *type, ...)
```