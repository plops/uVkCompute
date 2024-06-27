## VII. API Reference

This section provides detailed documentation for key classes and functions in the µVkCompute library.

**uvkc::vulkan::Driver**

Represents a Vulkan driver and serves as the starting point for interacting with the Vulkan system.

* **`Create(app_name: const char*, symbols: DynamicSymbols*) -> StatusOr<unique_ptr<Driver>>`:** 
    * Creates a Vulkan driver for an application with the specified name.
    * Takes the application name and a pointer to a `DynamicSymbols` object.
    * Returns a `StatusOr` containing a unique pointer to the created `Driver` on success, or an error status on failure.
* **`~Driver()`:** Destructor, destroys the underlying `VkInstance`.
* **`struct PhysicalDeviceInfo`:**
    * Structure containing information about a physical device.
    * Members:
        * `handle: VkPhysicalDevice`: Handle to the physical device.
        * `v10_properties: VkPhysicalDeviceProperties`: Vulkan 1.0 properties of the device.
        * `subgroup_properties: VkPhysicalDeviceSubgroupProperties`: Subgroup properties of the device.
* **`EnumeratePhysicalDevices() -> StatusOr<vector<PhysicalDeviceInfo>>`:**
    * Enumerates all available physical devices on the system.
    * Returns a `StatusOr` containing a vector of `PhysicalDeviceInfo` structures on success, or an error status on failure.
* **`CreateDevice(physical_device: const PhysicalDeviceInfo&, queue_flags: VkQueueFlags) -> StatusOr<unique_ptr<Device>>`:**
    * Creates a logical device from the given physical device.
    * Takes the `PhysicalDeviceInfo` of the desired device and a bitmask of queue flags.
    * Returns a `StatusOr` containing a unique pointer to the created `Device` on success, or an error status on failure.
* **`GetInstance() -> VkInstance`:** Returns the underlying `VkInstance` handle.

**uvkc::vulkan::Device**

Represents a Vulkan logical device and provides functionality for creating and managing Vulkan resources.

* **`Create(physical_device: VkPhysicalDevice, queue_family_index: uint32_t, valid_timestamp_bits: uint32_t, nanoseconds_per_timestamp_value: uint32_t, device: VkDevice, symbols: const DynamicSymbols&) -> StatusOr<unique_ptr<Device>>`:**
    * Creates a Vulkan logical device.
    * Takes the physical device handle, queue family index, valid timestamp bits, nanoseconds per timestamp value, logical device handle, and a reference to a `DynamicSymbols` object.
    * Returns a `StatusOr` containing a unique pointer to the created `Device` on success, or an error status on failure.
* **`~Device()`:** Destructor, destroys the underlying `VkDevice` and other resources.
* **`CreateBuffer(usage_flags: VkBufferUsageFlags, memory_flags: VkMemoryPropertyFlags, size_in_bytes: VkDeviceSize) -> StatusOr<unique_ptr<Buffer>>`:**
    * Creates a buffer with the specified usage flags, memory flags, and size.
    * Returns a `StatusOr` containing a unique pointer to the created `Buffer` on success, or an error status on failure.
* **`CreateImage(usage_flags: VkImageUsageFlags, memory_flags: VkMemoryPropertyFlags, image_type: VkImageType, image_format: VkFormat, dimensions: VkExtent3D, image_tiling: VkImageTiling, view_type: VkImageViewType) -> StatusOr<unique_ptr<Image>>`:**
    * Creates an image with the specified parameters.
    * Returns a `StatusOr` containing a unique pointer to the created `Image` on success, or an error status on failure.
* **`CreateSampler() -> StatusOr<unique_ptr<Sampler>>`:**
    * Creates a sampler with default parameters (nearest filtering, clipping to edge).
    * Returns a `StatusOr` containing a unique pointer to the created `Sampler` on success, or an error status on failure.
