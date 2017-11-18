---
uid: textures
---

# Textures

[Textures](xref:Veldrid.Texture) are device resources used as the source and destination of texture information. Textures can be created with a variety of options, controlling how individual instances are permitted to be used.

## Format

Texture data is stored in one of a number of [formats](xref:Veldrid.PixelFormat), specified by the [TextureDescription.Format](xref:Veldrid.TextureDescription#Veldrid_TextureDescription_Format) field.

## Dimensions

Textures can be 1D, 2D, or 3D. This is controlled by [TextureDescription.Width](xref:Veldrid.TextureDescription#Veldrid_TextureDescription_Width), [TextureDescription.Height](xref:Veldrid.TextureDescription#Veldrid_TextureDescription_Height), and [TextureDescription.Depth](xref:Veldrid.TextureDescription#Veldrid_TextureDescription_Depth).

## Array Layers

1D and 2D textures can have multiple array layers. 3D textures (where [TextureDescription.Depth](xref:Veldrid.TextureDescription#Veldrid_TextureDescription_Depth) > 1) cannot. To control the number of array layers, set [TextureDescription.ArrayLayers](xref:Veldrid.TextureDescription#Veldrid_TextureDescription_ArrayLayers)).

## Multisamples

1D and 2D textures can be multisampled Textures. 3D textures cannot. This is specified by [TextureDescription.SampleCount](xref:Veldrid.TextureDescription#Veldrid_TextureDescription_SampleCount). The maximum sample count is limited by the specific GraphicsDevice being used. Use the [GetSampleCountLimit](xref:Veldrid.GraphicsDevice#Veldrid_GraphicsDevice_GetSampleCountLimit_Veldrid_PixelFormat_System_Boolean_) method to get the maximum sample count for a given [PixelFormat](xref:Veldrid.PixelFormat).

## Mipmaps

Non-multisampled textures can have multiple mipmap levels. This is specified by [TextureDescription.MipLevels](xref:Veldrid.TextureDescription#Veldrid_TextureDescription_MipLevels).

## As Shader Resources

In order to be read in a shader, a Texture needs to be created with the [TextureUsage.Sampled](xref:Veldrid.TextureUsage) flag. Additionally, the dimension of the Texture object must match what is used in the shader program. For example, an HLSL Texture2D must be matched with a 2D non-multisampled Texture object.

To bind a Texture to a shader program, a [TextureView](xref:Veldrid.TextureView) resource must be created. This TextureView can be used in a [ResourceSet](xref:Veldrid.ResourceSet) to make the Texture available to the shader.

Textures can also be written to in shaders, if they were created using the [TextureUsage.Storage](xref:Veldrid.TextureUsage) flag.

## As Framebuffer Targets

Textures can be used as Framebuffer targets. To use a Texture as a color target, it must have been created with the [TextureUsage.RenderTarget](xref:Veldrid.TextureUsage) flag. To use a Texture as a depth target, it must have been created with the [TextureUsage.DepthStencil](xref:Veldrid.TextureUsage) flag.

## Updating a Texture

Texture data is uploaded from CPU memory using the [CommandList.UpdateTexture](xref:Veldrid.CommandList#Veldrid_CommandList_UpdateTexture_Veldrid_Texture_IntPtr_System_UInt32_System_UInt32_System_UInt32_System_UInt32_System_UInt32_System_UInt32_System_UInt32_System_UInt32_System_UInt32_) and [CommandList.UpdateTextureCube](xref:Veldrid.CommandList#Veldrid_CommandList_UpdateTextureCube_Veldrid_Texture_IntPtr_System_UInt32_Veldrid_CubeFace_System_UInt32_System_UInt32_System_UInt32_System_UInt32_System_UInt32_System_UInt32_) methods. These methods can be used to upload the data for a subregion of a single mipmap level and array layer. Multiple calls should be used to fully upload all of the mipmap levels and array layers for a Texture, as appropriate.