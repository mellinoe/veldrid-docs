---
uid: framebuffers
---

# Framebuffers

[Framebuffers](xref:Veldrid.Framebuffer) are device resources which control the set of textures that render commands draw into. Framebuffers have zero-or-more color attachments, and zero-or-one depth attachment. They are created with a [FramebufferDescription](xref:Veldrid.FramebufferDescription).

## Target Textures

For a [Texture](xref:Veldrid.Texture) to be used as the color target of a Framebuffer, it must have been created with the [TextureUsage.RenderTarget](xref:Veldrid.TextureUsage) flag. To be used as the depth target of a Framebuffer, it must have been created with the DepthStencil flag. Additionally, only Textures created using the R16_UNorm and R32_Float [PixelFormats](xref:Veldrid.PixelFormat) are able to be used as depth targets.

All target textures used to create a Framebuffer must have the same dimensions and multisample count.

## Framebuffer Compatibility

Before any Draw command can be issued, there must be an active Framebuffer set using the [CommandList.SetFramebuffer](xref:Veldrid.CommandList#Veldrid_CommandList_SetFramebuffer_Veldrid_Framebuffer_) method. In order to issue a Draw command to a CommandList, the current Pipeline and Framebuffer also must be compatible. A Pipeline is compatible with a Framebuffer if it has the same number of outputs, and the format of those outputs all match.

A Framebuffer exposes an [OutputDescription](xref:Veldrid.Framebuffer#Veldrid_Framebuffer_OutputDescription) property, which can be used to create a graphics Pipeline object (see [PipelineDescription.Outputs](xref:Veldrid.GraphicsPipelineDescription#Veldrid_GraphicsPipelineDescription_Outputs)), ensuring that its outputs are compatible.

## Multisampled Framebuffers

It is possible to create a multisampled Framebuffer by using multisampled target textures. All target textures must have the same multisample count. In order to resolve the multisampled target textures into a single-sampled texture (in order to present the image, for example), the [CommandList.ResolveTexture](xref:Veldrid.CommandList#Veldrid_CommandList_ResolveTexture_Veldrid_Texture_Veldrid_Texture_) method should be used.