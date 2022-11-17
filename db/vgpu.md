- TODO: rcuda lecture
    * http://www.rcuda.net/pub/rCUDA_isc18.pdf

## An Economy-Oriented GPU Virtualization with Dynamic and Adaptive Oversubscription. 2022

- 现代GPU虚拟化都干啥
- gOver strategy on Intel GVT-g
- 不同场景QoS的评判标志不同, 这里举例FPS来衡量QoS(云游戏场景), 作为调度依据
- 时分复用
- 使用`LD_PRELOAD`技巧来截取FPS信息
- Economy-Oriented: 从云厂商的角度出发, 看看怎么榨干显卡, 因为存在很多oversubscribe的情况

## Transparent I/O-Aware GPU Virtualization for Efficient Resource Consolidation

- API remoting
- a distributed I/O forwarding mechanism, 缓解GPU-CPU和网络带宽的gap
- 使用`LD_PRELOAD`技巧截获
- consolidation的方案: 如何未处于不同节点的app服务
- 设备管理: TODO:

## GaiaGPU: Sharing GPUs in Container Clouds

- 基于容器技术的gpu虚拟化
- 给出了**THE INTERCEPTED CUDA DRIVER APIS**
- **弹性资源申请**: 若vGPU空闲可以给实例分配多于其配置的资源, 否则回收保证人人不超配置
- TODO:

## LoGV: Low-overhead GPGPU Virtualization

- 内存分配和映射: 劫持所有内存申请相关的API
- **Command Submission**: Channel等于内存映射, 通过channel提交, 几个channel几个"并发"
    * 但目前没在时间层面做公平
    * 是我的话我可以直接做mmap然后这段区域的前面地址做metadata
- 迁移: 
    * unmap channel, 转而map内存中的一个存储器后面再从中恢复
- TODO:

## vCUDA: GPU-Accelerated High-Performance Computing in Virtual Machines

- 劫CUDA runtime API, 为什么劫(截哪些), 怎么劫
- TODO:

## FairGV

- 通过检测资源分配的函数来管理内存资源, 如cudaMalloc() and cudaFree() in CUDA
- TODO:

## GViM
- xen下的API remoting
- TODO:

## GPU Virtualization and Scheduling Methods: A Comprehensive Survey
- TODO:

## QCUDA

- API remoting
- windows base
- TODO

## RCUDA

- 第一个提出的
- API remoting
- client - server
- server处理新起**进程**, 保证一个挂掉时不影响其他
- 虚拟设备没有实现zero-copy, 但在rcuda virtio实现了
- TODO: 看看mrcude, github上的rcuda迁移方案
- TODO: 怎么处理stream?? Figure 2

## Pegasus: Coordinated Scheduling for Virtualized Accelerator-based Systems

- 提供一种在异构核上统一资源使用的模型
- 不该把专用设备当作第二公民: driver-based
