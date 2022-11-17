##  My VM is Lighter (and Safer) than your Container

- Xen based
- 容器存在的问题, 内核越来越复杂, 系统调用逐年增加, 容器的隔离性存在安全隐患
    * 容器是可以耗尽host资源的(cgroup), 而虚拟机会限制进程数, 防止ddos
- 而虚拟机只需要支持一套ABI即可(更小的计算可信基TCB)
- 实现目标: 快速实例, 多实例, 暂停/恢复
- 观察: **大多数是容器和VM都是用于运行单一app的**, 因此可以裁剪镜像和内存
    * unikernels, 但是定制化要求太高, 很多API可能缺失
    * **tinyx: 根据app自动构建最小linux镜像的工具**, 不那么定制化
- Tinyx: 
    * 镜像: 镜像文件系统挂载本地, 先在本地安装 + 一些裁剪操作
    * 内核: 修改config文件, e.g. 启动KVM/XEN, 关闭一些不必的选项
- 重新设计xen, e.g. page mapping, toolstack等
    * TODO: xen的xl/libxl

