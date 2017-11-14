---
uid: multi-threading
---

# Multi-Threading in Veldrid

Modern graphics APIs encourage the full utilization of CPU resources, and allow rendering commands to be submitted from many different application threads. Veldrid is similarly flexible, and most objects created by Veldrid can be utilized in a multi-threaded application.

# GraphicsDevice
All properties and methods of a [GraphicsDevice](xref:Veldrid.GraphicsDevice) can be safely invoked from any thread at any time.

Note that graphics commands do not complete synchronously with the method [GraphicsDevice.ExecuteCommands](xref:Veldrid.GraphicsDevice#Veldrid_GraphicsDevice_ExecuteCommands_Veldrid_CommandList_). They are merely submitted to the graphics device and may complete later, but submission order is respected. [GraphicsDevice.WaitForIdle](xref:Veldrid.GraphicsDevice#Veldrid_GraphicsDevice_WaitForIdle) can be called from any thread to block until all submitted graphics commands have completed.

# ResourceFactory
The [ResourceFactory](xref:Veldrid.ResourceFactory) object is responsible for the creation of all resources owned by the graphics device, and all of its methods can be safely invoked from any thread at any time. Device resources created this way can be used from multiple threads, but care must be taken when issuing resource update commands to them (see the notes on CommandList).

# CommandList
The usage of a [CommandList](xref:Veldrid.CommandList) is not thread-safe, but multiple CommandList objects can be created and used in parallel. Applications can leverage this to execute separate, independent render passes in parallel. For example, several cascaded shadow map passes, along with a 2D UI overlay, could be executed in parallel and then utilized in the scene's final output.

In general, CommandLists can execute commands in parallel which read from the same graphics resources, but not update them. For example, [CommandList.UpdateTexture](xref:Veldrid.CommandList#Veldrid_CommandList_UpdateTexture_Veldrid_Texture_IntPtr_System_UInt32_System_UInt32_System_UInt32_System_UInt32_System_UInt32_System_UInt32_System_UInt32_System_UInt32_System_UInt32_) cannot be executed on the same [Texture](xref:Veldrid.Texture) resource by multiple command lists in parallel, but several CommandLists could issue draw commands which read from that same Texture in parallel.
