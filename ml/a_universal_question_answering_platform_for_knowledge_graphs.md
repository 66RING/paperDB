---
title: A Universal Question-Answering Platform for Knowledge Graphs
author: 66RING
date: 2023-09-18
tags: 
- machine learning
- Knowledge Graphs
mathjax: true
---

# Abstract

- 论点
    * 人们提出使用QA系统将问题转换成KG的查询语言
    * 但现有的QA系统只能适用于特定领域，需要昂贵的预处理和人工成本
    * 这里提出的KGQAn则将问题理解问题转换成文本生成问题, 生成中间表示, 再由连接器将中间表示传输到SPARQL(一种后端)查询
- observation
    * we are inspired by web search engines that resolve user questions independently of any particular web site
    * 我们可以令问题理解独立于KG: which is 中间表示
- idea
    * 问题理解问题转换成文本生成问题(利用大模型的总结能力)
    * 生成(entity a, relation, entity b)这样的triple


# Introduce

- SPARQL基本模式
    * 如, 问"Name the sea into which Danish Straits flows and has Kaliningrad as one of the city on the shore"
    * 那么, SPARQL查询语句大致就会表示成⟨?sea, flows, DanishStraits⟩和⟨?sea, cityOnShore, Kaliningrad⟩两个元组
- KGQAn相当于用户和SPARQL后端之间的翻译, 将用户输入的自然语言翻译成SPARQL query语言


# Implement

- question understanding
    * 使用seq2seq网络从输入中提取一系列元组用于表示相关的实体
    * so-called phrase graph pattern (PGP)
- linking
    * 使用JIT将PGP映射成query
- filtering
    * 输出的结果再经过过滤, 根据限制条件过滤
- QA系统基本也是这三个流程(table 1): 提取做中间表示, 映射中间表示到图的点和边, 滤除结果
    * 实例1, Fig 3
    * 实例2, Fig 4:
        1. 用户提问
        2. 提取triple做中间表示: 输入表示, 结果表示
        3. linking, 构建图, 组织PGP
        4. 向后端查询
        5. 查询结果根据PGP在过滤答案

## 4 KGQAN QUESTION UNDERSTANDING

使用seq2seq transformer将问题提炼成一个个(entity a, relation, entity b)三元组。(微调)训练一下text 2 triple的模式(Fig 5)。

其中训练的数据集是手动标注的1752条问题

## 5 KGQAN LINKING

## 6 KGQAN EXECUTION AND FILTRATION


