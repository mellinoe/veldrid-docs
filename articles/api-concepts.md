---
uid: api-concepts
---

# API Concepts

Veldrid is a flexible library exposing a large number of types granting access to various pieces of graphics functionality. The following are the most important concepts in the API which are used for most common rendering techniques. See the [API Documentation](xref:Veldrid) page for a full listing of available Veldrid types.

## GraphicsDevice

A [GraphicsDevice](xref:Veldrid.GraphicsDevice) is the entry point into Veldrid. It represents a GPU device capable of creating and managing graphics resources, executing commands, and presenting rendered images to the application's main swapchain. GraphicsDevices are created explicity by the application, and a particular backing API (Vulkan, Direct3D, OpenGL) is selected at that time.

## CommandList

A [CommandList](xref:Veldrid.CommandList) is a device resource capable of recording graphics commands, which can then be executed by a GraphicsDevice. All rendering commands, like drawing primitives, updating textures, and clearing framebuffers, are submitted to a CommandList. Optionally, multiple CommandList objects can be used in parallel to record independent graphics commands on separate threads.

See the [CommandList overview](xref:command-lists) for more information.

## Resources

A variety of device resources can be created by a GraphicsDevice in order to control rendering operations. Veldrid resources are owned by their creator, and they must be disposed explicitly to avoid leaking device memory.

### "Descriptions"

Veldrid exposes a number of Description types, which are plain data structures used by a [ResourceFactory](xref:Veldrid.ResourceFactory) in order to create new graphics resources. These are simple, transient objects which contain a small number of fields describing the properties of a single type of resource. For example, an [IndexBufferDescription](xref:Veldrid.IndexBufferDescription) contains the two pieces of information necessary for creating an [IndexBuffer](xref:Veldrid.IndexBuffer): the total capacity and format of the index data to be stored.

All device resources can be uniformly created with an appropriate Description object. Additionally, there are some convenience methods that allow the creation of some resource objects with a small set of common parameters, rather than a full Description object.

### Textures and Buffers

[Textures](xref:Veldrid.Texture) and [Buffers](xref:Veldrid.Buffer) are staple device resources used to store various kinds of information on the GPU. They can act as the source or destination of data for rendering operations.

Textures can be sampled in shader programs using a [TextureView](xref:Veldrid.TextureView). They can also be used as the destination of drawing operations when used to create a [Framebuffer](xref:Veldrid.Framebuffer) object.

[VertexBuffers](xref:Veldrid.VertexBuffer) and [IndexBuffers](xref:Veldrid.IndexBuffer) are specific kinds of Buffers which are used to store vertex and index information. These are used as a data source for drawing operations.

[UniformBuffers](xref:Veldrid.UniformBuffer) are Buffers which can be read from shader programs. These are commonly used to store object transformations, camera transformations, and other arbitrary pieces of data encoding some information about the scene being rendered.

### Shaders

[Shaders](xref:Veldrid.Shader) are a device resource which represent a single shader module, for a single shader stage (vertex, fragment, tesselation, geometry). They are created from graphics-API-specific data chunks. Multiple shader modules are combined into a "shader set", used to construct a Pipeline. See the [Shaders and Resources overview](xref:shaders-and-resources) for more information.

### Pipelines

A [Pipeline](xref:Veldrid.Pipeline) is a device resource which encapsulates a large amount of graphics pipeline information. In other graphics libraries, a Pipeline is split into several unrelated objects or function calls which can be combined in confusing and unpredictable ways. Veldrid pipelines represent the combined set of all of these pieces of information, and are immutable. This avoids a huge amount of complexity inherent in the mutable state machine paradigm of OpenGL and similar APIs. Pipeline information encompasses:

* Blend state: how color values are blended into the Framebuffer.
* Depth stencil state: How depth testing, writing, comparing are performed.
* Rasterizer state: How culling, clipping, scissor tests, etc. are performed.
* Primitive topology: How a series of input vertices are interpreted.
* Shader set: the full set of shader programs in use.
* Resource layout: The types and layout of all shader resources being used.
* Outputs: The depth and color outputs of the Framebuffer that are written to.

### ResourceLayouts and ResourceSets

A [ResourceSet](xref:Veldrid.ResourceSet) is another fundamental device resource which is necessary, along with a VertexBuffer, IndexBuffer, and Pipeline, for all Drawing commands. ResourceSets are the mechanism by which BindableResource objects ([UniformBuffers](xref:Veldrid.UniformBuffer), [TextureViews](xref:Veldrid.TextureView), and [Samplers](xref:Veldrid.Sampler)) are bound to a Pipeline and become accessible to shaders for use when rendering. The types and order of resources is described in a [ResourceLayout](xref:Veldrid.ResourceLayout) object, used to create both a ResourceSet and a Pipeline.

### Framebuffer

A [Framebuffer](xref:Veldrid.Framebuffer) controls the set of textures that are drawn into when rendering commands are executed. The application's swapchain Framebuffer is also accessible via the [GraphicsDevice.SwapchainFramebuffer](xref:Veldrid.GraphicsDevice#Veldrid_GraphicsDevice_SwapchainFramebuffer) property. The swapchain Framebuffer is used to present an image to the application window or view.
