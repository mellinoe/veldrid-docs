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

Shader objects have a unique relationship with [ResourceLayouts](xref:Veldrid.ResourceLayout) and [ResourceSets](xref:Veldrid.ResourceSet). When creating a Pipeline, the provided [ResourceLayouts](xref:Veldrid.PipelineDescription#Veldrid_PipelineDescription_ResourceLayouts) must match the actual resource types that are specified in the shader code.

ResourceLayouts merely define the layout of resources expected by a set of shaders. When draw commands are executed, the actual shader resources (Buffers, TextureViews, and Samplers) are determined based on the CommandList's currently-bound [ResourceSets](xref:Veldrid.ResourceSet).

ResourceLayouts themselves contain a set of one-or-more resource elements. Additionally, multiple ResourceLayout objects can be used to define the inputs of a shader set. When this is done, each ResourceLayout must be matched by a corresponding ResourceSet containing the appropriate resource types. ResourceSet objects are bound to a particular slot. The slot corresponds to the index of the matching ResourceLayout given in the [ResourceLayouts](xref:Veldrid.PipelineDescription#Veldrid_PipelineDescription_ResourceLayouts) field of the PipelineDescription.

Generally, it is advisable to group resources into sets and layouts which are common or shared. For example, it is a good idea to group the camera's view and projection matrix, and other scene-level information, into a single ResourceLayout and shared ResourceSet. This allows many objects to be rendered using the same bound ResourceSet. Specific object Pipelines can utilize extra ResourceLayouts and ResourceSets to accomodate their specific rendering requirements while still utilizing shared resources when it makes sense. Changing ResourceSets can be a costly operation, so re-using them as much as possible can help performance.

## Mapping HLSL/GLSL resources to ResourceLayouts

The layout system is convention-based, and relies on shader code being authored in a particular way for resource slots to match.

* Vulkan: ResourceLayouts match very closely with regular Vulkan conventions. In a Vulkan shader, a uniform's "set" layout specifies the ResourceLayout index in the overall [ResourceLayouts](xref:Veldrid.PipelineDescription#Veldrid_PipelineDescription_ResourceLayouts) array. The "binding" layout specifies the specific resource within that ResourceLayout identified by the "set". For example:
```
layout(set = 0, binding = 1) uniform View
```
defines a uniform belonging to binding 1 (of the [ResourceLayoutElementDescription](xref:Veldrid.ResourceLayoutDescription#Veldrid_ResourceLayoutDescription_Elements) array), of set 0 (of the [ResourceLayouts](xref:Veldrid.PipelineDescription#Veldrid_PipelineDescription_ResourceLayouts) array).

* Direct3D 11: Resources are assigned HLSL registers based on their positions in the [ResourceLayouts](xref:Veldrid.PipelineDescription#Veldrid_PipelineDescription_ResourceLayouts) array first, and then by their position in the [ResourceLayoutElementDescriptions](xref:Veldrid.ResourceLayoutDescription#Veldrid_ResourceLayoutDescription_Elements) array. Each resource type (texture, sampler, uniform) is assigned an increasing integer value for its register number. For example, given these two ResourceLayouts:
```
// Layout 0
[0] UniformBuffer UB0
[1] TextureView Tex0
[2] Sampler Sampler0
[3] Sampler Sampler1
[4] TextureView Tex1
[5] UniformBuffer UB3
// Layout 1
[0] TextureView Tex2
[1] UniformBuffer UB1
[2] UniformBuffer UB2
```
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