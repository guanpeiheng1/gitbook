---
title: Matlab的使用
categories: Skill
abbrlink: Matlab-Installation
date: 2020-04-12 15:52:29
tags:
---

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gegaff5icfj30sg0g040h.jpg)

Matlab相关使用技巧。

<!-- more -->

# 并行计算

打开并行计算后，计算机将在CPU的每个核心独立执行任务，从而提高执行效率。

## 打开与关闭

```
partool('local',4) // 打开
delete(gcp) // 关闭
```

## 编写代码

由于每个任务是独立执行的，因此每一个过程不能存在依赖关系。打开并行计算后，需要用parfor代替for。以以下代码为例进行说明。

```
A = [];
parfor i = 1:100
	for j = 1:100
		A(i,j) = 1; // 错误，因为A(i,j)需要依赖i
	end
end
```
