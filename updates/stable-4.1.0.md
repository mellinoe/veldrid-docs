# Veldrid 4.1.0

Veldrid version 4.1.0 is available. This is an incremental update with many new features and bug fixes. There are no breaking changes introduced in this version.

## New Features

### Swapchains

A [Swapchain](xref:Veldrid.Swapchain) is a new type in the library, exposing functionality that was previously implicit. A Swapchain is an object representing an application surface or view that can be drawn on and presented to the user. It is now possible to create and destroy multiple Swapchains as needed during runtime. Additionally, it is now possible to create a `GraphicsDevice` which has no Swapchain at all, in case you are developing a "headless" application. This can be useful if you want to use Veldrid only for background compute or image processing operations. See [Swapchains](xref:swapchains) for more information.

### More PixelFormats

A large number of additional PixelFormats have been added. Veldrid now exposes practically every permutation of standard RGBA integer and floating-point formats, as well as a large number of specialized compressed and packed formats. See the full changelist below or the [PixelFormat](xref:Veldrid.PixelFormat) documentation page for more information.

### iOS Support

It's now possible to use Veldrid on iOS with the Metal backend. A [UIView](https://developer.apple.com/documentation/uikit/uiview) can be used to create a Swapchain to present rendered images to the application.

### Optional backends

For users compiling Veldrid from source, it's now possible to exclude the implementations for unused backends. To do this, set `ExcludeXYZ=true` in your project, where `XYZ` is a specific kind of backend (D3D11, OpenGL, Vulkan, or Metal).

## All Changes

