## abs

https://zhuanlan.zhihu.com/p/468732214

- 一般图计算框架
    * superstep
    * barrier: 同步点
    * iteration


- term
    * footprint -> 占用

## vGraph: Memory-Efficient Multicore Graph Processing for Traversal-Centric Algorithms

- a NUMA-aware, memory-efficient multicore graph processing system for **traversal-centric algorithms**
    * so我可以node-aware
- 引入预取, **work-stealing优化**
- state-of-the-art numa-aware
- 调查表明数据量不超过百万规模, 即大多单机可以解决
- 现代numa-aware system
    * 高预处理开销和高内存占用, 就为了优化局部性
    * 严重的内存放大
- vGraph的目标
    * 降低内存峰值占用以尽可能单机本地处理, 降低预处理开销
    * 做法: 不优化那么极致(step back), e.g. 编译原理
        + 压缩但不预处理: 预处理就引入了inter-numa。而压缩可以fit in, 进一步减少
- ⭐方便的编程接口
- 没有分布式支持
    * ⭐可以使用Compute Express Link (CXL) [8] technology.
- **work-stealing**: 数据迁移不如上下文迁移
- **pipeline算法**
    * TODO:!!!!!!!!!!, 搞清楚算法
- 调度, one multi-threaded process并不好调度
    * 刚好我在做N:M协程库
- 看看人家对算法的抽象: Graph Algorithm Applications
    * programming interface with two functions, namely, EdgeMap and VertexMap, 将操作F映射到点/边中, 每个点/边都可以独立计算
    * 计算件抽象: PageRank on vGraph
- 图处理的流程: 加载, 预处理, 执行
- 
- TODO: 博文++
- ref:
    * [24], [36], [52], [54], [11]
    * TODO:

## PageRank: Graph Processing Using Dataflow to Rank Web Pages According to Importance(Spark上应用PageRank)

- https://www.youtube.com/watch?v=meonLcN7LD4
    * https://www.youtube.com/watch?v=P8Kt6Abq_rM
    * 越多页面链接到的网页越importance。为了防止伪造importance, 我们要根据另一个important page的链接计算(所以有个迭代算法)
    * TODO:
- 更好的找到用户需要的网页, 根据重要性(importance)排序: To sum up and rank such many and diverse web pages is a big challenge 
- 处理大图的效率高于处理小图


## PowerGraph

https://zhuanlan.zhihu.com/p/38042241

## PageRank

迭代计算:

$$
PR(x) = \frac{\alpha}{N}  + (1 - \alpha) \Sigma{\frac{PR(y)}{out(y)}}
$$

## idea

- numa: roughly-equal shares
    * RAID???
- BSP: 整体同步并行计算模型 (Bulk synchronous parallel)
- GAS: 分布式图计算模型GAS（Gather-Apply-Scatter）详解
