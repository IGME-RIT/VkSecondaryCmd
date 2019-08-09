Secondary Command Buffers allow us to execute one 
command buffer from another. In this tutorial, rather
than having 3 command buffers (one for each swapchain
image) that are all almost identical, we have 3 
command buffers (one for each swapchain image) that
only handle the framebuffer, and then they all call a 
secondary command buffer, which we only have one of.
The secondary command buffer will hold the viewport,
scissor, pipeline bindings, vertex / index buffers, draw
commands, etc

In this tutorial, we add a VkCommandBuffer to Demo.h
	VkCommandBuffer drawCmd;

We also add the function Demo::build_secondary_cmd()
to Demo.cpp, which we call in prepare(), then we call
execute the secondary command buffer from inside the
primary command buffers that are in build_swapchain_cmds()

When we build the swapchain cmds, we add a flag to 
VkCommandBufferBeginInfo that allows us to call a secondary
command buffer, and we change the parameter in 
vkCmdBeginRenderPass from VK_SUBPASS_CONTENTS_INLINE
to VK_SUBPASS_CONTENTS_SECONDARY_COMMAND_BUFFERS.
Then we use vkCmdExecuteCommands to execute the secondary
command buffer.