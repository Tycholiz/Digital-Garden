
Historically speaking, the GPU's principle job is to render triangles onto the screen
- It is more performant than the CPU, but also more specific, so cannot do the wide-range of jobs that can be performed by a CPU

Their use has extended past this initial purpose, and they are now also used for ML neural networks and mining cryptocurrencies.
- using GPU like this (ie. not triangle drawing) is known as General Purpose GPU (GPGPU)

GPUs are optimized for throughput at the cost of latency
- GPUs need to sustain 3.3GB/s just for texture samples for a shader running at 1280x720 resolution
- this level of throughput is only possible if the memory of the GPU is very tightly integrated with the cores

a GPU can be a completely separate card with its own memory chip, so you control it via a so-called command buffer (or command queue), which is a chunk of memory that contains encoded commands for the GPU to execute.
- The command buffer is also the hook for the [[driver|os.kernel.driver]] or [[operating system|os]] to let multiple applications use the GPU without them interfering with each other. When you queue up your commands, the abstraction layers below will inject additional commands into the queue to save the previous program’s state and restore your program’s state so that it feels like no one else is using the GPU.

### GPU Cores
GPUs have many cores (from 100's to 1000's) that allow for a high degree of parallelization.
- however, each core is not as independent as the ones we'd find in a [[CPU|hardware.cpu]]

GPU cores are grouped hierarchically
