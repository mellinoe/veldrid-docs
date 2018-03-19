# Veldrid 4.1.3

Veldrid version 4.1.3 is available. This is a small patch with some minor fixes and improvements.

## All Changes

**Veldrid**

* [Vulkan] Use correct Framebuffer dimensions when setting Swapchain.SyncToVerticalBlank. [[1161758]](https://github.com/mellinoe/veldrid/commit/116175824de7719af32b7d9d559d3a7a8e252228)
* [Vulkan] Fix an issue with the tracking of clear values in VkCommandList. [[c337c56]](https://github.com/mellinoe/veldrid/commit/c337c56139c6dcec830571c9aaa149aff427631f)
* [Vulkan] Fix an erroneous comparison in VkTexture.cs. [[e84db0e]](https://github.com/mellinoe/veldrid/commit/e84db0ece093710f51f2715ea32011578f009037)
* Defensively copy some arrays in the creation of resources, when necessary. [[0e0fa87]](https://github.com/mellinoe/veldrid/commit/0e0fa874f1604e320f9b5fdd460160bd29699780)
* [Vulkan] Add more needed synchronization to VkGraphicsDevice. [[8d5490a]](https://github.com/mellinoe/veldrid/commit/8d5490aec433cf4c3bada9debd68fa60e7d72b78)
* [D3D11] Synchronize DXGI usage with the immediate context (per MSDN). [[de32772]](https://github.com/mellinoe/veldrid/commit/de327720d604e861bec2b47108bb08673b785683)
* [Metal] Support CommandList.CopyBuffer and UpdateBuffer when region is unaligned. [[903ea1c]](https://github.com/mellinoe/veldrid/commit/903ea1c168ee8b4979b269924adadcb39e77e201)
* [Metal] Fix condition for compute buffer copy path. [[10dcb69]](https://github.com/mellinoe/veldrid/commit/10dcb698918d8832cbbb7708c45477e5af3232cc)
* [Vulkan] Fix aspect mask for depth-stencil clears. [[5f6ceb9]](https://github.com/mellinoe/veldrid/commit/5f6ceb942d2e46ea3be0fa9ac300a21eb78e7953)
* [OpenGL] Fix issues with texture binding and state tracking. [[4e25323]](https://github.com/mellinoe/veldrid/commit/4e25323875ec2b96070aad596c4d89601c8d9883)
* [Vulkan] Fix an issue with how render passes are specified. [[88c8a87]](https://github.com/mellinoe/veldrid/commit/88c8a8781709274e078d24fceba96e0f54f65fbe)
* [Vulkan] Fix multiple vertex bindings in Vulkan. [[271a9bb]](https://github.com/mellinoe/veldrid/commit/271a9bb0c6df1b23c380f2fa9a7b5a5a4cd942bf)
