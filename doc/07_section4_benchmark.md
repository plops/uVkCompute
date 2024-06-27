## IV. Benchmarking and Profiling

µVkCompute provides a diverse collection of compute shaders designed to probe and evaluate various performance aspects of a target GPU. This section categorizes these benchmarks, explains their purpose, and guides users on adding new ones to the suite.

### Benchmark Categories

* **Compute Throughput Benchmarks (`compute/mad_throughput`)**

   The `mad_throughput` benchmark measures the raw compute capability of the GPU by executing a massive chain of Multiply-Add (MAD) operations within a loop.  It uses vectorized operations (vec4) to exploit potential instruction-level parallelism and hide latency. 

   This benchmark provides insights into the peak theoretical floating-point performance of the GPU.

* **Memory Bandwidth Benchmarks (`memory/`)**

   These benchmarks assess the GPU's memory bandwidth characteristics using different access patterns:

   * **`copy_sampled_image_to_storage_buffer`**: Copies data from a 2D sampled image to a storage buffer, benchmarking texture read bandwidth.
   * **`copy_storage_buffer`**: Copies data between two storage buffers, evaluating buffer read and write bandwidth using various vectorization factors and elements copied per thread.

* **Reduction Benchmarks (`reduction/`)**

   Reduction operations, common in many algorithms, are evaluated using different strategies:

   * **`tree_reduce`**: Performs in-place reduction over a data buffer using either a loop-based approach or subgroup reduction operations.
   * **`atomic_reduce`**: Divides data into batches processed by individual workgroups, using atomic operations to accumulate the final result.
   * **`one_workgroup_reduce`**: Executes reduction within a single workgroup without explicit synchronization mechanisms.

* **Convolution Benchmarks (`convolution/`)**

   These benchmarks target 2D convolution operations, a core building block of many deep learning models:

   * **`conv2d`**: Evaluates general 2D convolution throughput using a tiled approach, distributing work across workgroups and invocations.
   * **`depthwise_conv2d`**:  Focuses on depthwise convolution, a more specialized form of convolution, using a similar tiled approach.

* **Subgroup Operation Benchmarks (`subgroup/subgroup_arithmetic`)**

   The `subgroup_arithmetic` benchmark evaluates the performance of subgroup-level arithmetic operations (add and multiply), comparing loop-based implementations with Vulkan's intrinsic subgroup functions. This benchmark helps understand the efficiency of utilizing subgroup operations on the target GPU.

* **Matrix Multiplication Benchmarks (`matmul/` & `mmt/` & `vmt/`)**

    Matrix multiplication is a fundamental operation across various domains.  These benchmarks explore diverse strategies for its implementation:

    * **`matmul_tiled`**: Employs a tiled approach, varying tile sizes and data types to analyze performance across different configurations.
    * **`mmt`**: Evaluates strategies for matrix matrix transposed multiplication, again using tiling and varied data types.
    * **`vmt`**: Explores vector matrix transposed multiplication with similar configuration variations.

### Adding New Benchmarks

Integrating new benchmarks into µVkCompute follows a straightforward process:

1. **Write the Shader (`foo.glsl`)**: Create a GLSL shader implementing the desired computation. Use preprocessor macros for potential configuration variations (e.g., workgroup size, tile size, data type).

2. **Implement the Benchmark Logic (`foo_main.cc`)**: 
   * Include the generated SPIR-V code (`foo_spirv_instance.inc` or `foo_spirv_permutation.inc`).
   * Define the following functions within the `uvkc::benchmark` namespace:
      * `CreateVulkanContext()`:  Construct the Vulkan context. Consider customizing for specific needs.
      * `RegisterVulkanOverheadBenchmark()`: Register a benchmark to measure baseline overhead for `LatencyMeasureMode::kSystemDispatch`.  
      * `RegisterVulkanBenchmarks()`:  Programmatically register the benchmark using the Google Benchmark API, specifying the shader, parameters, and latency measurement mode.

3. **Integrate with CMake**:  Register the new shader and benchmark in the `CMakeLists.txt` file:
   * Use `uvkc_glsl_shader_instance` or `uvkc_glsl_shader_permutation` to generate the SPIR-V modules from `foo.glsl`.
   * Use `uvkc_cc_binary` to build `foo_main.cc`, linking it with the generated shader, benchmark library, and `uvkc::benchmark::main`.

### Latency Measurement Modes

µVkCompute supports multiple modes for measuring shader execution latency, controlled by the `--latency_measure_mode` flag:

* **`system_submit`**:  Measures the complete CPU-side time between queue submission and the completion of the submitted command buffer. This includes the overhead of queue submission and synchronization.

* **`system_dispatch`**: Attempts to isolate the "dispatch" time of the kernel by subtracting the time for a void dispatch from the `system_submit` measurement. 

* **`gpu_timestamp`**: Utilizes GPU timestamps to measure the duration between the start and end of the shader execution on the GPU. This mode relies on the availability of timestamp queries on the target GPU and offers a more accurate measure of actual shader runtime.

By understanding the different benchmark categories, their purpose, and the available latency measurement modes, users can effectively employ µVkCompute to gain deeper insights into the performance characteristics of their target GPUs.
