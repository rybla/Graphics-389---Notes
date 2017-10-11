# Exploding Bunny

Jim's GL demo, illustrating the use of vertex and fragment shaders.


## Steps

1. Vertex Buffer objects
2. Compile vertex and fragment shaders and load
3. Configure the pipeline (choose which vertex/fragment shadeders to turn on)
4. Request to issue geometry (from data memory -> kernel (code) -> GPU)

## Details

### Vertex Buffers

* `vertex_buffer = glGenBuffers(x)` get id of new vertex buffer.
* `glBufferData(GL_ARRAY_BUFFER, ...vertex array... , GL_STATIC_DRAW)` stores the vertex info into the vertex buffer.

This is done for the faces and edges as well.

### Vertex and Fragment Shaders

* `vertex_shader = glCreateShader(GL_VERTEX_SHADER)`: get id of new vertex shader. 
* `glShaderSource(filename)`: connect to shader source file.

This is done for the fragment shaders as well.

### Example Vertex Shader Code

```c
// each will haev these
attribute vec3 vertex;
attribute vec3 normal;
attribute vec3 bary;

...

// static final
uniform vec3 light;
uniform vec3 eye;

...

// varying forms blend of varying vars
varying vec3 I;

...

void main() {
    vec3 n = normal;
    vec3 P = vertex;
    vec3 b = bary;

    ...

    // supports vector addition, product, and normalization
    vec3 l = normalize(light - P);
    ...
    vec3 r = -l + 2.0 * dot(l,n) * n;

    ...

    vec3 ambient = ...;
    vec3 diffuse = ...;
    vec3 specular = ...;

    I = ambient + diffuse + specular
    gl_Position = glProjectionMatrix * gl_ModeelViewMatrix * vec4(P,1.0);
}
```

## Fragment Shaders

Produces the `gl_FragColor`, per fragment, from "packet" per vertex.

Simple Example:

```c
#version 120

varying vec3 I;

uniform vec3 light;
uniform vec3 eye;
uniform int wireframe;

void main() {
    gl_FragColor = vec4(I,0.0); // 4th arg is alpha
}
```

## Phong Interpolation

`fs-phong-interpolation.c`:

```c
#version 120

// same setup as above

void main() {
    n = normal;
    P = vertex;
    bcoord = bary;

    gl_Position = glProjectionMatrix * gl_ModelViewMatrix * vec4(P,1.0)
}

```

## Exploding Bunny

`vs-mesh.c`:

```
// normal setup
...

void main() {
    n = normal;
    // wiggle the vertices along normal
    float wiggle_amount = pow(sin(  6 * (clock/60.0)), 2.0 ) / 10
}
```

`fs-mesh.c`:

```
// normal setup
...

// if close to edge, paint yellow
void main() {

    if ((wireframe == 1) && edge) {
        gl_FragColor = vec4(yellow,1.0);
    
    } else {
        // normal shading

    }
}
```

To choose the shader (in main python code):

```
...

shs = shaders[which_shader]
useProgram(shs)

// get info from shader
...
h_normal = GlGetUniformAttributeLocation(shs,'normal');
...


glDrawArrays(GL_TRAINGLES, 0, len(face.instances) * 3) // draw everything

...

glDisable... // turn off the arrays

...
```