## Graphics Pipleline

2. **Vertex Processing** yields transformed geometry. Vertex coord -> screen coord
3. **Rasterization** yields fragments.
3. **Fragment Processing** yields color / opacity. Object -> Pixelated image "fragments"; Each pixel location -> info for shading
4. **Blendng** yields frame buffer image. Combine shading info for each pixel of an object with all the other shadings that 'use' the pixel (each pixel respectively).


## Programmable Steps

Both use GLSL.

- **Vertex Processing** with _Vertex Shader_.

- **Fragment Processing** Programmable with _Fragment Shader_.

In some stages, vertex/fragment processing can access other data e.g. textures. This can be used for material color normal "offset" via dispacement mapping / bump mapping.

This programmability was implemented in early 2000's in order to let game programmers go in and add their own effects, without it having to be supported by the base code.


## CPU, GPU

CPU can send "kernels" to GPU, and comp. memory can send "buffers" to GPU's data memory.

Steps:
- Set up buffers (register, copy, ...)
- Compile and load the shaders (kernels)
- Constantly: ask GPU to run a shader with respect to some buffers.

So the shader is like: ```shaderX(d1,d2,...,dk) = calculate stuff```, and we pick which X to use and what (d1,d2,...,dk) to give.


## GPU

THe GPU is a processing unit with 100's of 'streaming' processors that can run in parallel.

Nvidia is a main company that works with GPU's and research for them.