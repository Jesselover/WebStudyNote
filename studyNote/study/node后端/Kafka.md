[Kafka 中文文档 - ApacheCN](https://kafka.apachecn.org/intro.html)

[【尚硅谷】Kafka3.x教程（从入门到调优，深入全面）_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1vr4y1677k/?spm_id_from=333.337.search-card.all.click&vd_source=9ff9288661d168a184d858583892913c)

版本 3.0.0

```mermaid
---
title "课程计划"
---
flowchart LR
入门 --> 外部系统集成 --> 生产调优 --> 源码解析
```

基础：Javase基础、Linux常用命令、idea开发工具

### 概述

**Kafka：**
1. 分布式 基于 发布、订阅者 的消息队列， 主要应用于大数据实时处理领域。
2. 分布式事件流平台：数据管道、流分析、数据集成、关键任务应用
3. 消息队列：消峰缓冲、解耦、异步通信
	1. 点对点模式 （1个消费者，用完即删）
	2. 发布/订阅模式 （多个消费者，订阅者主动删除）
	![[Pasted image 20231017113421.png]]

**Kafka 基础结构**

- Zookeeper：1. 服务器节点运行状态 2. leader相关信息 3.Kafka正在逐步抛弃Zookeeper
- leader挂掉后，follower在一定条件下成为leader

![[Pasted image 20231017114552.png]]