* **`CreateShaderModule(spirv_data: const uint32_t*, spirv_size: size_t) -> StatusOr<unique_ptr<ShaderModule>>`:**
    * Creates a shader module from the given SPIR-V code.
    * Returns a `StatusOr` containing a unique pointer to the created `ShaderModule` on success, or an error status on failure.
* **`CreatePipeline(shader_module: const ShaderModule&, entry_point: const char*, spec_constants: Span<Pipeline::SpecConstant>) -> StatusOr<unique_ptr<Pipeline>>`:**
    * Creates a compute pipeline using the specified shader module, entry point, and specialization constants.
    * Returns a `StatusOr` containing a unique pointer to the created `Pipeline` on success, or an error status on failure.
* **`CreateDescriptorPool(shader_module: const ShaderModule&) -> StatusOr<unique_ptr<DescriptorPool>>`:**
    * Creates a descriptor pool with enough resources for the given shader module's layout.
    * Returns a `StatusOr` containing a unique pointer to the created `DescriptorPool` on success, or an error status on failure.
* **`struct BoundBuffer`:**
    * Represents a buffer bound to a descriptor set.
    * Members:
        * `buffer: const Buffer*`: Pointer to the bound buffer.
        * `set: uint32_t`: Descriptor set index.
        * `binding: uint32_t`: Binding index within the set.
* **`AttachBufferToDescriptor(shader_module: const ShaderModule&, layout_set_map: const unordered_map<VkDescriptorSetLayout, VkDescriptorSet>&, bound_buffers: Span<const BoundBuffer>) -> Status`:**
    * Attaches buffers to descriptors for the given shader module.
    * Takes the shader module, a map of descriptor set layouts to allocated sets, and a span of `BoundBuffer` structures.
    * Returns an `OkStatus` on success, or an error status on failure.
* **`struct BoundImage`:**
    * Represents an image bound to a descriptor set.
    * Members:
        * `image: const Image*`: Pointer to the bound image.
        * `sampler: const Sampler*`: Pointer to the associated sampler (optional).
        * `set: uint32_t`: Descriptor set index.
        * `binding: uint32_t`: Binding index within the set.
* **`AttachImageToDescriptor(shader_module: const ShaderModule&, layout_set_map: const unordered_map<VkDescriptorSetLayout, VkDescriptorSet>&, bound_images: Span<const BoundImage>) -> Status`:**
    * Attaches images to descriptors for the given shader module.
    * Takes the shader module, a map of descriptor set layouts to allocated sets, and a span of `BoundImage` structures.
    * Returns an `OkStatus` on success, or an error status on failure.
* **`AllocateCommandBuffer() -> StatusOr<unique_ptr<CommandBuffer>>`:**
    * Allocates a primary command buffer from the device's command pool.
    * Returns a `StatusOr` containing a unique pointer to the allocated `CommandBuffer` on success, or an error status on failure.
* **`ResetCommandPool() -> Status`:** 
    * Resets the device's command pool, recycling all allocated command buffers.
    * Returns an `OkStatus` on success, or an error status on failure.
* **`CreateTimestampQueryPool(query_count: uint32_t) -> StatusOr<unique_ptr<TimestampQueryPool>>`:**
    * Creates a query pool for managing timestamp queries.
    * Takes the number of queries to allocate in the pool.
    * Returns a `StatusOr` containing a unique pointer to the created `TimestampQueryPool` on success, or an error status on failure.
* **`QueueSubmitAndWait(command_buffer: const CommandBuffer&) -> Status`:**
    * Submits the given command buffer to the device's queue and waits for it to complete.
    * Returns an `OkStatus` on success, or an error status on failure.

**uvkc::vulkan::ShaderModule**

Represents a Vulkan shader module and provides functionality for loading, reflecting on, and accessing descriptor set information.

