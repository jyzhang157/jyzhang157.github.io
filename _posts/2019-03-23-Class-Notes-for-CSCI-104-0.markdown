---
layout: post
title:  "【Class Notes for CSCI 104】综述"
date:   2019-03-23 +0800
---
# 【Class Notes for CSCI 104】综述
> 主要记录一些在学习Data Strutures时的一些知识点  
> 课程来源：CSCI 104 in University of Southern California in Fall of 2013 “Data Structures and  Object-Oriented Design”

## 综述
不涉及基本的编程内容，如：
- Data types: int, String, float/double, struct
- Arithmetic
- Loops: for, while, do-while
- Pointers
- Functions and recursion
- Other: I/O and useful function
- OOP

写“好的”代码设计两个方面：
1. 对于我们不知道如何解决，或者如何足够快解决的问题，学习一些解决问题的概念和方法
2. 学习新的语言特性和编程概念，来帮助我们实现一些方便我们阅读、调试、拓展的代码

作为一名更好的程序员，需要把数据本身作为我们思考的重心，在写代码的同时要思考数据时如何传递和改变的

##
**The basic question of how to organize data to support the operations we need is the centerpiece of what you will learn in this class.**  
组织我们数据的最好方式大多数情况要视使用情况而定，我们需要思考的是我们将如何对我们的数据进行操作，从而设计合适的机构。比如：
- 我们是否需要频繁的搜索？
- 添加数据操作是否经常被执行？
- *short chunks* or *in occasional huge blocks*?
- 是否多次更改我们的数据结构？
- 我们访问数据时使用哪种查询方式？

> 比如我们有一个数据结构需要实现以下功能
>- The **add (key, value)** operation gets a key and a value. (In our case, they were both strings, but in general, they don’t have to be.) It creates an association between the key and value.
>- The **remove (key)** operation gets a key, and removes the corresponding pair.
>- The **lookup (key)** operation gets a key, and returns the corresponding value, assuming the key is in the structure.

对应的数据结构应当叫做*map*，或者叫做*dictionary*
## OOP
### 抽象数据类型(abstract data type)
> This combination of data and code, with a well-specified interface of functions to call

当我们确定使用抽象数据类型时，我们在编程过程中思考的，就是数据结构本身及其伴随的一些处理数据本身的功能（经过实际在对数据操作时，和传统的数据类型的操作形式是一致的），这就自然而然引入了OOP(object-oriented design)。
### 封装(encapsulation)
> shielding as much of the inside of an object as possible from the outside, so that it can be changed without negatively affecting the code around it.
> 这被称作数据的"walls"

封装实现了模块化的特性，使得我们可以更容易的对代码块单独的分析和测试

## 课程目标
1. 通过学习一些基本的或者高级的方法，实现高效的数据结构
2. 学习如何去抽象的思考代码，在代码设计中从“how”的思维分离处“what”的思维方式，并使用抽象数据累心来实现功能
3. 学习OOP和相关的用于实现oop的数据类型