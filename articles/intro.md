---
uid: intro
---

# Introduction

Veldrid is a low-level graphics library for .NET. It can be used to create high-performance 2D and 3D games, simulations, tools, and other graphical applications. Unlike most other .NET graphics libraries, Veldrid is designed to be portable, meaning it is not tied to any particular operating system or native graphics API. With Direct3D, Vulkan, and OpenGL backends, applications built with Veldrid can run on all desktop and mobile platforms without modification.

A basic understanding of graphics APIs and hardware is helpful for understanding and best utilizing Veldrid, which is designed in the vein current-gen graphics APIs like Vulkan, Direct3D 12, and Metal. As a low-level library, it is powerful and flexible, but verbose -- it is best used as the robust foundation of a high-performance game engine or application framework.

# Features

* Full customization of graphics resources and pipeline state, with support for current-gen features and optimizations.
  * Programmable vertex, fragment, tessellation, geometry, and compute shaders.
  * Indirect drawing and compute dispatch.
  * Simplified resource layout and binding model, including support for read-write textures and buffers.
  * Simple, unified resource creation and update mechanisms.
* High-performance: Veldrid is built on a thin, low-cost abstraction that is close to current-gen graphics API's.
* Allocation-less: The core rendering loop (outside of resource creation and initialization) can be used without allocating any garbage-collected memory. If application code follows suit, then completely GC-less rendering can be achieved.
* .NET-friendly: Unlike native graphics API's, Veldrid was designed with .NET in mind, and integrates cleanly with regular .NET code.
* Multi-threaded: Veldrid objects can be used from multiple threads, with some restrictions. See [Multi-threading in Veldrid](xref:multi-threading) for specifics.
* Dependency-less: With the exception of bindings for native graphics APIs, Veldrid is implemented without any third-party dependencies.

# Getting Started

See [Getting Started](xref:getting-started-intro) for a basic startup guide.

# API Concepts

See [API Concepts](xref:api-concepts) for an overview of the common object types in Veldrid.

# Samples

See the [Veldrid Samples](https://github.com/mellinoe/veldrid-samples) repository for a growing set of demo applications build using Veldrid.

Additionally, [NeoDemo](https://github.com/mellinoe/veldrid/tree/master/src/NeoDemo) is a sample application being built in tandem with Veldrid, and which uses a large chunk of the available features.

# NuGet Packages

Pre-release packages are available from MyGet: https://www.myget.org/feed/mellinoe/package/nuget/Veldrid.