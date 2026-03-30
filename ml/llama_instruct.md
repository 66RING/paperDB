---
title: Llama指令模板
author: 66RING
date: 2024-06-08
tags: 
- machine learning
mathjax: true
---

# Llama指令模板

> Llama Prompt Template

测试模型效果的时候有时候会出现模型拒绝回答的情况，有可能就是prompt的问题(虽然不是很有用)。

这里记录llama2, llama3使用时prompt的cheat sheet。llama2和llama3的prompt指令有所不同

- [llama2 prompt template](https://llama.meta.com/docs/model-cards-and-prompt-formats/meta-llama-2/)
- [llama3 prompt template](https://llama.meta.com/docs/model-cards-and-prompt-formats/meta-llama-3/)

## Llama2

- `<s></s>`用于分割多轮消息
- `[INST][/INST]`用于包裹用户输入(system promtp + user input), 之后就是模型输出
- `<<SYS>><</SYS>>`用于包裹system prompt

一个多轮对话的例子: 多轮对话就有多个`<s></s>`, 其中最后一个`<s>`是没有`</s>`的来让模型补全。

```
<s>[INST] <<SYS>>
{{ system_prompt }}
<</SYS>>

{{ user_message_1 }} [/INST] {{ model_answer_1 }} </s>
<s>[INST] {{ user_message_2 }} [/INST]
```

## llama3

- `<|begin_of_text|>`用于标记prompt的开始
- `<|start_header_id|>{role}<|end_header_id|>`标记prompt的类型, 之后就是不同prompt
    * role可以是system, user, assistant
    * 使用user来输入用户输入, 使用assistant才能让模型回答
- `<|eot_id|>`用于标记一轮输入的结束, 这里和llama2`<s><\s>`的区别在与llama3的一轮要么是用户输入, 要么是模型输出
    * 相当于`<|eot_id|>`是数组的分隔符, 数组的每个元素是`{header, message}`


一个多轮对话的例子:

```
<|begin_of_text|><|start_header_id|>system<|end_header_id|>

You are a helpful AI assistant for travel tips and recommendations<|eot_id|><|start_header_id|>user<|end_header_id|>

What is France's capital?<|eot_id|><|start_header_id|>assistant<|end_header_id|>

Bonjour! The capital of France is Paris!<|eot_id|><|start_header_id|>user<|end_header_id|>

What can I do there?<|eot_id|><|start_header_id|>assistant<|end_header_id|>

Paris, the City of Light, offers a romantic getaway with must-see attractions like the Eiffel Tower and Louvre Museum, romantic experiences like river cruises and charming neighborhoods, and delicious food and drink options, with helpful tips for making the most of your trip.<|eot_id|><|start_header_id|>user<|end_header_id|>

Give me a detailed list of the attractions I should visit, and time it takes in each one, to plan my trip accordingly.<|eot_id|><|start_header_id|>assistant<|end_header_id|>
```
