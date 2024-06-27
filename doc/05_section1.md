## I. Introduction 

**What is µVkCompute?**

µVkCompute is a lightweight framework designed to simplify the development and execution of Vulkan compute shader microbenchmarks. It provides a streamlined pipeline, abstracting away common Vulkan boilerplate and offering a collection of pre-built compute shaders for benchmarking and profiling GPU compute capabilities. 

µVkCompute aims to bridge the gap between the complexities of the Vulkan API and the desire for rapid prototyping and performance evaluation of compute shaders. It empowers developers to quickly assess the performance characteristics of their shaders across various GPUs,  providing insights into memory bandwidth, compute throughput, and other critical performance metrics. 

**Key Features:**

* **Abstraction of Vulkan Boilerplate:** µVkCompute handles the essential Vulkan setup, including instance and device creation, memory allocation, and command pool management. This frees developers from the need to write and maintain repetitive Vulkan code. 
* **Simplified Shader Resource Management:** Leveraging SPIR-V reflection, µVkCompute automatically constructs pipeline layouts and compute pipelines based on the resources used by the shaders. This eliminates the manual specification of descriptor sets and pipeline bindings. 
* **Thin Wrappers for Command Buffer Construction and Dispatch:**  µVkCompute provides convenient functions to construct and submit command buffers for executing compute shaders. The focus is on single dispatch operations, allowing for precise measurement of shader execution time.

**Target Audience:**

µVkCompute is targeted at developers interested in:

* **Benchmarking Compute Shader Performance:**  Evaluating the performance of compute shaders on different GPUs and under varying workload conditions.
* **Profiling GPU Compute Capabilities:** Investigating the memory bandwidth, compute throughput, and other key performance characteristics of target hardware.
* **Rapid Prototyping of Compute Shaders:** Quickly testing and iterating on new compute shader implementations.

µVkCompute simplifies the process of writing Vulkan compute benchmarks and empowers developers to focus on shader optimization and performance analysis, without getting bogged down in the complexities of the underlying API. 
