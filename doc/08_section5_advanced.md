## V. Advanced Usage

This section details advanced usage scenarios for µVkCompute, empowering users with more fine-grained control over the benchmarking environment and integration with external tools.

### 5.1 Customizing the Vulkan Context

By default, µVkCompute creates a Vulkan context using `CreateDefaultVulkanContext()`, which initializes a logical device with one compute queue for each available physical device on the system. While this approach is suitable for most benchmarking scenarios, users might need to customize the Vulkan context for specific requirements.

Here are some common customization scenarios:

* **Selecting a Specific Physical Device:** To benchmark against a specific GPU, modify the `CreateVulkanContext()` implementation in your benchmark's `main.cc` file. Instead of creating a device for each physical device, iterate through the available devices (accessible via `VulkanContext::physical_devices`), identify the desired one (by name or properties), and create a device only for that specific GPU.
* **Enabling Vulkan Extensions:**  If your benchmark requires specific Vulkan extensions, modify the `VkInstanceCreateInfo` and `VkDeviceCreateInfo` structures used during instance and device creation. Add the necessary extensions to the `ppEnabledExtensionNames` member of these structures. Note that not all extensions are supported by all GPUs, so ensure your chosen device supports the required extensions before enabling them. 
* **Modifying Device Features:**  Similarly, if your benchmark utilizes specific device features,  adjust the `pEnabledFeatures` member of `VkDeviceCreateInfo` to enable the desired features. 

### 5.2 Integrating with External Tools

µVkCompute can be integrated with external profiling and debugging tools to gain deeper insights into shader execution and performance.

* **RenderDoc Integration:**
    * Enabling RenderDoc Capture: By setting the `--enable_renderdoc` command-line flag to `true`, µVkCompute will automatically start and end a RenderDoc capture for each benchmark run. This allows you to inspect the graphics pipeline state, shader execution, and memory access patterns within RenderDoc.
    * RenderDoc API: For more fine-grained control over RenderDoc capture and analysis, you can access the RenderDoc API directly.  The `GetRdocApi()` function (defined in `benchmark/main.cc`) provides a handle to the RENDERDOC_API_1_6_0 struct, enabling interaction with RenderDoc's API from your benchmark code.

**Important:**

* **Device Compatibility:** Remember to check for device compatibility when customizing the Vulkan context. Not all devices support all extensions or features.
* **Tool Documentation:** Refer to the documentation of your chosen profiling or debugging tool for specific usage instructions and API details.

By leveraging these advanced customization and integration techniques, users can tailor µVkCompute to their specific needs and gain deeper insights into GPU compute performance. 
