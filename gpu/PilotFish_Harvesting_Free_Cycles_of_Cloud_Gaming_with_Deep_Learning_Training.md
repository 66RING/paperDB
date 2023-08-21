# PilotFish Harvesting Free Cycles of Cloud Gaming with Deep Learning Training

> low GPU utilization problem of cloud gaming

game is "realtime sansitive" and unpredictable, so how to use those free cycle?

- make full use of gpu resources spec in cloud gaming workload
    * so Pilotfish can harvest free GPU using DL 
    * co-locate DL with cloud gaming


### intro

- cloud gaming utilization just about 50%
    * so => co-locate multi workload in one GPU
    * but the gaming work load is unpredictable and resource is unstable
    * and we find DL trainning fit in **since its predictable workload**(consist of iteration step) 

- analysis procedure(gaming pipline) and find out what can we do


### motivation

- low utilization problem
- nature idea: co-localte
    * but the randamness of game cause co-location performance down

- why DL
    1. huge demand
    2. predictable and find graint


### Overview

- make DL low priority
- co-design cloud gaming and DL
- frame-level monitor => hack graphic libraries
- if there is an idle period, schedule. exec prev.end - next.start
    * schedule base on: kernel duration predictor to provide the execution time of the computation kernels,
- **preempt**, without effect the game + low overhead checkpoint for DL job
    * TODO: about this low overhead checkpoint

### how obtain idle period

1. monitor the rendering of frame
    - current soft of tracing have large latency 
    - so we do not tracing, we **just hijact the lib**
2. insert notify command to start schedule
3. 60FPS workload so there is some tolerence


### scheduler

> figure 9

- **DL kernel is predictable**
- isRendering
- isNotRendering
    * free > job => ok


### Task executor

> preempt + allow for some time out. just like overcommit


- hard guarantee: immediately down
- soft guarantee: droped frame exceed


#### **low overhead pause and resume**

> allow loss of training progress

- multi-priority stream to send signal to DL with the hightest priority
- **share mem pool to save model**
    * copy from host to GPU at least take 30ms
* mitigating other resource: CPU, PCI, IO, mem, cache...
    + CPU: havn't say how to do. just inspire for a while
    + PCI bandwith: reservation for gaming
    + IO: IO isolation and IO priority
    + GPU mem and cache: only co-localet when all fit
        + cache: havn't see any impact
    + network: separate network, so there are no interference


### mechanisms of capture idle GPU periods and fine-grainded scheduling

### what previous work have done

- co-locate with statis profiling


### star

- preempt
- lightweight checkpoint
- no trace just hack the common lib
    * such as. insert some notify command
- AI help for tracing
- **low overhead pause and resume**
- **concern about other resource contention**
    * bucket theory, bottle neck


### all in all, the basic idea





### TODO

- figure 8




