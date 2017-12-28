---
uid: command-lists
---

# Command Lists

Using Veldrid, all graphics commands must be executed using a [CommandList](xref:Veldrid.CommandList). A CommandList is a special device resource (created by a [GraphicsDevice](xref:Veldrid.GraphicsDevice)) that records a variety of commands. These commands can then be submitted for execution on the device using the [GraphicsDevice.SubmitCommands](xref:Veldrid.GraphicsDevice#Veldrid_GraphicsDevice_SubmitCommands_Veldrid_CommandList_) method.

Several types of commands are available, depending on what work is being done.

## Resource Updates

[Texture](xref:Veldrid.Texture) and [DeviceBuffer](xref:Veldrid.DeviceBuffer) objects can be updated directly in a CommandList, using the UpdateBuffer and UpdateTexture methods.

The color and depth targets of the active [Framebuffer](xref:Veldrid.Framebuffer) can be cleared.

Multisampled Textures can be resolved down into a regular non-multisampled Texture using the ResolveTexture method.

## State Changes

There are several state-change methods available, which control various pieces of state influencing Draw commands. The following state can be changed:
* [Framebuffer](xref:Veldrid.Framebuffer)
* [Viewports](xref:Veldrid.Viewport)
* Scissor rectangles
* [Vertex Buffers](xref:Veldrid.DeviceBuffer)
* [Index Buffer](xref:Veldrid.DeviceBuffer)
* [Pipeline](xref:Veldrid.Pipeline)
* [ResourceSets](xref:Veldrid.ResourceSet)

## Drawing

The [DrawIndexed](xref:Veldrid.CommandList#Veldrid_CommandList_DrawIndexed_System_UInt32_System_UInt32_System_UInt32_System_Int32_System_UInt32_) method can be invoked to record an indexed draw command into the CommandList. The effect of this draw is controlled by the active state of the CommandList -- which vertex buffer, index buffer, Pipeline, ResourceSets, and Framebuffer are bound, and what Viewport and scissor rectangles are set. Regular indexed and instanced draw commands are both supported by this method.

The [Draw](xref:Veldrid.CommandList#Veldrid_CommandList_Draw_System_UInt32_System_UInt32_System_UInt32_System_UInt32_) method can be invoked to record a non-indexed draw command into the CommandList. This method can be used without an index buffer bound, and simply selects sequential vertices from the bound vertex buffer, if it exists.

## Compute Dispatch

The [Dispatch](xref:Veldrid.CommandList#Veldrid_CommandList_Dispatch_System_UInt32_System_UInt32_System_UInt32_) method can be invoked to record a compute dispatch command into the CommandList. The effects of this call depend on the currently-bound compute Pipeline and ResourceSets.

## Indirect

There are also "Indirect" variants of the above draw and compute functions. These "Indirect" variants allow the relevant draw/dispatch parameters to be read from a GPU DeviceBuffer, rather than passed in directly. This "Indirect" buffer must have been created with the [BufferUsage.IndirectBuffer](xref:Veldrid.BufferUsage) flag, and is subject to some other restrictions. The information stored in the buffer should adhere to the format of one of the following structures, depending on the operation being performed:

* [IndirectDrawArguments](xref:Veldrid.IndirectDrawArguments), used by [DrawIndirect](xref:Veldrid.CommandList#Veldrid_CommandList_DrawIndirect_Veldrid_DeviceBuffer_System_UInt32_System_UInt32_System_UInt32_)
* [IndirectDrawIndexedArguments](xref:Veldrid.IndirectDrawIndexedArguments), used by [DrawIndexedIndirect](xref:Veldrid.CommandList#Veldrid_CommandList_DrawIndexedIndirect_Veldrid_DeviceBuffer_System_UInt32_System_UInt32_System_UInt32_)
* [IndirectDispatchArguments](xref:Veldrid.IndirectDispatchArguments), used by [DispatchIndirect](xref:Veldrid.CommandList#Veldrid_CommandList_DispatchIndirect_Veldrid_DeviceBuffer_System_UInt32_)