---
layout: post
title: "Apollo自动驾驶入门课程(五)预测"
date: 2022-03-08 23:39:56
tags:	- Automation
categories: []
---

## 预测

> 无人车需要为环境中所有其他物体做出预测，这些共同形成了在一段时间内的预测路径。在每一个时间段内，为每一辆汽车重新计算预测他们新生成的路径，这些预测路径为无人车在规划阶段做出决策提供了必要信息。

## 快速回忆

1. 预测路径的基本要求有哪些？
2. 有哪些预测方式？

<!-- More -->

## 预测路径的要求

预测路径需要具备**实时性**和**准确性**两大要求。

- 实时性：指算法的延迟越短越好；
- 准确性；
- 能够学习新的行为：开发出每种场景的静态模型是不可能完成的任务，所以要求预测模块能够学习新的行为。

## 预测方式

预测方式主要有基于**模型的预测**与**数据驱动预测**。

![](predictmethods.jpg)

### 基于车道序列的预测

### 障碍物状态

### 预测目标车道

### RNN预测目标车道

### 轨道生成



## 参考资料

1. [优达学城-无人驾驶第一课：从Apollo起步](https://apollo.auto/devcenter/coursevideo_cn.html)
2. [Apollo自动驾驶入门课程第⑥讲 — 预测](https://mp.weixin.qq.com/s?__biz=MzI1NjkxOTMyNQ==&mid=2247485361&idx=1&sn=a9d4e10b0ae01530944784afe6825fe2&chksm=ea1e15c3dd699cd56417f27c16f1390fc9fafddae64a9b0ac4e32afeb85ef4b9989a3eac5046&scene=21#wechat_redirect)