* **`Create(device: VkDevice, spirv_data: const uint32_t*, spirv_size: size_t, symbols: const DynamicSymbols&) -> StatusOr<unique_ptr<ShaderModule>>`:**
    * Creates a shader module from the given SPIR-V code and device.
    * Reflects on the SPIR-V code to extract descriptor set layout information.
    * Returns a `StatusOr` containing a unique pointer to the created `ShaderModule` on success, or an error status on failure.
* **`~ShaderModule()`:** Destructor, destroys the underlying `VkShaderModule` and other resources.
* **`shader_module() -> VkShaderModule`:** Returns the underlying `VkShaderModule` handle.
* **`num_sets() -> uint32_t`:** Returns the number of descriptor sets used in the shader module.
* **`descriptor_set_layouts() -> Span<const VkDescriptorSetLayout>`:** Returns a span of all descriptor set layout objects for the shader module.
* **`GetDescriptorSetLayout(set: uint32_t) -> StatusOr<VkDescriptorSetLayout>`:** 
    * Returns the `VkDescriptorSetLayout` for the given descriptor set index.
    * Returns a `StatusOr` containing the descriptor set layout on success, or an error status if the index is invalid.
* **`GetDescriptorSetLayoutMap() -> unordered_map<uint32_t, VkDescriptorSetLayout>`:** 
    * Returns a map from descriptor set indices to their corresponding `VkDescriptorSetLayout` objects.
* **`GetDescriptorSetLayoutBinding(set: uint32_t, binding: uint32_t) -> StatusOr<const VkDescriptorSetLayoutBinding *>`:** 
    * Returns a pointer to the `VkDescriptorSetLayoutBinding` structure for the given set index and binding index.
    * Returns a `StatusOr` containing the pointer on success, or an error status if the indices are invalid.
* **`CalculateDescriptorPoolSize() -> vector<VkDescriptorPoolSize>`:** 
    * Calculates the minimum pool size requirements for each descriptor type used in the shader module.
    * Returns a vector of `VkDescriptorPoolSize` structures.

**uvkc::vulkan::Pipeline**

Represents a Vulkan compute pipeline.

* **`struct SpecConstant`:**
    * Represents a specialization constant for pipeline creation.
    * Members:
        * `id: uint32_t`: ID of the specialization constant.
        * `type: Type`: Data type of the constant.
        * `value: union`: Value of the constant (union for different data types).
        * `size() -> size_t`: Returns the size of the constant in bytes.
    * Nested enum `Type`:
        * `s32`: 32-bit signed integer.
        * `u32`: 32-bit unsigned integer.
        * `f32`: 32-bit floating-point.
* **`Create(device: VkDevice, shader_module: const ShaderModule&, entry_point: const char*, spec_constants: Span<SpecConstant>, symbols: const DynamicSymbols&) -> StatusOr<unique_ptr<Pipeline>>`:**
    * Creates a compute pipeline with the given shader module, entry point, specialization constants, and device.
    * Returns a `StatusOr` containing a unique pointer to the created `Pipeline` on success, or an error status on failure.
* **`~Pipeline()`:** Destructor, destroys the underlying `VkPipeline` and other resources.
* **`pipeline() -> VkPipeline`:** Returns the underlying `VkPipeline` handle.
* **`pipeline_layout() -> VkPipelineLayout`:** Returns the `VkPipelineLayout` associated with this pipeline.

**uvkc::vulkan::CommandBuffer**

Represents a Vulkan command buffer, providing functionality for recording and submitting compute commands.

* **`CommandBuffer(device: VkDevice, command_buffer: VkCommandBuffer, symbols: const DynamicSymbols&)`:** 
    * Constructor, takes the device handle, command buffer handle, and a reference to a `DynamicSymbols` object.
