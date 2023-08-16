# osdi
## 2023

- [AlpaServe: Statistical Multiplexing with Model Parallelism for Deep Learning Serving](https://www.usenix.org/conference/osdi23/presentation/li-zhouhan)
    * 模型并行同时用于模型服务(Deep Learning Serving)
- [Welder: Scheduling Deep Learning Memory Access via Tile-graph](https://www.usenix.org/conference/osdi23/presentation/shi)
    * 基于计算图的调度器, 优化内存瓶颈
- [Effectively Scheduling Computational Graphs of Deep Neural Networks toward Their Domain-Specific Accelerators](https://www.usenix.org/conference/osdi23/presentation/zhao)
    * 基于计算图的调度器, 感知底层硬件
- [Hydro: Surrogate-Based Hyperparameter Tuning Service in Datacenters](https://www.usenix.org/conference/osdi23/presentation/hu)



- misc
    * [Cocktailer: Analyzing and Optimizing Dynamic Control Flow in Deep Learning](https://www.usenix.org/conference/osdi23/presentation/zhang-chen)
        + 编译器
    * [Flor: An Open High Performance RDMA Framework Over Heterogeneous RNICs](https://www.usenix.org/conference/osdi23/presentation/li-qiang)
        + NIC异构


## 2022

- [Walle: An End-to-End, General-Purpose, and Large-Scale Production System for Device-Cloud Collaborative Machine Learning](https://www.usenix.org/conference/osdi22/presentation/lv)
    * 适用云环境的, 云环境感知的ML
- [x] [Unity: Accelerating DNN Training Through Joint Optimization of Algebraic Transformations and Parallelization](https://www.usenix.org/conference/osdi22/presentation/unger)
    * 计算图优化, 自动并行, 自动生成并行代码
- [Orca: A Distributed Serving System for Transformer-Based Generative Models](https://www.usenix.org/conference/osdi22/presentation/yu)
    * 以iteration为单位调度
- [x] [Microsecond-scale Preemption for Concurrent GPU-accelerated DNN Inferences](https://www.usenix.org/conference/osdi22/presentation/han)
    * GPU没有抢占导致高latency, IPADS出品, 魔改GPU驱动以支持抢占
- [Alpa: Automating Inter- and Intra-Operator Parallelism for Distributed Deep Learning](https://www.usenix.org/conference/osdi22/presentation/zheng-lianmin)
    * 自动并行
- [Ekko: A Large-Scale Deep Learning Recommender System with Low-Latency Model Update](https://www.usenix.org/conference/osdi22/presentation/sima)
    * 现在的推荐系统模型更新得慢(延迟高), 原因在于分布式的同步。提出一种低延迟的更新方法


## 2021

- [Pollux: Co-adaptive Cluster Scheduling for Goodput-Optimized Deep Learning](https://www.usenix.org/conference/osdi21/presentation/qiao)

- misc
    * [Privacy Budget Scheduling](https://www.usenix.org/conference/osdi21/presentation/luo)


## 2020

- [Serving DNNs like Clockwork: Performance Predictability from the Bottom Up](https://www.usenix.org/conference/osdi20/presentation/gujarati)
- [x] [A Unified Architecture for Accelerating Distributed DNN Training in Heterogeneous GPU/CPU Clusters](https://www.usenix.org/conference/osdi20/presentation/jiang)
    - byteps
- [Heterogeneity-Aware Cluster Scheduling Policies for Deep Learning Workloads](https://www.usenix.org/conference/osdi20/presentation/narayanan-deepak)
    * 异构感知的调度器
- [PipeSwitch: Fast Pipelined Context Switching for Deep Learning Applications](https://www.usenix.org/conference/osdi20/presentation/bai)
    * 分时共享 It allows multiple DL applications to time-share the same GPU with the entire GPU memory and millisecond-scale switching overhead.
    * achieves near 100% GPU utilization
- [Ansor: Generating High-Performance Tensor Programs for Deep Learning](https://www.usenix.org/conference/osdi20/presentation/zheng)
    * 算子优化


- GPU share
    * [HiveD: Sharing a GPU Cluster for Deep Learning with Guarantees](https://www.usenix.org/conference/osdi20/presentation/zhao-hanyu)
    * [AntMan: Dynamic Scaling on GPU Clusters for Deep Learning](https://www.usenix.org/conference/osdi20/presentation/xiao)




# sosp
## 2023


## 2021

- [HEALER: Relation Learning Guided Kernel Fuzzing](https://dl.acm.org/doi/10.1145/3477132.3483547)
    * 算子融合
- [Gradient Compression Supercharged High-Performance Data Parallel DNN Training](https://dl.acm.org/doi/10.1145/3477132.3483553)
    * 压缩, ustc, licheng

- misc
    * [LineFS: Efficient SmartNIC Offload of a Distributed File System with Pipeline Parallelism](https://dl.acm.org/doi/10.1145/3477132.3483565)


# eurosys
## 2023

- SiloD: A Co-design of Caching and Scheduling for Deep Learning Clusters
    * 调度缓存协同设计
- Pocket: ML Serving from the Edge
- MariusGNN: Resource-Efficient Out-of-Core Training of Graph Neural Networks
    * offload
- Lyra: Elastic Scheduling for Deep Learning Clusters
- Egeria: Efficient DNN Training with Knowledge-Guided Layer Freezing
    * 通信草图?
- Hi-Speed DNN Training with Espresso: Unleashing the Full Potential of Gradient Compression with Near-Optimal Usage Strategies
    * 压缩



## 2022

- GNNLab: A Factored System for Sample-based GNN Training over GPUs
- Out-Of-Order BackProp: An Effective Scheduling Technique for Deep Learning
- Varuna: Scalable, Low-cost Training of Massive Deep Learning Models


## 2021

- [DGCL: An Efficient Communication Library for Distributed GNN Training](https://github.com/czkkkkkk/gccl)


- misc
    * [Tahoe: Tree Structure-Aware High Performance Inference Engine for Decision Tree Ensemble on GPU](https://github.com/zhen-xie/Tahoe)
        + 决策树, 推理

## 2020

- Balancing Efficiency and Fairness in Heterogeneous GPU Clusters for Deep Learning


# hpca
## 2023

- Logical/Physical Topology-Aware Collective Communication in Deep Learning Training
- MPress: Democratizing Billion-Scale Model Training on Multi-GPU Servers via Memory-Saving Inter-Operator Parallelism
    * ustc, licheng
- ISOSceles: Accelerating Sparse CNNs through Inter-Layer Pipelining
- Exploiting Compressed-Sparse Features in Deep Graph Convolutional Network Accelerators
- INCA: Input-stationary Dataflow at Outside-the-box Thinking about Deep Learning Accelerators
- DeFiNES: Enabling Fast Exploration of the Depth-first Scheduling Space for DNN Accelerators through Analytical Modeling
- CEGMA: Coordinated Elastic Graph Matching Acceleration for Graph Matching Networks
- OptimStore: In-Storage Optimization of Large Scale DNNs with On-Die Processing
- Chimera: An Analytical Optimizing Framework for Effective Compute-intensive Operators Fusion
    * 算子融合
- Tensor Movement Orchestration In Multi-GPU Training Systems

- PhotoFourier: A Photonic Joint Transform Correlator-Based Neural Network Accelerator

## 2022

- Hercules: Heterogeneity-Aware Inference Serving for At-Scale Personalized Recommendation


- misc
    * ScalaGraph: A Scalable Accelerator for Massively Parallel Graph Processing

## 2021

- Heterogeneous Dataflow Accelerators for Multi-DNN Workloads
- Efficient Tensor Migration and Allocation on Heterogeneous Memory Systems for Deep Learning
- FuseKNA: Fused Kernel Convolution based Accelerator for Deep Neural Networks
    * 算子优化



## 2020


# fast
## 2023

- [GL-Cache: Group-level learning for efficient and high-performance caching](https://www.usenix.org/conference/fast23/presentation/yang-juncheng)
    * 缓存感知
- [SHADE: Enable Fundamental Cacheability for Distributed Deep Learning Training](https://www.usenix.org/conference/fast23/presentation/khan)
    * 缓存感知

## 2022

- [Hardware/Software Co-Programmable Framework for Computational SSDs to Accelerate Deep Learning Service on Large-Scale Graphs](https://www.usenix.org/conference/fast22/presentation/kwon)
    * 软硬协同, irregular preprocessing不好, 最好在实际存储位置执行, 类似NUMA感知等, 减少remote的访问

- misc
    * [Practicably Boosting the Processing Performance of BFS-like Algorithms on Semi-External Graph System via I/O-Efficient Graph Ordering](https://www.usenix.org/conference/fast22/presentation/yang)

## 2021

- [FlashNeuron: SSD-Enabled Large-Batch Training of Very Deep Neural Networks](https://www.usenix.org/conference/fast21/presentation/bae)
    * 数据从GPU换出到SSD
    * batch size可放大12~14x


