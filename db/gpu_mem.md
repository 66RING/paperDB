In 2011, CUDA 4 introduced Unified Virtual Addressing (UVA) to provide a single virtual memory address
space for both CPU and GPU memory and enable pointers to be accessed from GPU code no matter
where in the system they reside, whether in GPU memory (on the same or a different GPU), CPU
memory, or on-chip shared memory.

##  GPUswap: Enabling Oversubscription of GPU Memory through Transparent Swapping

- 现代GPU视page fault为错误, 没有多余标志位, 导致没有操作空间(e.g. demand paging)
    * 好像现在(2022)gpu是可以page fault和demand paging了
    * Pascal及以上架构的GPU是可以处理页错误(Page Fault)的
- 有Gdev等基于软件调度的方式(切换kernel)但是开销大
- 比较详细的GPU内存管理的介绍
- 但是GPU的页表除了可以映射GPU的内存**还可以映射RAM**, 就是慢了点
- TODO: **维护多BO**
    * 什么是BO? 用户视角的BO?
- inside OS kernel, 从而获取透明的获取程序信息 -> SO we can do it in vm
- 组件
- 方法
    * 截获所有内存申请
        + 分页思想: **将申请的OB拆分为若干chunk, 以chunk为单位管理内存**
            + chuck size又可以做文章了

##  Memory Harvesting in Multi-GPU Systems with Hierarchical Unified Virtual Memory

- 主打邻居GPU的互连, 因为用专用线能更快
- 可以整和GPU, 邻居GPU和host ram
    * comprised of local GPU, spare memory of neighbor GPUs, and the host memory
- NVIDIA’s unified virtual memory (UVM) driver

## Mosaic: A GPU Memory Manager with Application-Transparent Support for Multiple Page Sizes

- key idea: 细粒度的transfer, 连续的物理页, 智能的合并和分离
- TODO: 
    * 66, 114
    * information about the GPU memory hierarchy can be found in [8, 10, 44, 46, 47, 48, 54, 86, 95, 115, 116].
    * demand paging [3, 60, 82, 92, 118]. I
- 现代gpu可以支持虚拟内存和demand paging等
- 地址翻译传输和缓存miss的trade-off: poor TLB stall TLP, **经典page size和TLB问题**
    * 有着强大的TLP(thread-level parallelism), 一次传输块越大并行读越高, 但缓存越容易失效
    * 缓存越失效, 由于木桶效应TLP的性能又大打折扣
- 研究发现可以应用"智能page size", 并且申请时尽量让虚存连续(降低拷贝开销)
    * a key observation: GPGPU app很多都是一次申请很多内存的, **TODO: 所以可以:**
        + 虚存连续那可以让他尽量物理内存连续
        + 同类申请放同类虚存区

## A Framework for Memory Oversubscription Management in Graphics Processing Units

- vm:  [7, 9, 10, 13, 19, 37, 76, 77, 96].
- 论点: when application working sets exceed physical memory capacity, the resulting data movement can cause great performance loss
- ⭐ ETC outperforms the state-of-the-art baseline by 60.4% 
- 具体问题具体分析: broadly categorize applications into regular and irregular a


## ⭐ Zorua: A Holistic Approach to Resource Virtualization in GPUs

- 一套完整的gpu虚拟化方法: propose to decouple an application’s resource specification from the available hardware resources by virtualizing 
- 目前我们的的编程架构基本都是: 在某个设备申请内存, 然后运行。paper想要解决这种静态分配, 解耦资源和具体设备
- Observation. 性能断崖: 资源超过GPU资源时。资源利用率: GPU可以大量并行, 资源利用率越高越能并行


## TODO: NVMMU: A Non-Volatile Memory Management Unit for Heterogeneous GPU-SSD Architectures

## Refs

- UVM: https://developer.nvidia.com/blog/maximizing-unified-memory-performance-cuda/
- contaxt: https://docs.nvidia.com/cuda/cuda-c-programming-guide/index.html#context
