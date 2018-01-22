# Veldrid Stable Release

Today, the new version of Veldrid is stable and available for use. There are a small number of features and bug fixes incorporated after the beta 2 release. Notably, there is now support for the Metal graphics API, which adds a new high-performance, high-fidelity option for Apple systems.

## Features

* Add a new Metal backend (GraphicsBackend.Metal). [bbb7722](https://github.com/mellinoe/veldrid/commit/bbb7722d6b85557cb54e2f0142ce091311d28c89)
* Add a new function, `GraphicsDevice.IsBackendSupported(GraphicsBackend)`, which can be used to ask ahead of time whether a particular GraphicsBackend is actually supported by the current system. For most backends, this is a simple platform check (e.g., "Am I on Windows? Am I on macOS?"), but the Vulkan backend does more extensive checks to determine whether the system supports it. [0a93729](https://github.com/mellinoe/veldrid/commit/0a93729e7e93eaec3d2fcbaedeb5fcc86a9437d8)
* Add a uint indexer to MappedResourceView. [f40ee62](https://github.com/mellinoe/veldrid/commit/f40ee62538eeeb610cf88f76e25501ec264683bb)
* Added a couple of helper overloads in VeldridStartup. [7a3e192](https://github.com/mellinoe/veldrid/commit/7a3e1923cdb14a373b5551b6d4f791fe0da66fd7)

## Breaking Changes from Beta 1

* The VertexElementFormat enumeration has undergone some final cleanup. The floating-point values are unchanged (`Float1/2/3/4`), but the integer formats have all been normalized and filled out, so some have been renamed. The integer values are now all of this standard format:

    \<Type\>\<Count\>[\_Norm].

  * Type: The type of each element.
  * Count: The number of elements
  * [\_Norm]: An optional suffix which indicates whether the vertex element is actually a 0->1 value normalized into the range of the integer. For example, `UShort4_Norm` represents a vertex element with four ushorts (UInt16), where the values represent a normalized 0->1 value. `UShort4` represents the same, but the values are not normalized -- they are simply interpreted as unsigned integers.

* When creating a Compute Pipeline, you must now specify the threadgroup size up front. This is in support of the Metal backend.

## Bug Fixes

* Fix support for CursorVisible.get/set in SDL2. [e95f98f](https://github.com/mellinoe/veldrid/commit/e95f98fba68b3927a0d8f1b8e98ab90ce743636f)
* Validate that dynamic DeviceBuffers cannot use MapMode.Read. [e7f3121](https://github.com/mellinoe/veldrid/commit/e7f31218baa1248aa78611827e67dfae1fb41fb9)
* [Vulkan] Fix some issues with framebuffer attachment image layouts and transitions. [75e2eae](https://github.com/mellinoe/veldrid/commit/75e2eae7d9133e7f9f314cce017a760eff7dc5d6)
* [D3D11] Fix issue with using CommandList.UpdateBuffer on a staging buffer. [698f791](https://github.com/mellinoe/veldrid/commit/698f79134d91678cbb446816c2faadf2b0cfa7f3)
* Fix copying non-square Textures. [f993db3](https://github.com/mellinoe/veldrid/commit/f993db3f0b84314828c5e9a88764749991a015d9)
* Fix setting the window's position with SDL2. [b6064ef](https://github.com/mellinoe/veldrid/commit/b6064ef01b9d392f2e063c0da3e5cfb009c294c5)
* Fix some issues with updating and copying odd-sized BC3 textures. [b5e1cac](https://github.com/mellinoe/veldrid/commit/b5e1cace2da136f5ae28dec8ee95279496b61434) [d70a3f6](https://github.com/mellinoe/veldrid/commit/d70a3f6a1380b57b1cd886cf3aea7c67803a187a)
* [OpenGL] Make sure ManualResetEventSlim objects are disposed after use. [2a9502e](https://github.com/mellinoe/veldrid/commit/2a9502e8188f682a80c3b02b39cfbf30e5e94762)
* GraphicsDevice.Dispose now calls WaitForIdle before disposing owned resources. [52e499a](https://github.com/mellinoe/veldrid/commit/52e499aeb91183371179fe3cd314cc41ef301f53)