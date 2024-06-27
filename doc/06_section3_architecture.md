## III. Architecture

µVkCompute aims to provide a straightforward interface for developing Vulkan compute shader benchmarks. Its architecture reflects this goal by hiding complex Vulkan operations and providing convenient abstractions. This section delves into the core components and code structure of the µVkCompute project.

**A. Conceptual Overview**

The µVkCompute architecture centers around a layered approach for managing Vulkan resources and executing compute shaders. Here's a simplified representation of the key components:

```
  +----------------------+
  | Benchmarking Logic  |
  +----------------------+
        |
  +----------------------+
  | Command Buffer      | <--- Wraps Vulkan command buffer for dispatching shaders.
  +----------------------+
        |
  +----------------------+
  | Pipeline            | <--- Represents a Vulkan compute pipeline.
  +----------------------+
        |
  +----------------------+
  | Shader Module       | <--- Loads and reflects on a SPIR-V shader module.
  +----------------------+
        |
  +----------------------+
  | Device              | <--- Manages Vulkan device and resource creation (buffers, images).
  +----------------------+
        |
  +----------------------+
  | Driver              | <--- Handles Vulkan instance creation and physical device enumeration.
  +----------------------+
        |
  +----------------------+
  | Dynamic Symbols     | <--- Loads Vulkan function pointers dynamically.
  +----------------------+
```

1. **Dynamic Symbols:** This layer loads Vulkan functions dynamically, allowing µVkCompute to support different Vulkan implementations and versions.
2. **Driver:** The driver component handles the creation of a Vulkan instance and enumerates available physical devices on the system.
3. **Device:** This component manages a specific logical Vulkan device, created based on a chosen physical device. It provides functions for creating buffers, images, and other Vulkan resources.
4. **Shader Module:** This component loads SPIR-V shader modules and uses reflection to extract information about resource bindings and pipeline layout requirements.
5. **Pipeline:** This layer creates a Vulkan compute pipeline based on a shader module, potentially with specialized constants.
6. **Command Buffer:** This component wraps a Vulkan command buffer, providing a simplified interface for recording shader dispatch commands and managing resource dependencies.
7. **Benchmarking Logic:** This is the top-level layer where benchmark-specific code resides. It utilizes the lower-level components to set up resources, dispatch shaders, and measure performance.

**B. Code Structure**

The µVkCompute source code is organized into directories reflecting these layers:

* **`base/`:** Contains fundamental utilities used throughout the project, such as status handling, logging, file management, and dynamic library loading.
* **`android/`:** Provides utilities for working with the Vulkan API on the Android platform, including a workaround for specific symbol loading issues. 
* **`vulkan/`:** Implements the core abstractions of the Vulkan compute pipeline:
    * `driver.h/cc`: Defines the `Driver` class for managing Vulkan instances and physical devices.
    * `device.h/cc`: Defines the `Device` class for managing logical devices and resource creation.
    * `shader_module.h/cc`: Defines the `ShaderModule` class for loading and reflecting on SPIR-V modules.
    * `pipeline.h/cc`: Defines the `Pipeline` class for creating compute pipelines.
    * `command_buffer.h/cc`: Defines the `CommandBuffer` class for recording and submitting compute commands.
    * `buffer.h/cc`: Defines the `Buffer` class for managing Vulkan buffer objects.
    * `image.h/cc`: Defines the `Image` and `Sampler` classes for managing Vulkan image and sampler objects.
* **`benchmark/`:** Contains utilities and infrastructure for writing benchmarks:
    * `vulkan_context.h/cc`: Defines the `VulkanContext` structure, which holds shared Vulkan objects for multiple benchmarks.
    * `vulkan_buffer_util.h/cc`: Provides helper functions for managing data in Vulkan buffers.
    * `vulkan_image_util.h/cc`: Provides helper functions for managing data in Vulkan images.
    * `data_type_util.h/cc`: Provides utilities for working with different data types, including half-precision floating point (fp16).
    * `dispatch_void_shader.h/cc`: Defines a benchmark for measuring queue submission overhead.
    * `status_util.h/cc`: Provides macros for error checking and reporting in benchmark code.
    * `main.h/cc`: Defines the entry point for the benchmark executables and sets up the benchmarking environment.
* **`compute/`, `convolution/`, `matmul/`, etc.:** These directories contain individual benchmarks. Each benchmark consists of:
    * `.glsl` file: Contains the GLSL source code for the compute shader.
    * `_main.cc` file: Contains the C++ code for setting up the benchmark, dispatching the shader, and verifying results.

By understanding this architectural breakdown, developers can effectively utilize µVkCompute to write and analyze Vulkan compute shader benchmarks.
