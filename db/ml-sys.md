# Note about ml-sys papers

## SwapAdvisor: Pushing Deep Learning Beyond the GPU Memory Limit via Smart Swapping

- 通过计算图指导swap
- 工作: opt scheduling, memory allocation, and swap planning.
    * 用户可定制的搜索方法
    * opt schedule: 计算图有多个分支, 怎么调度
    * memory alloc: 各种size都有, 怎么缓存
- adopt Genetic Algorithm (GA for short) for NP-hard combinatorial problems [28, 39] and scheduling in parallel systems
- 搜索跑在真实框架太慢的，所以**跑在模拟器上**
    * 模拟器不用真正执行就能返回一个开销时间


## byteps paper (scheduler): A Generic Communication Scheduler for Distributed DNN Training Acceleration

- 目前框架都是重复造轮子, PS的实现都不同
- 两个问题
    * 调度的顺序如何和原本实现一致
        + Dependency Proxy
    * 有些框架存在barrier抽象
        + layer-wise out-of-engine dependencies
- 自动系统参数调整: 传输的块多大
- 传输顺序调整，可以不FIFO
    * 怎么做到不FIFO的? 网络stack怎么处理
        * credit base
- 接口实现: 加中间层(Proxy), 封装原始API, 从而管理
- 调度: credit(滑动窗口), 控制最小不可调整顺序单位
    * 且支持自动调整

## byteps paper (ps itself): A Unified Architecture for Accelerating Distributed DNN Training in Heterogeneous GPU/CPU Clusters

> GPU CPU NIC异构的利用率。处理GPU外还希望利用好CPU和带宽资源

- GPU数量固定的情况下也尽可能高的利用CPU
- Summation Service(SS)抽象
    * sum等cpu强的用cpu跑
    * run on both CPU and GPU machine
    * SS功能简单: sum up and send back
    * 任务分配分析: SS4.1.1 TODO
- 额外考虑的跨pci switch子系统的情况
    * "hierarchical all-reduce"再优化

- 原PS设计的瓶颈: SS6.2 RDMA performance



## mpress paper

- key obs: vram使用率不均, 因此可以人为D2D swap
    * pipeline流动的时候vram动态变化的嘛
- hierachy: GPU -> Other CPU -> CPU
- 3 mem saving: D2D, D2H, recompute
    * D2D tech
        + Data striping: 1 to N swap, transmit them in parallel through disjoint links
            + metadata table to track
        + Device mapping: 灵活映射pipeline, 使得pipeline所在GPU的邻居尽可能多有空闲内存
            + **思路类似罗宾汉哈希降低hash冲突**


## TACCL paper

- 用户可以给出**通信草图**指导
- TACCL's schedules can be run by registering TACCL-EF files **using the MSCCL tool stack**.
- MSCCL -> Nccl

## Breaking the Computation and Communication Abstraction Barrier in Distributed Machine LearningWorkloads

> 分层直接/间接导致性能问题, 但想要修改和优化就得知道底层细节和修改底层。
>
> CoCoNet, a language to describe distributed machine learning workloads and optimize them across computation and communication boundary.

- **通信的时候做计算: 流水线, 每次传输一小个chunk就可以计算了, 如此往复**
- 提供一种IR来编写计算和通信, 通过编译器自动生成目标代码

- 怎么做到打破抽象的? 根据什么原理融合?
    * 一点传一点算: 算传overlap

- DSL
    * 扩展tensor概念, 新增layout的概念: sliced, replicated, and local 三种layout
        + sliced tensor is equally distributed among all nodes in a group along a specified dimension **with RANK identifying** the slice for that process.
        + replicated, across all ranks in a group where it has the same value on each rank and it does not have a rank identifier. (没有标识的sliced). 如bias等
        + local tensor has same shape on all ranks but different values on all ranks
    * 操作抽象(计算图): local computations和cross rank communication
    * 混合集合通信操作: TODO: S5.2
    * Overlapping Operations: S5.3
- **Coconet transformations**: 对所写DSL做优化(指令重排等)
    * split 分原语
        + AllReduce -> RS-AG: AllReduce into a ReduceScatter to produce a sliced tensor and an AllGather on the sliced tensor to return a replicated tensor.
    * reorder 重排
        + (scD , scOut , agOut ) = reorder (d, out , agSum )
    * fusing 重组: fuse multiple computations and communications in a single operation
        + Computation Fuse: 多个操作合并成一个
        + AllReduce Fuse: fuses a series of ReduceScatter, sliced computations, and AllGather operations in a single FusedAllReduce
    * overlapping 流水: to overlap a series of **producer-consumer operations** to utilize multiple resources of hardware simultaneously


## Overlap Communication with Dependent Computation via Decomposition in Large Deep Learning Models

- 和coconet的区别: coconet相当于算子拆分然后overlap, 这个相当于流程拆分然后重组
- 一种重组思路: 交叉
    * P1sum(a1)的时候异步发a0给P2
- ccl流水线


## sccl(Synthesizing Optimal Collective Algorithms)

> scclang自动生成通信算法代码

- bw optimize: 降链路拥堵
    * ring algo
- latency optimize: 增加并行
    * Pareto-optimal with respect to the class of k-synchronous algorithms
- push比pull要快10%
- 大input size的时候还是nccl好，但是可以定制一个专门处理大input size的sccl??


## Microsecond-scale Preemption for Concurrent GPU-accelerated DNN Inferences

