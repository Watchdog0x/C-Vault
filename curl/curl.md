# curl/curl.h - URL Transfer Library

**Category:** 📂 curl/ (Third-Party Ecosystem)
**Header:** `<curl/curl.h>`
**Scope:** libcurl / Multi-protocol

The `curl/curl.h` header is the primary interface for libcurl, a client-side URL transfer library supporting HTTP, FTP, SMTP, and many other protocols.

## Facilities Overview

| Facility Category | Key Symbols | Description |
|-------------------|-------------|-------------|
| [Global Init](#global-initialization) | [curl_global_init](#curl_global_init) | Library-wide initialization. |
| [Easy Interface](#easy-interface) | [curl_easy_init](#curl_easy_init), [curl_easy_perform](#curl_easy_perform) | Synchronous, single-transfer API. |
| [Configuration](#configuration) | [curl_easy_setopt](#curl_easy_setopt) | Configuring transfer behavior (URL, callbacks). |
| [Error Handling](#error-handling) | [curl_easy_strerror](#curl_easy_strerror) | Converting error codes to strings. |
| [Cleanup](#cleanup) | [curl_easy_cleanup](#curl_easy_cleanup) | Releasing resources. |

---

## Types & Variables

### CURL

```c
typedef void CURL;
```

An opaque handle for a single transfer (the "easy" handle).

### CURLcode

```c
typedef enum { ... } CURLcode;
```

Enumeration of error codes returned by easy functions (e.g., `CURLE_OK`, `CURLE_COULDNT_CONNECT`).

### CURLoption

```c
typedef enum { ... } CURLoption;
```

Enumeration of options used with `curl_easy_setopt`.

---

## Functions

### Global Initialization

### curl_global_init

```c
CURLcode curl_global_init(long flags)
```

Initializes the libcurl library. This must be called at least once before any other libcurl function is used.

*   `flags`: usually `CURL_GLOBAL_ALL` or `CURL_GLOBAL_DEFAULT`.

**Returns:** `CURLE_OK` (0) on success. Non-zero on failure.

---

### Easy Interface

### curl_easy_init

```c
CURL *curl_easy_init(void)
```

Allocates and initializes a libcurl easy handle.

**Returns:** A pointer to the handle on success, or `NULL` on error.

### curl_easy_setopt

```c
CURLcode curl_easy_setopt(CURL *handle, CURLoption option, ...)
```

Sets options for a libcurl easy handle.

**Common Options:**
*   `CURLOPT_URL`: String URL to fetch.
*   `CURLOPT_FOLLOWLOCATION`: Follow HTTP redirects (long: 1).
*   `CURLOPT_WRITEFUNCTION`: Callback for writing received data.

**Returns:** `CURLE_OK` on success.

### curl_easy_perform

```c
CURLcode curl_easy_perform(CURL *handle)
```

Performs the network transfer as described by the options. This function is blocking.

**Returns:** `CURLE_OK` on success.

<details><summary>Example</summary>

```c
#include <curl/curl.h>
#include <stdio.h>

int main(void) {
    curl_global_init(CURL_GLOBAL_DEFAULT);
    CURL *curl = curl_easy_init();
    
    if (curl) {
        curl_easy_setopt(curl, CURLOPT_URL, "https://example.com");
        
        CURLcode res = curl_easy_perform(curl);
        
        if (res != CURLE_OK) {
            fprintf(stderr, "Request failed: %s\n", curl_easy_strerror(res));
        }
        
        curl_easy_cleanup(curl);
    }
    
    curl_global_cleanup();
    return 0;
}
```

</details>

### curl_easy_cleanup

```c
void curl_easy_cleanup(CURL *handle)
```

Closes the easy handle and releases associated resources.

---

### Error Handling

### curl_easy_strerror

```c
const char *curl_easy_strerror(CURLcode errornum)
```

Returns a string describing the `CURLcode` error code.

**Returns:** A static string describing the error.