**Veldrid**
* Add a description of what ShaderDescription.ShaderBytes must contain for Metal shaders. [@tgjones](https://github.com/tgjones) [[492c688]](https://github.com/mellinoe/veldrid/commit/492c6880ed6e058352570db1301fe71797cd4a79)
* Implement a large number of new PixelFormat types. Big thanks to [@tgjones](https://github.com/tgjones) for his contributions in this space. [[764e9de]](https://github.com/mellinoe/veldrid/commit/764e9de268600c08350939e473b417efd5795016) [[380a620]](https://github.com/mellinoe/veldrid/commit/380a62062228458e36068249c206e8c80b9f5194) [[98e0cee]](https://github.com/mellinoe/veldrid/commit/98e0ceec11d103bd5ff514d7289868de616fc99a)
* Implement some additional helper state descriptions. [@tgjones](https://github.com/tgjones). [[f04cd79]](https://github.com/mellinoe/veldrid/commit/f04cd79e4a6339b4cb22405eaf3fc83735d1b227)
* Add several more static colors to RgbaByte and RgbaFloat. [[4808a22]](https://github.com/mellinoe/veldrid/commit/4808a22c476d39927573fe3697ff4f191532fd64) [[2ae5d95]](https://github.com/mellinoe/veldrid/commit/2ae5d95c1f95c8139e646e66f3dda7e89c72cf2c)
* Fix mapping of DXGI_D32_Float format. [[1c54d15]](https://github.com/mellinoe/veldrid/commit/1c54d15be7cc868e22163eb2c50aca2c1c3f44cd)
* Improve how Vulkan caches and disposes some of its staging resources. [[56322d3]](https://github.com/mellinoe/veldrid/commit/56322d314da24a6bc684295105bdacff7b81ed8a)
* Fix some issues with compressed texture copying and optimize many texture copy code paths. [eecbea1](https://github.com/mellinoe/veldrid/commit/eecbea1860e10e4cf5bca5f102cd517fc86292db) [[d3f2cd5]](https://github.com/mellinoe/veldrid/commit/d3f2cd5ea38bbafccd6c4aa097c3813747032870)
* Lazily set vertex buffers in MTLCommandList, during PreDrawCommand. [[86687d1]](https://github.com/mellinoe/veldrid/commit/86687d1343e8f17300805b6852705a2422ec083b)
* In the OpenGL backend, allow items specified in ResourceLayouts to be missing This matches the behavior of other backends. [[576df20]](https://github.com/mellinoe/veldrid/commit/576df20a8c1d5c81432e1e9002492722096b6351)
* Fix structured buffer creation in D3D11 backend. [@tgjones](https://github.com/tgjones) [[3abe081]](https://github.com/mellinoe/veldrid/commit/3abe0819d5d7f0f5c75e6ba9445862bff0e93980)
* Fix some issues with how render pass loading and clearing is handled in the Vulkan backend. [[b49a165]](https://github.com/mellinoe/veldrid/commit/b49a165a9247f4cbb62264e627e320ef6dae478c)
* Fix OpenGL's viewport translation. [[d00f6bc]](https://github.com/mellinoe/veldrid/commit/d00f6bcdeace3c1643296f0fff0680570ced6fb4)
* Fix OpenGL's assignment of UBO and SSBO binding locations when multiple ResourceSets are used. [[b05fd4f]](https://github.com/mellinoe/veldrid/commit/b05fd4fbae4fa00604de60ac852990f958beff99)
* Fix some missing resource disposals in the D3D11 backend. [[9dfc41a]](https://github.com/mellinoe/veldrid/commit/9dfc41af8e24918919dce55152ea2d080f98f71b)
* Fix the `VALIDATE_USAGE` define constant. [[9807394]](https://github.com/mellinoe/veldrid/commit/9807394ddd540b100498006e6f1c4fbc442b0d76) [[a6a8464]](https://github.com/mellinoe/veldrid/commit/a6a8464b90170f8a489da87e286ba0cabb4c12eb)
* Allow conditional exclusion of graphics backends when compiling from source. Thanks to [@Krakean](https://github.com/Krakean) for his contribution. [[0247576]](https://github.com/mellinoe/veldrid/commit/0247576114d39330ade04a6cedc7c6aa06abde77) [[f9825ca]](https://github.com/mellinoe/veldrid/commit/f9825cad90504d56dddda156018c8ccc28c5fc39)
* Fix resource validation for Texture sample counts. [[c04c48b]](https://github.com/mellinoe/veldrid/commit/c04c48bea638180a75f4f5cef87d1a6a7990c926)
* In the Direct3D 11 backend, use a different MapSubresource overload to avoid an unnecessary DataStream allocation. [[3a9c486]](https://github.com/mellinoe/veldrid/commit/3a9c48614b9e73210e62bf04ef812e82637fddf8)
* Add a new SwapchainType and the ability to create it. [[dd64f0f]](https://github.com/mellinoe/veldrid/commit/dd64f0f3d6be9853007f1e90a056848f8e5b940f)
* Add support for iOS through a UIView SwapchainSource. [[5e63b39]](https://github.com/mellinoe/veldrid/commit/5e63b39a49a674f7b1e650724e038443f3b6a7f8)
* Fix Metal's handling of PolygonFillMode.Wireframe. [[7e505de]](https://github.com/mellinoe/veldrid/commit/7e505def90170766dbd9d4f8d7b91363fab840fa)

**Veldrid.SDL2**
* Update the bundled SDL2 binaries for Windows and macOS to version 2.0.7. [[e0027af]](https://github.com/mellinoe/veldrid/commit/e0027afc220b23fe7877c8e0c77190f6d4056ff2) [[4428e3d]](https://github.com/mellinoe/veldrid/commit/4428e3d3d8d7c9ddae8ab43608791bf132b5c41d)
* Add Sdl2Window constructor that allows creation from an existing window handle [@tgjones](https://github.com/tgjones) [[4968827]](https://github.com/mellinoe/veldrid/commit/4968827665c0ddb9253cb141c6ebed8a32315c1a)
* Add support for custom handling of SDL2 events, bypassing the regular event processing callbacks. [[c1263c9]](https://github.com/mellinoe/veldrid/commit/c1263c901c6c2e341851269d4d896255064fc3e7)
* Add the ability to get a SwapchainSource from an Sdl2Window., and fix event processing when multiple SDL2 windows are simultaneously in use. [[dd64f0f]](https://github.com/mellinoe/veldrid/commit/dd64f0f3d6be9853007f1e90a056848f8e5b940f)

**Veldrid.ImGui**
* Fix some ordering issues that prevented mouse wheel input from working. [[fc6be2a]](https://github.com/mellinoe/veldrid/commit/fc6be2a17c6a1e38484e80dbc086c5a581166a3a)
* Add a method to clear cached ImGui image resources. [[337fcdd]](https://github.com/mellinoe/veldrid/commit/337fcdd65d204a66f305419037f403b8a67a2a4e)

**Veldrid.Utilities**
* Fix parsing of OBJ/MTL files when the newline character in the file does not match the system's newline character. [[587fd53]](https://github.com/mellinoe/veldrid/commit/587fd538e4588e223a3cb764c99c41367307747b)
