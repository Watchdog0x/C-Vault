# GL/gl.h - OpenGL Graphics API

The `GL/gl.h` header provides the interface for OpenGL, a cross-platform API for rendering 2D and 3D vector graphics.

## Facilities Overview

| Facility Category | Key Symbols | Description |
|-------------------|-------------|-------------|
| State Management | glClear, glClearColor, glEnable | Configuring the rendering pipeline state. |
| Buffers | glGenBuffers, glBufferData | Managing GPU-side memory blocks. |
| Shaders | glCreateShader, glCompileShader | Compiling and using programmable shaders. |
| Drawing | glDrawArrays, glDrawElements | Initiating rendering commands. |
| Transformations | glViewport, glScissor | Mapping NDC to screen coordinates. |

---

## Types & Variables

### GLuint

Standard unsigned integer type for OpenGL.

### GLint

Standard signed integer type for OpenGL.

### GLfloat

Standard floating-point type for OpenGL.

### GL_COLOR_BUFFER_BIT

Bitmask for clearing the color buffer.

---

## Functions

### State Control

#### glClearColor

```c
void glClearColor(GLfloat red, GLfloat green, GLfloat blue, GLfloat alpha)
```

Sets the color used when clearing the screen.

**Example:**

```c
#include <GL/gl.h>

void init() {
    glClearColor(0.2f, 0.3f, 0.3f, 1.0f);
}
```

#### glClear

```c
void glClear(GLbitfield mask)
```

Performs the clear operation on the specified buffers.

---

### Buffer Management

#### glGenBuffers

```c
void glGenBuffers(GLsizei n, GLuint *buffers)
```

Generates `n` buffer object names.

#### glBindBuffer

```c
void glBindBuffer(GLenum target, GLuint buffer)
```

Binds a named buffer object to a specified target.

#### glBufferData

```c
void glBufferData(GLenum target, GLsizeiptr size, const void *data, GLenum usage)
```

Creates and initializes a buffer object's data store.

---

### Drawing

#### glDrawArrays

```c
void glDrawArrays(GLenum mode, GLint first, GLsizei count)
```

Renders primitives from array data.