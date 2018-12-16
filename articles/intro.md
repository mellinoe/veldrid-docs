---
uid: intro
---

# Introduction

Veldrid is a low-level graphics library for .NET. It can be used to create high-performance 2D and 3D games, simulations, tools, and other graphical applications. Unlike most other .NET graphics libraries, Veldrid is designed to be portable, meaning it is not tied to any particular operating system or native graphics API. With Vulkan, Direct3D, Metal, OpenGL, and OpenGL ES backends, applications built with Veldrid can run on all desktop and mobile platforms without modification.

# Features

* Modern GPU functionality:
  * Programmable vertex, geometry, tessellation, and fragment shaders
  * Programmable compute shaders
  * Full control over the graphics pipeline: blend state, depth-stencil state, rasterizer state, shaders and shader specialization, resource access, and more
  * Modern buffer features, including vertex, index, uniform, structured, and indirect buffers, read-write shader access, and more
  * Modern [texture](xref:textures) features, including 3D textures, array textures, multisampling, texture views, multi-output [Framebuffers](xref:framebuffers), read-write shader access, a variety of standard, compressed and packed [pixel formats](xref:Veldrid.PixelFormat), and more
  * Multiple [Swapchains](xref:swapchains), with support for runtime creation and destruction
  * Much more
* Targets modern graphics APIs, including Vulkan and Metal.
* Cross-platform: Windows, macOS, Linux, Android, iOS.
* Portable Shaders: First-class support for SPIR-V bytecode allows you to write your shaders once for all platforms.
* Virtual Reality: Supports both OpenVR and Oculus using a simple, portable API.
* High-performance: Built on a thin, low-cost abstraction that is close to current-gen graphics API's.
* Multi-threaded: Most Veldrid objects can be used from multiple threads simultaneously, allowing you to split your work as you see fit. See [Multi-threading in Veldrid](xref:multi-threading) for specifics.
* Allocation-free: The core rendering loop (outside of resource creation and initialization) can be used without allocating any garbage-collected memory. Avoiding allocations is important for high-performance, low-latency rendering.
* Headless: Can be used without any application window or view, to perform GPU-accelerated operations in the background.
* .NET-friendly: Unlike native graphics API's, Veldrid was designed with .NET in mind, and integrates cleanly with regular .NET code.

# Getting Started

See [Getting Started](xref:getting-started-intro) for a basic startup guide.

# API Concepts

See [API Concepts](xref:api-concepts) for an overview of the common object types in Veldrid.

# Samples

See the [Veldrid Samples](https://github.com/mellinoe/veldrid-samples) repository for a growing set of demo applications built using Veldrid.

Additionally, [NeoDemo](https://github.com/mellinoe/veldrid/tree/master/src/NeoDemo) is a sample application being built in tandem with Veldrid, and which uses a large chunk of the available features.

# NuGet Packages

Pre-release packages are available from MyGet: https://www.myget.org/feed/mellinoe/package/nuget/Veldrid.