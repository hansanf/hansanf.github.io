---
title: atomic
date: 2022-06-29 10:55:35
tags: 
categories: C++
---

### 内存模型
内存模型：指令执行顺序的模型

现代的处理器基本是并发式处理机器指令的,在一个cpu 时钟 issue 多条指令。

指令若总是严格按照书写顺序执行的，则称这样的内存模型为强顺序的(strong ordered),按照一定规则允许乱序的，称为弱顺序的(weak ordered)。

内存模型详细介绍：

- [https://www.zhihu.com/question/24301047](https://www.zhihu.com/question/24301047)

- [https://zhuanlan.zhihu.com/p/264975254](https://www.zhihu.com/question/24301047)


### C++ atomix 内存顺序

根据允许指令的混乱程度，C++为原子操作指定了内存顺序（memory_order），其包含6种：
- momory_order_relaxed,
- memory_order_consume,
- memory_order_acquire,
- memory_order_release,
- memory_order_acq_rel,
- memory_order_seq_cst.

![在这里插入图片描述](https://img-blog.csdnimg.cn/56a139b6a7184bd5af35c4df4610a9aa.png)

虽然共有 6 个选项,但它们表示的是三种内存模型: 

**sequential consistent**: memory_order_seq_cst

如果对于一个原子变量的操作都是顺序一致的，那么多线程程序的行为就像是这些操作都以一种特定顺序被单线程程序执行。相当于所有的线程都用同一份内存，对同一个变量保持时刻可见

[使用示例](https://www.zhihu.com/question/24301047)

**relaxed**: memory_order_seq_cst

在同一线程内对同一变量的操作仍保持happens-before关系，但这与别的线程无关。 在 relaxed ordering 中唯一的要求是在同一线程中，对同一原子变量的访问不可以被重排。

**acquire release**: memory_order_consume, memory_order_acquire, 
memory_order_release, memory_order_acq_rel

在这种序列模型下,原子 load 操作是 acquire 操作(memory_order_acquire), 原子 store 操作是release操作(memory_order_release).

原子read_modify_write操作(如fetch_add(), exchange())可以是 acquire, release 或两者皆是(memory_order_acq_rel). 

同步是成对出现的,它出现在一个进行 release 操作和一个进行 acquire 操作的线程间。

### C++ atomic 用法



### atomic 和 mutex 区别
atomic 可以实现数据结构的无锁设计，为什么还要用 mutex 呢？

std::atomic原子操作，主要是保护一个变量，互斥量的保护范围更大，可以一段代码或一个变量。
