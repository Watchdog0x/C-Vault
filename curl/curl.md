# curl/curl.h - URL Transfer Library

The `curl/curl.h` header is the primary interface for libcurl, a multiprotocol file transfer library.

## Facilities Overview

| Facility Category | Key Symbols | Description |
|-------------------|-------------|-------------|
| Easy Interface | curl_easy_init, curl_easy_perform | Synchronous, single-transfer API. |
| Multi Interface | curl_multi_init, curl_multi_perform | Asynchronous, multiple concurrent transfers. |
| Options | CURLOPT_URL, CURLOPT_WRITEDATA | Configuring transfer behavior and targets. |
| Cleanup | curl_easy_cleanup, curl_global_cleanup | Releasing resources. |

---

## Types & Variables

### CURL

An opaque handle for a single transfer (the "easy" handle).

### CURLcode

Enumeration of error codes returned by easy functions.

### CURLoption

Enumeration of options used with `curl_easy_setopt`.

---

## Functions

### Easy Interface

#### curl_easy_init

```c
CURL *curl_easy_init(void)
```

Allocates and initializes a libcurl easy handle.

**Example:**

```c
#include <curl/curl.h>

int main(void) {
    CURL *curl = curl_easy_init();
    if (curl) {
        curl_easy_cleanup(curl);
    }
    return 0;
}
```

#### curl_easy_setopt

```c
CURLcode curl_easy_setopt(CURL *handle, CURLoption option, ...)
```

Sets options for a libcurl easy handle.

#### curl_easy_perform

```c
CURLcode curl_easy_perform(CURL *handle)
```

Performs a network transfer.

#### curl_easy_cleanup

```c
void curl_easy_cleanup(CURL *handle)
```