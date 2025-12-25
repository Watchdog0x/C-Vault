# GL/gl.h - OpenGL Graphics API

**Category:** 📂 gl/ (Graphics & Display)
**Header:** `<GL/gl.h>`
**Scope:** OpenGL 1.1+ (Legacy/Core)

The `GL/gl.h` header provides the interface for OpenGL, a cross-platform API for rendering 2D and 3D vector graphics. It interacts with the GPU driver to perform hardware-accelerated rendering.

## Facilities Overview

| Facility Category | Key Symbols | Description |
|-------------------|-------------|-------------|
| [State Control](#state-control) | [glClear](#glclear), [glClearColor](#glclearcolor), [glEnable](#glenable) | Configuring the global rendering state. |
| [Drawing](#drawing) | [glDrawArrays](#gldrawarrays), [glDrawElements](#gldrawelements) | Initiating rendering commands from buffers. |
| [Buffers](#buffer-management) | [glGenBuffers](#glgenbuffers), [glBindBuffer](#glbindbuffer) | Managing GPU-side memory (VBOs). |
| [View System](#transformations) | [glViewport](#glviewport) | Mapping Normalized Device Coordinates to window pixels. |

---

## Types & Variables

### GLuint

```c
typedef unsigned int GLuint;
```

Standard unsigned 32-bit integer.

### GLint

```c
typedef int GLint;
```

Standard signed 32-bit integer.

### GLfloat

```c
typedef float GLfloat;
```

Standard 32-bit floating point.

### GLenum

```c
typedef unsigned int GLenum;
```

Enumerated type for OpenGL constants (e.g., `GL_FLOAT`, `GL_TRIANGLES`).

---

## Functions

### State Control

### glClearColor

```c
void glClearColor(GLfloat red, GLfloat green, GLfloat blue, GLfloat alpha)
```

Specifies the red, green, blue, and alpha values used by `glClear` to clear the color buffers. Values are clamped to [0, 1].

**Returns:** None.

<details><summary>Example</summary>

```c
#include <GL/gl.h>

void setup_render(void) {
    // Set clear color to teal
    glClearColor(0.0f, 0.5f, 0.5f, 1.0f);
}
```

</details>

### glClear

```c
void glClear(GLbitfield mask)
```

Sets the bitplane area of the window to values previously selected by `glClearColor`, `glClearDepth`, and `glClearStencil`.
*   `mask`: Bitwise OR of `GL_COLOR_BUFFER_BIT`, `GL_DEPTH_BUFFER_BIT`, etc.

**Returns:** None.

---

### Buffer Management

### glGenBuffers

```c
void glGenBuffers(GLsizei n, GLuint *buffers)
```

Generates `n` buffer object names (IDs). No buffer objects are associated with the returned names until they are first bound.

**Returns:** None.

### glBindBuffer

```c
void glBindBuffer(GLenum target, GLuint buffer)
```

Binds a named buffer object to the specified buffer binding point (target).
*   `target`: `GL_ARRAY_BUFFER` (Vertex attributes), `GL_ELEMENT_ARRAY_BUFFER` (Indices), etc.

**Returns:** None.

### glBufferData

```c
void glBufferData(GLenum target, GLsizeiptr size, const void *data, GLenum usage)
```

Creates and initializes a buffer object's data store.
*   `usage`: `GL_STATIC_DRAW`, `GL_DYNAMIC_DRAW`, etc.

**Returns:** None.

<details><summary>Example</summary>

```c
#include <GL/gl.h>

void upload_triangle(GLuint vbo) {
    float vertices[] = {
        -0.5f, -0.5f, 0.0f,
         0.5f, -0.5f, 0.0f,
         0.0f,  0.5f, 0.0f
    };
    
    glBindBuffer(GL_ARRAY_BUFFER, vbo);
    glBufferData(GL_ARRAY_BUFFER, sizeof(vertices), vertices, GL_STATIC_DRAW);
}
```

</details>

---

### Drawing

### glDrawArrays

```c
void glDrawArrays(GLenum mode, GLint first, GLsizei count)
```

Render primitives from array data.
*   `mode`: `GL_TRIANGLES`, `GL_LINES`, etc.

**Returns:** None.

### glViewport

```c
void glViewport(GLint x, GLint y, GLsizei width, GLsizei height)
```

Specifies the affine transformation of x and y from normalized device coordinates to window coordinates.

**Returns:** None.