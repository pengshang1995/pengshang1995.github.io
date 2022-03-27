---
title: Go
date: 2022-03-26 16:50:16
---

# Go

#### Go协程简单用法
对于IO密集型计算可以采用go协程进行处理，使用这三个协程的操作，同时处理可以有效降低耗时。


#### go学习之- cas的理解
https://blog.csdn.net/chenxun_2010/article/details/103598252


#### make和new的区别
1. new 和 make 都用于分配内存；
2. new 和 make 都是在堆上分配内存；
3. new 对指针类型分配内存，返回值是分配类型的指针，new不能直接对 slice 、map、channel 分配内存；
4. make 仅用于 slice、map和 channel 的初始化，返回值为类型本身，而不是指针；

#### 