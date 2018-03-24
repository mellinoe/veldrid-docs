# Veldrid 4.1.4

Veldrid version 4.1.4 is available. This is a small patch with some minor fixes and improvements.

## All Changes

**Veldrid**

* [Vulkan] Fix GraphicsDevice.UpdateBuffer with an offset. [[1018318]](https://github.com/mellinoe/veldrid/commit/1018318e65004ea2b23e3bcb3c2a53ab14b8fe70)
* [D3D11] Fix some edge case handling for some types of D3D11 buffer updates. [[09ac9e8]](https://github.com/mellinoe/veldrid/commit/09ac9e8652d03b068fad7994e8c7ed854b4243c4)
* [Vulkan] Cache the value indicating whether Vulkan is supported by the current system. [[0058c68]](https://github.com/mellinoe/veldrid/commit/0058c687435c06c54f2fab121222770705f9b08a)
* [Metal] Implement a real support check for Metal. [[8a7ccd3]](https://github.com/mellinoe/veldrid/commit/8a7ccd3019fa2ed27b0c2fa1e5dad33462e0202a)
* [Metal] Only use multiple viewports if it's supported in the current device. [[c0c7af0]](https://github.com/mellinoe/veldrid/commit/c0c7af0852835bd5780823c142f070e3896b2877)
* [Metal] Detect when an MTLShader fails to locate the entry point. [[60f97bb]](https://github.com/mellinoe/veldrid/commit/60f97bb59adc7f97dc6f2bad875ccd7a1e4bdd93)
* [Metal] Don't forget the bound ResourceSets when a new Pipeline is set. [[6b9692e]](https://github.com/mellinoe/veldrid/commit/6b9692e3117d8ce4466cabb8cba0a913de4eb296)
* [OpenGL] Don't forget the bound ResourceSets when a new Pipeline is set. [[05f110b]](https://github.com/mellinoe/veldrid/commit/05f110b996d231ada3cc548ee080a650a64bf66a)

**Veldrid.StartupUtilities**

* Request OpenGL version 3.0 instead of 4.0. [[fa044e2]](https://github.com/mellinoe/veldrid/commit/fa044e2fdac2fbc3a9a9d38545f354b2414ebe5d)
  * This version is a "minimum" version -- the driver will generally still return a higher version context, if supported.
  * This allows `VeldridStartup.CreateGraphicsDevice` to be used on machines supporting only OpenGL 3.

**Veldrid.SDL2**
* Refresh the cached size value when a new Width/Height is set. [[54eb5bc]](https://github.com/mellinoe/veldrid/commit/54eb5bc4791354fcf24e860d9459c44cea0fa14b)