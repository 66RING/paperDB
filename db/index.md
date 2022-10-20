- An Economy-Oriented GPU Virtualization with Dynamic and Adaptive Oversubscription. 2022
	* 现代GPU虚拟化都干啥
	* gOver strategy on Intel GVT-g
	* 不同场景QoS的评判标志不同, 这里举例FPS来衡量QoS(云游戏场景), 作为调度依据
	* 时分复用
	* 使用`LD_PRELOAD`技巧来截取FPS信息
	* Economy-Oriented: 从云厂商的角度出发, 看看怎么榨干显卡, 因为存在很多oversubscribe的情况
- Transparent I/O-Aware GPU Virtualization for Efficient Resource Consolidation
	* API remoting
	* a distributed I/O forwarding mechanism, 缓解GPU-CPU和网络带宽的gap
	* 使用`LD_PRELOAD`技巧截获
	* consolidation的方案: 如何未处于不同节点的app服务
	* 设备管理: TODO:
- GaiaGPU: Sharing GPUs in Container Clouds
	* 基于容器技术的gpu虚拟化
	* 给出了**THE INTERCEPTED CUDA DRIVER APIS**
	* **弹性资源申请**: 若vGPU空闲可以给实例分配多于其配置的资源, 否则回收保证人人不超配置
	* TODO:
- LoGV: Low-overhead GPGPU Virtualization
	* 内存分配和映射: 劫持所有内存申请相关的API
	* **Command Submission**: Channel等于内存映射, 通过channel提交, 几个channel几个"并发"
		+ 但目前没在时间层面做公平
		+ 是我的话我可以直接做mmap然后这段区域的前面地址做metadata
	* 迁移: 
		+ unmap channel, 转而map内存中的一个存储器后面再从中恢复
	* TODO:
- vCUDA: GPU-Accelerated High-Performance Computing in Virtual Machines
	* 劫CUDA runtime API, 为什么劫(截哪些), 怎么劫
	* TODO:
- FairGV
	* 通过检测资源分配的函数来管理内存资源, 如cudaMalloc() and cudaFree() in CUDA
	* TODO:
- GViM
	* xen下的API remoting
	* TODO:
- GPU Virtualization and Scheduling Methods: A Comprehensive Survey
	* TODO:
- QCUDA
	* API remoting
	* windows base
	* TODO
- DBOS
	* UNIX: "everything is file" => DBOS: "everyting is table"
	* 所谓DBOS是指利用DBMS的方法设计OS
		+ 状态可query
	* 内嵌多节点事务来解决可扩展性等问题
	* 微内核
	* 当前的简单serverless设计:
		+ 无复杂内存分配机制: 内存一次性分配
	* 方便的分布式多节点访问: 系统的任意状态可以通过SQL查询
