
## DBOS

- UNIX: "everything is file" => DBOS: "everyting is table"
- 所谓DBOS是指利用DBMS的方法设计OS
    * 状态可query
- 内嵌多节点事务来解决可扩展性等问题
- 微内核
- 当前的简单serverless设计:
    * 无复杂内存分配机制: 内存一次性分配
- 方便的分布式多节点访问: 系统的任意状态可以通过SQL查询
