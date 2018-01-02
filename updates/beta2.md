---
uid: beta2
---

# Veldrid Beta 2

_2018-1-1_

Beta 2 is now available. This version includes a large number of fixes, optimizations, and improvements. This is the last planned beta before a stable release. There are a small number of minor breaking changes from the previous beta, all of which are listed below. NuGet packages are available on [NuGet.org](https://www.nuget.org/packages/Veldrid/).

## Features

* Allow a D3D11 GraphicsDevice to be created from a UWP SwapChainPanel. [[5118f26]](https://github.com/mellinoe/veldrid/commit/5118f26415e6fc0bd87ed3ef4c36429c22ba609a)
* Add a new type, "Fence", which allows you to wait on the completion of a particular CommandList submission. [[eb8b2df]](https://github.com/mellinoe/veldrid/commit/eb8b2df3f95558e31368fcbafca29149817d69d7).
* Add two convenience overloads for CommandList.CopyTexture. [[482501b]](https://github.com/mellinoe/veldrid/commit/482501bac1a150223b3a3b6c056cff5d7eaeb84a)
* Added descriptions to NuGet packages. [[592528b]](https://github.com/mellinoe/veldrid/commit/592528be423671ef9495b50cc05453e3d4a4022f)
* Various performance optimizations and allocation improvements.
* Improved early usage validation and error messages in many cases. [[bac67e3]](https://github.com/mellinoe/veldrid/commit/bac67e39d60096fa45deba3b7f98a922f2f0adcc), [[0d128f4]](https://github.com/mellinoe/veldrid/commit/0d128f4dc3e1dffa6be10d0874d3649acceb5c9a), [[18d8efb]](https://github.com/mellinoe/veldrid/commit/18d8efbaede3e43e7e8018a0d62e99443668992c), [[fe8a60d]](https://github.com/mellinoe/veldrid/commit/fe8a60d18087818fe94e580398de6e038adc929f), [[1e31065]](https://github.com/mellinoe/veldrid/commit/1e31065874c1f71002a3452616868af691a2aab5), [[8dd4005]](https://github.com/mellinoe/veldrid/commit/8dd4005f459449158ab723c2ac1b582925a174e0)

## Breaking Changes from Beta 1

* Veldrid.Buffer has been renamed to Veldrid.DeviceBuffer to avoid name collisions with System.Buffer. [[2080e66]](https://github.com/mellinoe/veldrid/commit/2080e66a7c094ab9b07349a84ae68f0f64fe8128)
* Rename GraphicsDevice.ExecuteCommands() to GraphicsDevice.SubmitCommands(). There is no functional change, but the new name better reflects the fact that the work is only being submitted, and may not complete before the method returns.
* Add a TextureType enumeration, and add a TextureType parameter to each TextureDescription constructor. This value is required.
  * TextureDescription now includes a few helper methods which allow you to easily create a description for a particular type of Texture (see Texture1D(), Texture2D(), and Texture3D()).
* GraphicsBackend.OpenGLES has been removed. When the backend is truly supported, the enumeration value will be added back. [[fa89504]](https://github.com/mellinoe/veldrid/commit/fa895045e2f9948b29c0df980c64d5833a1d85af)
* Some elements of the VertexElementFormat enumeration have been cleaned up and made more consistent. These are the 8-bit UNorm and UInt variants. [[632fdab]](https://github.com/mellinoe/veldrid/commit/632fdabe6f7170eb423dac6b5c6ec12fcac4356c)
  * Byte1 has been removed.
  * Byte2 has been split into Byte2_UNorm and Byte2_UInt.
  * Byte4 has been split into Byte4_UNorm and Byte4_UInt.
* The InstanceStepRate property of vertex layouts has been moved onto the VertexLayoutDescription structure, rather than on individual elements as it was before. It wasn't actually possible to specify a different step rate for each element, so the previous behavior was unnecessarily confusing. [[e54a795]](https://github.com/mellinoe/veldrid/commit/e54a79589cd1660d6b502f719af77ac571b72d56)

## Bug Fixes

* On Linux, attempt to load SDL2 from multiple different names. [[ece7f27]](https://github.com/mellinoe/veldrid/commit/ece7f27f175c130c825ef28e7bd4b7237831c94f) [[Issue #18]](https://github.com/mellinoe/veldrid/issues/18)
* Fix an SDL2 interop issue when running on 32-bit Windows. [[444d329]](https://github.com/mellinoe/veldrid/commit/444d3295bf97346ffafdbdd0960c96fdce91d0a8) [[Issue #17]](https://github.com/mellinoe/veldrid/issues/17)
* Fixed an issue with how OpenGL was representing its Swapchain Textures. [[46d2795]](https://github.com/mellinoe/veldrid/commit/46d279512b992307b41365b126a841c478596f79)
* Give an appropriate error message when OpenGL shader compilation fails on macOS. [[c828007]](https://github.com/mellinoe/veldrid/commit/c8280070ab688365255e4d7c91b709b1b87af43b)
* Fix a timing issue related to how/when some Vulkan objects were being disposed [[de01cfc]](https://github.com/mellinoe/veldrid/commit/de01cfc3564fac0e3fddfcd3809a369a91cb3ace)
* Fix an issue where D3D11 structured buffers had a hardcoded size. [[f1750bb]](https://github.com/mellinoe/veldrid/commit/f1750bb64d1f2642f40b6e06e18f9d23c9d285ba)
* Fix an issue where an OpenGL CommandList could not be submitted more than once before previous submissions fully completed. [[8a29172]](https://github.com/mellinoe/veldrid/commit/8a291727eb041b833f2786cdb1b8841796b3c036)
* Fix issues with Texture->Texture copying on multiple backends. [[19cbd5e]](https://github.com/mellinoe/veldrid/commit/19cbd5e5aa78c1142eac3f30305740cb2477b337), [[d7dec20]](https://github.com/mellinoe/veldrid/commit/d7dec201c64a3cb30f9741091098db78ab5b132f), [[4262c13]](https://github.com/mellinoe/veldrid/commit/4262c13673cb6b9d8a01fe8baab449936e38bc3f), [[e83710d]](https://github.com/mellinoe/veldrid/commit/e83710d2bc513beca8504a5644622306106dc0e4), [[517f6e1]](https://github.com/mellinoe/veldrid/commit/517f6e1033f1937e2a2acdefdd19ba5a888d3f06)
