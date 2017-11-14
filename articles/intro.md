---
uid: intro
---

# Introduction

Veldrid is a low-level graphics library for .NET. It can be used to create high-performance 2D and 3D games, simulations, tools, and other graphical applications. Unlike most other .NET graphics libraries, Veldrid is designed to be portable, meaning it is not tied to any particular operating system or native graphics API. With Direct3D, Vulkan, and OpenGL backends, applications built with Veldrid can run on all desktop and mobile platforms without modification.

A basic understanding of graphics APIs and hardware is helpful for understanding and best utilizing Veldrid, which is designed in the vein current-gen graphics APIs like Vulkan, Direct3D 12, and Metal. As a low-level library, it is powerful and flexible, but verbose -- it is best used as the robust foundation of a high-performance game engine or application framework.

# Features

* Full customization of graphics pipeline state, with support for current-gen features and optimizations.
  * Blend, depth-stencil, and rasterizer states.
  * Programmable vertex, fragment, tessellation, and geometry shaders.
  * Simplified resource layout and binding model.
* Simple, unified resource creation and update mechanisms.
* High-performance: Veldrid is built on a thin, low-cost abstraction that is close to current-gen graphics API's.
* Allocation-less: The core rendering loop (outside of resource creation and initialization) can be used without allocating any garbage-collected memory. If application code follows suit, then completely GC-less rendering can be achieved.
* .NET-friendly: Unlike native graphics API's, Veldrid was designed with .NET in mind, and integrates cleanly with regular .NET code.
* Multi-threaded: Veldrid objects can be used from multiple threads, with some restrictions. See [Multi-threading in Veldrid](xref:multi-threading) for specifics.
* Dependency-less: With the exception of bindings for native graphics APIs, Veldrid is implemented without any third-party dependencies.

# API Concepts

## GraphicsDevice

A [GraphicsDevice](xref:Veldrid.GraphicsDevice) is the entry point into Veldrid. It represents a GPU device capable of creating and managing graphics resources, as well as executing commands. GraphicsDevices are created explicity by the application, and a particular backing API (Vulkan, Direct3D, OpenGL) is selected at that time. The GraphicsDevice is also responsible for presenting rendered images to the application's main swapchain.

## CommandList

A [CommandList](xref:Veldrid.CommandList) is a device resource capable of recording graphics commands, which can then be executed by a GraphicsDevice. All rendering commands, like drawing primitives, updating textures, and clearing framebuffers, are submitted to a CommandList. Optionally, multiple CommandList objects can be used in parallel to record independent graphics commands on separate threads.

## Resources

A variety of device resources can be created by a GraphicsDevice in order to control rendering operations. Veldrid resources are owned by their creator, and they must be disposed explicitly to avoid leaking device memory.

### Textures, Buffers

[Textures](xref:Veldrid.Texture) and [Buffers](xref:Veldrid.Buffer) are staple device resources used to store various kinds of information on the GPU. They can act as the source or destination of data for rendering operations.

Textures can be sampled in shader programs using a [TextureView](xref:Veldrid.TextureView). They can also be used as the destination of drawing operations when used to create a [Framebuffer](xref:Veldrid.Framebuffer) object.

[VertexBuffers](xref:Veldrid.VertexBuffer) and [IndexBuffers](xref:Veldrid.IndexBuffer) are specific kinds of Buffers which are used to store vertex and index information. These are used as a data source for drawing operations.

[UniformBuffers](xref:Veldrid.UniformBuffer) are Buffers which can be read from shader programs. These are commonly used to store object transformations, camera transformations, and other arbitrary pieces of data encoding some information about the scene being rendered.

### Shaders

[Shaders](xref:Veldrid.Shader) are a device resource which represent a single shader module, for a single shader stage (vertex, fragment, tesselation, geometry). They are created from pieces of API-specific data (see [Shaders](xref:shaders) for more information). Multiple shader modules can be combined to produce a set of shaders usable in a Pipeline.

### Pipelines

A [Pipeline](xref:Veldrid.Pipeline) is a device resource which encapsulates a large amount of graphics pipeline information. In other graphics libraries, a Pipeline is split into several unrelated objects or function calls which can be combined in confusing and unpredictable ways. Veldrid pipelines represent a combined set of all of these pieces of information, and are immutable. This avoids a huge amount of complexity related to the mutable state machine paradigm of OpenGL and similar APIs. Pipeline information includes:

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

# Getting Started

See [Getting Started](xref:getting-started) for a basic startup guide.

# Samples

[NeoDemo](https://github.com/mellinoe/veldrid/tree/master/src/NeoDemo) is a sample application being built in tandem with Veldrid.

# NuGet Packages

Pre-release packages are available from MyGet: https://www.myget.org/feed/mellinoe/package/nuget/Veldrid.