# API Documentation

The Veldrid API documentation is automatically generated from source-code-level comments. Often, more information can be found by looking into the [source code](https://github.com/mellinoe/veldrid) itself.

See [API Concepts](xref:api-concepts) for a broad overview and description of the most commond Veldrid object types. Also, take a look at the [Samples repository](https://github.com/mellinoe/veldrid-samples) for a more hands-on demonstration of Veldrid.

* [GraphicsDevice](xref:Veldrid.GraphicsDevice): An abstract graphics device, which is the entry point type into Veldrid.
* [CommandList](xref:Veldrid.CommandList): A device resource which enables the recording of graphics and compute commands. Later, they can be submitted to the GraphicsDevice for execution.
* [Buffer](xref:Veldrid.Buffer) and [Texture](xref:Veldrid.Texture): Device resources capable of storing various types of structured data, which can be read by the GPU.
* [Pipeline](xref:Veldrid.Pipeline) and [ResourceSet](xref:Veldrid.ResourceSet): Two important kinds of device resources which set up the GPU state necessary for issuing drawing and compute commands.
* [Framebuffer](xref:Veldrid.Framebuffer): A resource which controls where draw commands are rendered to.