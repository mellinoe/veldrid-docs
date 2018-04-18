# Android Support

Veldrid supports the Android platform through the Vulkan and OpenGL ES backends. A small amount of setup code is needed to bootstrap your Xamarin application and allow Veldrid to render into your application view.

## Setting up a GraphicsDevice on Android

There are a few simple steps you should follow to get up and running on Android.

1. Create a "SurfaceView" subclass. Veldrid works primarily with Android "Surface" objects. To obtain one of those, you need to inherit from `Android.Views.SurfaceView` and implement the `Android.Views.ISurfaceHolderCallback` interface. This will give you a notification when the Android Surface is actually ready to use (`void SurfaceCreated(ISurfaceHolder holder)`).

2. Create an Android [SwapchainSource](xref:Veldrid.SwapchainSource). Inside of the "SurfaceCreated" callback, you should call [SwapchainSource.CreateAndroidSurface](xref:Veldrid.SwapchainSource#Veldrid_SwapchainSource_CreateAndroidSurface_IntPtr_IntPtr_). This will allow you to create an appropriate [SwapchainDescription](xref:Veldrid.SwapchainDescription) describing a Swapchain that renders into your Android view.

3. Create a GraphicsDevice. Depending on what your device supports, you can either use Vulkan or OpenGL ES. Vulkan support is still rare on current Android devices, but is becoming more common. The majority of devices support OpenGL ES 3.0+. With your SwapchainDescription from the last step, use one of the following methods to create a GraphicsDevice:

* [GraphicsDevice.CreateOpenGLES](xref:Veldrid.GraphicsDevice#Veldrid_GraphicsDevice_CreateOpenGLES_Veldrid_GraphicsDeviceOptions_Veldrid_SwapchainDescription_)
* [GraphicsDevice.CreateVulkan](xref:Veldrid.GraphicsDevice#Veldrid_GraphicsDevice_CreateVulkan_Veldrid_GraphicsDeviceOptions_Veldrid_SwapchainDescription_)

Support for Vulkan can be queried using [GraphicsDevice.IsBackendSupported](xref:Veldrid.GraphicsDevice#Veldrid_GraphicsDevice_IsBackendSupported_Veldrid_GraphicsBackend_).

## Other Android Events

You should respond to other application events that relate to the Surface's lifecycle.

* When your Surface is "changed" (`void SurfaceChanged(ISurfaceHolder holder, Format format, int width, int height)`), you need to [resize your Veldrid Swapchain](xref:Veldrid.Swapchain#Veldrid_Swapchain_Resize_System_UInt32_System_UInt32_) so that its dimensions match the Android Surface's. This event will commonly be called when the device orientation changes, or when a layout change invalidates the view's size.

* When your Surface is destroyed (`void SurfaceDestroyed(ISurfaceHolder holder)`), it is no longer valid to use as a render target. You should detect this and take the appropriate action.
