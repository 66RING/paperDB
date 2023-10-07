---
title: Paged Attention
author: 66RING
date: 2023-08-22
tags: 
- machine learning
mathjax: true
---

# Paged Attention

> 一种推理框架 [vllm](https://vllm.ai/)

- abs
    1. 复用, prompt等
    2. 小块多batch
    3. 降低碎片

- main idea
    * OS一般分页, 让连续的kv可以存储到不连续的物理块中, kv分页到固定大小的block中
        + block相当于OS的page
        + tokens相当于OS的bytes
        + sequences相当于OS的process
        + **好处**
            1. 新token生成时动态申请, 按需申请. e.g. prompt一下申请大部分, 后续生成的token按需申请
            2. 减小内存浪费, 一次能batch的序列更多(因为一个序列只用一个block了)
            3. 更高效的内存共享, e.g. 使用同样的prompt时可以共享, arallel sampling and beam search等
                - 使用ref count和cow等机制
    * 需要一种自动fetch机制(没有MMU)
- observation
    * 产生的KV量大且捉摸不定
    * We find that existing systems waste 60% – 80% of memory due to fragmentation and over-reservation




