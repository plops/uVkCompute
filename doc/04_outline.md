## µVkCompute Documentation Outline

This outline proposes a more thorough documentation for the µVkCompute project. 

**I. Introduction**

* **What is µVkCompute?** Briefly explain the project's purpose, its utility as a micro Vulkan compute pipeline, and its role in benchmarking and profiling GPU compute capabilities.
* **Key Features:** Highlight µVkCompute's main functionalities:
    * Abstraction of Vulkan boilerplate (instance/device creation).
    * Simplified shader resource management via SPIR-V reflection.
    * Thin wrappers for command buffer construction and dispatch.
* **Target Audience:** Identify the intended audience for µVkCompute, emphasizing developers focusing on compute shader microbenchmarking and GPU profiling.

**II. Getting Started**

* **Prerequisites:** Detail the necessary tools and libraries for building and using µVkCompute:
    * CMake
    * (Optional) Ninja
    * C/C++ compiler
    * Python
    * Vulkan SDK (with `VULKAN_SDK` environment variable set)
* **Installation:** Provide clear step-by-step instructions on how to build and install µVkCompute, including platform-specific guidance if needed. 
* **Running Benchmarks:** Explain how to execute the included benchmarks and interpret their output.
    * Command-line arguments: Detail usage of flags like `--latency_measure_mode` and `--benchmark_*`.
    * Output interpretation: Describe the format of the benchmark results and how to analyze them.

**III. Architecture**

* **Conceptual Overview:** Provide a high-level diagram outlining the core components of µVkCompute:
    * Vulkan driver abstraction
    * Device management
    * Shader module loading and reflection
    * Pipeline creation
    * Command buffer construction and dispatch
* **Code Structure:** Explain the directory structure and organization of the source code:
    * `base/`: Fundamental utilities (status, logging, files, dynamic libraries).
    * `android/`:  Android-specific Vulkan utilities.
    * `vulkan/`: Vulkan compute pipeline abstractions.
    * `benchmark/`: Benchmarking utilities and common benchmark code.
    * `compute/`, `convolution/`, `matmul/`, etc.: Directories containing specific benchmarks.

**IV. Benchmarking and Profiling**

* **Benchmark Categories:** Group and categorize the included benchmarks, explaining the purpose of each:
    * Compute throughput benchmarks (`compute/mad_throughput`)
    * Memory bandwidth benchmarks (`memory/copy_storage_buffer`)
    * Reduction benchmarks (`reduction/tree_reduce`)
    * Convolution benchmarks (`convolution/conv2d`)
    * Subgroup operation benchmarks (`subgroup/subgroup_arithmetic`)
* **Adding New Benchmarks:** Provide a step-by-step guide on how to create and integrate a new benchmark into µVkCompute:
    * Writing the shader (`foo.glsl`)
    * Implementing the benchmark logic (`foo_main.cc`)
    * Integrating with CMake
* **Latency Measurement Modes:** Explain the different latency measurement modes supported by µVkCompute:
    * `system_submit`: End-to-end CPU-measured latency.
    * `system_dispatch`: CPU-measured latency excluding queue submission overhead.
    * `gpu_timestamp`:  GPU-measured latency using timestamp queries.

**V. Advanced Usage**

* **Customizing the Vulkan Context:** Explain how to modify the Vulkan context to suit specific benchmarking needs, such as using a specific physical device or enabling Vulkan extensions.
* **Integrating with External Tools:**  Discuss how to use µVkCompute in conjunction with external profiling and debugging tools like RenderDoc.

**VI. Contributing**

* **Contribution Guidelines:** Outline the process for contributing to µVkCompute, including coding standards, pull request workflow, and testing requirements.
* **Reporting Issues:** Provide clear instructions on how to report bugs or request new features.

**VII. API Reference**

* **Detailed documentation for key classes and functions:**
    * `uvkc::vulkan::Driver`
    * `uvkc::vulkan::Device`
    * `uvkc::vulkan::ShaderModule`
    * `uvkc::vulkan::Pipeline`
    * `uvkc::vulkan::CommandBuffer`
    * `uvkc::benchmark::VulkanContext`

This documentation outline will serve as a valuable resource for both novice and experienced users of µVkCompute, promoting better understanding, easier onboarding, and efficient project maintenance. 