* **`~CommandBuffer()`:** Destructor.
* **`command_buffer() -> VkCommandBuffer`:** Returns the underlying `VkCommandBuffer` handle.
* **`Begin() -> Status`:**  Begins recording commands into the buffer. Returns an `OkStatus` on success, or an error status on failure.
* **`End() -> Status`:** Ends recording commands into the buffer. Returns an `OkStatus` on success, or an error status on failure.
* **`Reset() -> Status`:** Resets the command buffer to its initial state. Returns an `OkStatus` on success, or an error status on failure.
* **`CopyBuffer(src_buffer: const Buffer&, src_offset: size_t, dst_buffer: const Buffer&, dst_offset: size_t, length: size_t)`:** 
    * Records a command to copy data between buffers. 
    * Takes the source buffer, source offset, destination buffer, destination offset, and length to copy.
* **`CopyBufferToImage(src_buffer: const Buffer&, src_offset: size_t, dst_image: const Image&, image_dimensions: VkExtent3D)`:**
    * Records a command to copy data from a buffer to an image.
    * Takes the source buffer, source offset, destination image, and image dimensions.
* **`CopyImageToBuffer(src_image: const Image&, image_dimensions: VkExtent3D, dst_buffer: const Buffer&, dst_offset: size_t)`:**
    * Records a command to copy data from an image to a buffer.
    * Takes the source image, image dimensions, destination buffer, and destination offset.
* **`TransitionImageLayout(image: const Image&, from_layout: VkImageLayout, to_layout: VkImageLayout) -> Status`:**
    * Records a command to transition an image's layout between different states.
    * Returns an `OkStatus` on success, or an error status on failure.
* **`BindPipelineAndDescriptorSets(pipeline: const Pipeline&, bound_descriptor_sets: Span<const BoundDescriptorSet>)`:**
    * Records a command to bind a compute pipeline and its associated descriptor sets.
    * Takes the pipeline to bind and a span of `BoundDescriptorSet` structures.
* **`ResetQueryPool(query_pool: const TimestampQueryPool&)`:**
    * Records a command to reset a timestamp query pool.
* **`WriteTimestamp(query_pool: const TimestampQueryPool&, pipeline_stage: VkPipelineStageFlagBits, query_index: uint32_t)`:**
    * Records a command to write a timestamp to a query pool.
* **`Dispatch(x: uint32_t, y: uint32_t, z: uint32_t)`:** 
    * Records a dispatch command with the given workgroup dimensions.
* **`DispatchBarrier()`:** 
    * Records a pipeline barrier to synchronize shader read/write operations within a dispatch.

**uvkc::benchmark::VulkanContext**

A structure that holds the Vulkan application context for benchmarks, including the driver, devices, and latency measurement settings.

* **`VulkanContext(symbols: unique_ptr<vulkan::DynamicSymbols>, driver: unique_ptr<vulkan::Driver>, physical_devices: vector<vulkan::Driver::PhysicalDeviceInfo>, devices: vector<unique_ptr<vulkan::Device>>)`:** 
    * Constructor, takes ownership of the Vulkan dynamic symbols, driver, physical devices, and logical devices.
* **`symbols: unique_ptr<vulkan::DynamicSymbols>`:** Unique pointer to the `DynamicSymbols` object.
* **`driver: unique_ptr<vulkan::Driver>`:** Unique pointer to the `Driver` object.
* **`physical_devices: vector<vulkan::Driver::PhysicalDeviceInfo>`:** Vector of `PhysicalDeviceInfo` structures for available physical devices.
* **`devices: vector<unique_ptr<vulkan::Device>>`:** Vector of unique pointers to created `Device` objects.
* **`latency_measure: LatencyMeasure`:** 
    * Structure containing latency measurement settings.
    * Members:
        * `mode: LatencyMeasureMode`: Mode for measuring shader execution time.
        * `overhead_seconds: double`: Overhead latency to subtract from benchmarks (used in `system_dispatch` mode).


This API reference provides a detailed overview of the key classes and functions in µVkCompute, aiding developers in understanding and utilizing the library for creating and running compute shader benchmarks.
