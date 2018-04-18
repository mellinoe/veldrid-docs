---
uid: swapchains
---

# Swapchains

A Swapchain is a special kind of resource that provides the ability to present rendered images to an application view or surface. If you want the display any rendered images in Veldrid, you need a Swapchain.

## The Main Swapchain

Veldrid has the notion of a "main Swapchain" (see [GraphicsDevice.MainSwapchain](xref:Veldrid.GraphicsDevice#Veldrid_GraphicsDevice_MainSwapchain)), which is the Swapchain created along with the GraphicsDevice itself. The main Swapchain is only created when certain GraphicsDevice.Create overloads are used:

* Overloads accepting a `SwapchainDescription`
* Overloads accepting platform-specific window information, for example:
  * [GraphicsDevice.CreateD3D11(GraphicsDeviceOptions, IntPtr, UInt32, UInt32)](xref:Veldrid.GraphicsDevice#Veldrid_GraphicsDevice_CreateD3D11_Veldrid_GraphicsDeviceOptions_IntPtr_System_UInt32_System_UInt32_)
  * [GraphicsDevice.CreateVulkan(GraphicsDeviceOptions, VkSurfaceSource, UInt32, UInt32)](xref:Veldrid.GraphicsDevice#Veldrid_GraphicsDevice_CreateVulkan_Veldrid_GraphicsDeviceOptions_Veldrid_Vk_VkSurfaceSource_System_UInt32_System_UInt32_)

Overloads that only accept a `GraphicsDeviceOptions` parameter do NOT create a main Swapchain. When no main Swapchain has been created, it is not valid to use the GraphicsDevice.MainSwapchain property, or to call the methods which implicitly operate on the main Swapchain:

* [GraphicsDevice.SwapBuffers()](xref:Veldrid.GraphicsDevice#Veldrid_GraphicsDevice_SwapBuffers) _Parameter-less overload only_
* [GraphicsDevice.ResizeMainWindow(UInt32, UInt32)](xref:Veldrid.GraphicsDevice#Veldrid_GraphicsDevice_ResizeMainWindow_System_UInt32_System_UInt32_)
* [GraphicsDevice.SwapchainFramebuffer](xref:Veldrid.GraphicsDevice#Veldrid_GraphicsDevice_SwapchainFramebuffer)
* [GraphicsDevice.SyncToVerticalBlank](xref:Veldrid.GraphicsDevice#Veldrid_GraphicsDevice_SyncToVerticalBlank)

NOTE: Calling any of the above members is equivalent to accessing the same member on [GraphicsDevice.MainSwapchain](xref:Veldrid.GraphicsDevice#Veldrid_GraphicsDevice_MainSwapchain). There is no functional difference.

## Creating Swapchains

Like other resources, it is possible to create and destroy extra Swapchains at runtime, using a [ResourceFactory](xref:Veldrid.ResourceFactory) and a [SwapchainDescription](xref:Veldrid.SwapchainDescription). Constructing a SwapchainDescription requires a [SwapchainSource](xref:Veldrid.SwapchainSource), which is a special kind of Veldrid object representing a renderable surface that the application controls. Since this component interacts with the OS and its windowing system, it is platform-specific and created in a different way to other Veldrid objects. There are a variety of factory methods which allow you to create a SwapchainSource for a particular kind of operating system and UI framework. Due to platform limitations, each kind of SwapchainSource can only be used to create a subset of GraphicsDevice types.

* [SwapchainSource.CreateWin32(IntPtr hwnd, IntPtr hinstance)](xref:Veldrid.SwapchainSource#Veldrid_SwapchainSource_CreateWin32_IntPtr_IntPtr_): D3D11, Vulkan, OpenGL, OpenGL ES
* [SwapchainSource.CreateUwp(object swapChainPanel, float logicalDpi)](xref:Veldrid.SwapchainSource#Veldrid_SwapchainSource_CreateUwp_System_Object_System_Single_): D3D11
* [SwapchainSource.CreateXlib(IntPtr display, IntPtr window)](xref:Veldrid.SwapchainSource#Veldrid_SwapchainSource_CreateXlib_IntPtr_IntPtr_): Vulkan, OpenGL, OpenGL ES
* [SwapchainSource.CreateNSWindow(IntPtr nsWindow)](xref:Veldrid.SwapchainSource#Veldrid_SwapchainSource_CreateNSWindow_IntPtr_): Metal
* [SwapchainSource.CreateUIView(IntPtr uiView)](xref:Veldrid.SwapchainSource#Veldrid_SwapchainSource_CreateUIView_IntPtr_): Metal, OpenGL ES
* [SwapchainSource.CreateAndroidSurface(IntPtr surfaceHandle, IntPtr jniEnv)](xref:Veldrid.SwapchainSource#Veldrid_SwapchainSource_CreateAndroidSurface_IntPtr_IntPtr_): Vulkan, OpenGL ES

## Using a Swapchain

There are three main operations on a Swapchain: Rendering to it, presenting it, and resizing it.

Rendering to a Swapchain is accomplished by using its [Framebuffer](xref:Veldrid.Swapchain#Veldrid_Swapchain_Framebuffer) property. Bind this object using a [CommandList](xref:Veldrid.CommandList), and then issue draw commands. This Framebuffer is the same as any other Framebuffer in Veldrid.

Presenting a Swapchain is accomplished by calling [GraphicDevice.SwapBuffers(Swapchain)](xref:Veldrid.GraphicsDevice#Veldrid_GraphicsDevice_SwapBuffers_Veldrid_Swapchain_). This should be called after the relevant drawing commands have been submitted.

Resizing a Swapchain is accomplished by calling [Resize(UInt32, UInt32)](xref:Veldrid.Swapchain#Veldrid_Swapchain_Resize_System_UInt32_System_UInt32_). This should generally be done as a response to a "Resized" event issued by whatever windowing or UI framework you are using. If the Swapchain is not resized when the application view changes, then your presented image will appear distorted.

## OpenGL

The OpenGL backend does not support multiple Swapchains. This is due to fundamental part of the design of OpenGL, and support cannot be synthesized or emulated efficently. Note that it is also not possible to create an OpenGL GraphicsDevice without a main Swapchain.