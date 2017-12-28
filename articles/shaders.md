---
uid: shaders-and-resources
---

# Shaders and Resources

[Shaders](xref:Veldrid.Shader) are a fundamental device resource in Veldrid. Each Shader object represents a single shader module created from specialized shader code. Shader objects are combined into shader sets which are one input into a [Pipeline](xref:Veldrid.Pipeline). Shaders can read from several other device resources while executing.

## Creating a Shader

A [ShaderDescription](xref:Veldrid.ShaderDescription) takes two pieces of information. The first piece is the [stage](xref:Veldrid.ShaderStages) the shader is applicable to. The second piece of information is an API-specific byte array containing the shader code itself. The contents of this byte array depend on the specific graphics API being used (see [GraphicsDevice.BackendType](xref:Veldrid.GraphicsDevice#Veldrid_GraphicsDevice_BackendType)).

* Direct3D11: `ShaderBytes` must contain HLSL bytecode.
* Vulkan: `ShaderBytes` must contain SPIR-V bytecode.
* OpenGL: `ShaderBytes` must contain ASCII-encoded GLSL text.

## Shader Resources

Shader objects have a unique relationship with [ResourceLayouts](xref:Veldrid.ResourceLayout) and [ResourceSets](xref:Veldrid.ResourceSet). When creating a Pipeline, the provided [ResourceLayouts](xref:Veldrid.GraphicsPipelineDescription#Veldrid_GraphicsPipelineDescription_ResourceLayouts) must match the actual resource types that are specified in the shader code.

ResourceLayouts merely define the layout of resources expected by a set of shaders. When draw commands are executed, the actual shader resources (DeviceBuffers, TextureViews, and Samplers) are determined based on the CommandList's currently-bound [ResourceSets](xref:Veldrid.ResourceSet).

ResourceLayouts themselves contain a set of one-or-more resource elements. Additionally, multiple ResourceLayout objects can be used to define the inputs of a shader set. When this is done, each ResourceLayout must be matched by a corresponding ResourceSet containing the appropriate resource types. ResourceSet objects are bound to a particular slot. The slot corresponds to the index of the matching ResourceLayout given in the [ResourceLayouts](xref:Veldrid.GraphicsPipelineDescription#Veldrid_GraphicsPipelineDescription_ResourceLayouts) field of the [GraphicsPipelineDescription](xref:Veldrid.GraphicsPipelineDescription) or [ComputePipelineDescription](xref:Veldrid.ComputePipelineDescription).

Generally, it is advisable to group resources into sets and layouts which are common or shared. For example, it is a good idea to group the camera's view and projection matrix, and other scene-level information, into a single ResourceLayout and shared ResourceSet. This allows many objects to be rendered using the same bound ResourceSet. Specific object Pipelines can utilize extra ResourceLayouts and ResourceSets to accomodate their specific rendering requirements while still utilizing shared resources when it makes sense. Changing ResourceSets can be a costly operation, so re-using them as much as possible can help performance.

## Types of Resources

There are many types of shader resources available in Veldrid.

### Uniform Buffer

A uniform [DeviceBuffer](xref:Veldrid.DeviceBuffer) is a resource used to store a small-to-medium amount of data for a shader to access. Uniform buffers are commonly used to store per-object transformations, camera transformations and properties, and other information.

A DeviceBuffer must be created with [BufferUsage.UniformBuffer](xref:Veldrid.BufferUsage) to be used as a uniform buffer.

Uniform buffers correspond to "cbuffer" blocks in HLSL, and a uniform variable or uniform block in GLSL.

### Structured Buffer

A structured buffer is another kind of DeviceBuffer resource available to shaders. Like uniform buffers, they can be used to store arbitrary data, but are generally much larger. Structured buffers are used to store a large amount of a single kind of value (a "structure"). The size of the structure that is stored must be designated upon DeviceBuffer creation (see [BufferDescription.StructureByteStride](xref:Veldrid.BufferDescription#Veldrid_BufferDescription_StructureByteStride)).

Structured buffers may be read-only or read-write. Read-write buffers can be written to in the fragment and compute stages, allowing arbitrary data to be output by shaders. Read-only structured buffers must be created with the [BufferUsage.StructuredBufferReadOnly](xref:Veldrid.BufferUsage) flag, and read-write structured buffers must be created with the [BufferUsage.StructuredBufferReadWrite](xref:Veldrid.BufferUsage) flag.

Structure buffers correspond to `StructuredBuffer<T>` or `RWStructuredBuffer<T>` objects in HLSL, and `readonly` or normal "buffer blocks" in GLSL.

### TextureView

A [TextureView](xref:Veldrid.TextureView) is a resource which gives a shader read-only or read-write access to a [Texture](xref:Veldrid.Texture). A TextureView allows a subset of the Texture object's dimensions to be accessible, enabling a single slice of an array texture to be read from or written to, for example.

Read-only TextureView objects must have a [Target](xref:Veldrid.TextureViewDescription#Veldrid_TextureViewDescription_Target) that was created with the [TextureUsage.Sampled](xref:Veldrid.TextureUsage) flag. Read-write TextureViews must target a Texture created with the [TextureUsage.Storage](xref:Veldrid.TextureUsage) flag.

Read-only TextureViews correspond to "Texture" objects in HLSL (Texture2D, Texture2DArray, TextureCube, etc). They correspond to "sampler" objects in OpenGL-flavored GLSL (sampler2D, sampler2DArray, samplerCube, etc.), and "texture" objects in Vulkan-flavored GLSL (texture2D, texture2DArray, textureCube, etc.).

Read-write TextureViews correspond to "RWTexture" objects in HLSL. They correspond to "uniform image" types in GLSL (image2D, image2DArray, imageCube, etc.).

### Sampler

A [Sampler](xref:Veldrid.Sampler) is a resource which controls how TextureViews are sampled. See [SamplerDescription](xref:Veldrid.SamplerDescription) for the set of properties governing their behavior.

Several shared Samplers are available as properties on [GraphicsDevice](xref:Veldrid.GraphicsDevice). These can be used when a common type of Sampler is needed and you don't want to manage the lifetime of a shared Sampler.

There is an important caveat regarding OpenGL support for Sampler objects and how they can be bound to a Pipeline. Before Vulkan, GLSL did not allow Sampler object state to be separated from Textures. GLSL "sampler" objects encapsulate both, and the GL objects must be bound to a shared set of texture units. TextureViews and Samplers are separated in Veldrid, and these objects must be bound separately to a Pipeline. This means it is not possible to represent Veldrid's abstraction fully in the OpenGL backend: when a Sampler object appears in a ResourceLayout list, it applies to all of the TextureView objects before it (until the previous Sampler in the list). This means that if you need to sample the same TextureView with two different Samplers, then you need to declare two TextureViews with two Samplers in your ResourceLayout.

## Mapping HLSL/GLSL resources to ResourceLayouts

The layout system is convention-based, and relies on shader code being authored in a particular way for resource slots to match.

* Vulkan: ResourceLayouts match very closely with regular Vulkan conventions. In a Vulkan shader, a uniform's "set" layout specifies the ResourceLayout index in the overall [ResourceLayouts](xref:Veldrid.GraphicsPipelineDescription#Veldrid_GraphicsPipelineDescription_ResourceLayouts) array. The "binding" layout specifies the specific resource within that ResourceLayout identified by the "set". For example:
```
layout(set = 0, binding = 1) uniform View
```
defines a uniform belonging to binding 1 (of the [ResourceLayoutElementDescription](xref:Veldrid.ResourceLayoutDescription#Veldrid_ResourceLayoutDescription_Elements) array), of set 0 (of the [ResourceLayouts](xref:Veldrid.GraphicsPipelineDescription#Veldrid_GraphicsPipelineDescription_ResourceLayouts) array).

* Direct3D 11: Resources are assigned HLSL registers based on their positions in the [ResourceLayouts](xref:Veldrid.GraphicsPipelineDescription#Veldrid_GraphicsPipelineDescription_ResourceLayouts) array first, and then by their position in the [ResourceLayoutElementDescriptions](xref:Veldrid.ResourceLayoutDescription#Veldrid_ResourceLayoutDescription_Elements) array. Each resource type (texture, sampler, uniform) is assigned an increasing integer value for its register number. For example, given these two ResourceLayouts:

    **Layout 0**
    | Element | Type | Name |
    |------ |------|------|
    | 0 | UniformBuffer | UB0 |
    | 1 | TextureView | Tex0 |
    | 2 | Sampler | Sampler0 |
    | 3 | Sampler | Sampler1 |
    | 4 | TextureView | Tex1 |
    | 5 | UniformBuffer | UB3 |

    **Layout 1**
    | Element | Type | Name |
    |------ |------|------|
    | 0 | TextureView | Tex2 |
    | 1 | UniformBuffer | UB1 |
    | 2 | UniformBuffer | UB2 |

    the HLSL resources must be specified as follows:
    ```
    cbuffer UB0 : register(b0) { ... }
    cbuffer UB3 : register(b1) { ... }
    cbuffer UB1 : register(b2) { ... }
    cbuffer UB2 : register(b3) { ... }
    Texture2D Tex0 : register(t0);
    Texture2D Tex1 : register(t1);
    Texture2D Tex2 : register(t2);
    SamplerState Sampler0 : register(s0);
    SamplerState Sampler1 : register(s1);
    ```
    (the declaration order is unimportant -- only the register indices matter).

* OpenGL: Resources are matched strictly by-name. Each resource must correspond to a uniform or uniform block in the shader program, and the names must be identical. Numerical indices are ignored when matching resources to GLSL uniforms.