> DNN推理系统, [reef](https://github.com/66RING/reef-artifacts)

- cool
    * code inject
    * 改写GPU驱动

- 实时任务(called real-time(RT) task in this paper)
- 非实时任务(called best-effort(BE) task in this paper)

1. **DNN推理系统, 幂等性, 所以可以基于重启kernel实现抢占**
    - reset-based preemption scheme
2. kernel padding, 因为DNN任务是确定且可预测
    - a dynamic kernel padding mechanism

- 挑战
    * 目前没有给GPU的抢占调度
    * 即要实时又不要饥饿(work conserving)

如何实现

- 实时任务到来立刻抢占
- best-effort task则利用实时任务剩余的资源
- 怎么启动, 怎么kill, 怎么restore
    * 怎么kill: 
        + host queue: 有API可以控制
        + device queue: lazy eviction
            + 在kernel前段插入代码, 检测是否被eviction, 到cu后就会自动停止
            + 抢占太慢问题: 频繁内存回收, CU fetch开销(fetch后立刻stop听浪费的)
                + 后台GC, 讲一个flag指针置null
                + device queue 容量小点, 保证可以及时挑战(ref byteps), 
                    + **牺牲一点execution time**, switch 频率高点
                    + CPU占用提升
        + **compute unit(kill running kernel)**, 根本没有API
            + **直接修改GPU 驱动(AMD yes)**, 思路: 关掉CPU process, GPU process回释放那一定有方法控制
    * 怎么restore
        + 存在重新加载kernel开销问题, 且DNN 一般有一大堆kernel
        + 解决方案类似device queue, 每次抢占len(device queue)个kernel


## flash attention

> 主打io aware, 快1.5~3x

- 局限性
    * 需要重写cuda kernel, 且cuda kernel可能不可跨框架移植
        + 可以高层语言写(作为接口), 通过编译器自动生成cuda代码
    * io aware在更多地方(除了attention), 虽然attention是最内存密集的
    * 多GPU io aware, 当前是单GPU
        + 又会收到很多并行等因素的影响

传统attention: 

1. S = QK, read QK, store S
2. P = softmax(S), read S, store P
3. O = PV, read PV, store O

- 方法: 尽量使用GPU的SRAM, ref: 非常详细且通俗易懂的[FlashAttention简记](https://zhuanlan.zhihu.com/p/582606847)
    1. tiling: 输入拆分成多块, 然后多轮计算, softmax的公式拆分成增量的形式
    2. recompute: 反向传播不存储注意力矩阵(S, P)
        - 存储一个softmax的归一化因子, 可以用来重算这两个矩阵. 
        - 怎么做到的? 论文S3.1


## On Optimizing the Communication of Model Parallelism

> overlap + loadbalance

- **Figure 3.**, 策略分析: T(A, B)表示从一个设备发到A个设备(每个设备B张卡)的时间
    * 一个一个发送: T(A, B) = ABt
    * 分B part, 分别发送到节点中的设备, 然后节点内部all gather
    * Send/recv + global all-gather
    * Broadcast
    * 本质: inter传部分, 再高速intra exchange, 再将这两个阶段overlap
- 负载均衡TODO:??
    * baseline: 全局指定一个固定传输顺序
    * load balance only: 贪心, 对任务的执行时间排序后，将任务分发给对应host
    * DFS剪枝: 全局给个全任务的排序, 然后任务Si的开始时间设置为 
    * TODO
- overlap key
    * observation: 计算i, i传输i+1, 计算i+1, 相互依赖不可overlap
        + 怎么个不可overlap法? 
            + 传输i+1的时候无所事事, e.g. Fig4: (a) mesh1要算fwd4, 但要先等fwd4接收完成
    * eager-f1b1:
        + 传输的时候计算其他batch的任务
        + 但相当于同时保存多个任务的数据, 内存开销增大
    * overlap backwrad: bw拆分成激活函数的梯度计算和参数的梯度计算, 两者没有依赖关系, 可以在这两个阶段之间插入任务


## OPTIMIZING DNN COMPUTATION WITH RELAXED GRAPH SUBSTITUTIONS(Metaflow)

> 非常详细, 非常适合初学者

> 传统图优化的贪心找"严格优", 这里想不要那么"严格优"行不行(relax), 更多可能性后可以带来整体更有的可能

> We introduce a backtracking search algorithm over a set of relaxed graph substitutions to find optimized networks
> and use a flow-based graph split algorithm to recursively split a computation graph into smaller subgraphs to
> allow efficient search

- 优化的示例: 多opt合一, 减少访存和kernel launch

搞清楚四个实现: 代价模型, 回溯算法, 图分割, 子图替换

核心函数: `optimize_graph`


## OSP: Overlapping Computation and Communication in Parameter Server for Fast Machine Learning

> PS机制: 在每个worker中计算梯度, 然后在ps中同步回传更新的参数
>
> Figure 3

- 对ps的优化, async但不降低loss趋势
- worker不在一push一pull一更新了, **而是可以自己先本地更新, 本地再计算**, 达到一定阈值后再push/pull
    * 怎么做到自己本地更新的? 如果可以本地更新还要ps干嘛
- result: **加速好 loss好**, 图4, 图5, 图6

idea: vblock(虚拟块, 分页)为iter单位


## Overlapping Communication With Computation in Parameter Server for Scalable DLTraining

> 分页以overlap, 但页面大小不固定更灵活**从而刚好用满等待的时间**

1. 采用通信和计算的时间
2. 根据采用结果用ai算法决定分片的大小策略


