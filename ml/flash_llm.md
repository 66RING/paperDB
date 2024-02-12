---
title: Flash-LLM paper reading
author: 66RING
date: 2023-09-22
tags: 
- machine learning
- LLM
mathjax: true
---

# Flash-LLM

> [flash-llm](https://github.com/AlibabaResearch/flash-llm)

## abstract

- LLM需要大量内存
- 内存和swap有tradeoff
- 使用稀疏attention的方式让数据全存在内存中是中不错的方法
    * 尤其是unstructed sparity, 因为有用的信息不一定挨在一起
- 但是稀疏的矩阵(unstructed sparity)对GPU不友好无法充分利用硬件性能(连续地址)
- 这里提出"稀疏加载, 稠密计算"的机制, 并且利用多设备加速这一过程(流水线)


## intro

> MatMul is skinny

- 面对显存瓶颈, 数据的换入换出是一种方式, 但是这种方式极大的影响latency, 导致推理时间过长
- 另一种方式就是权重裁切(weight pruning), 这种方式在内存开销, 计算效率, 准确度的影响都比较好
- 但现代GPU对unstructed sparity的支持比较差
    * (没有页表, NVIDIA fuck you)
- Flash-LLM是一个支持unstructed sparity的库, 可以解决显存占用和计算效率的问题
- observation: 乘法操作其实是mem bound的(skinny很瘦, 形状的瘦的)
- 难点在于设计"稀疏加载, 稠密计算"


## background

TODO:





