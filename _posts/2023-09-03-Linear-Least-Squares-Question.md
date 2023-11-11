---
title: 线性最小二乘推导与实现
author: melon
date: 2023-09-03 20:33:00 +0800
categories: [Math]
tags: [typography]
pin: true
math: true
mermaid: true
---

给定多组特征值与其对应的观测量情况下，求解系数A使式1值最小:  

$$f(x) = \sum_{j=0}^{n-1}a_jx^{j} \tag0$$  

$$E_{LS} = \sum_{i}|\sum_{j=0}^{n-1}a_jx_i^{j} - y_i|^{2} = \sum_{i}|Ax_i - y_i|^{2} = \|XA - Y\|^{2} \tag1$$

其中A表示未知参数，向量模平方等价于向量內积，即  

$$E_{LS} = (XA - Y)^T(XA - Y)  \tag2$$

方式一：  

对A求微分，并根据微分与导数关系可得式3，式4，如下：  

$$dE = (XdA)^T(XA - Y) + (XA - Y)^T(XdA) \\=2(XA - Y)^T(XdA)\tag3 $$

$$dE = \frac{\delta E^T}{\delta A}dA \tag4$$

综合式3与式4，可得对A偏导如式5

$$\frac{\delta E}{\delta A} = 2((XA - Y)^TX)^T\tag5$$

最后，由于最小平方差和对应着偏导数为0，由式5可得系数A

$$A = (X^TX)^{-1}X^TY\tag6$$

方式二：

$$\begin{aligned}E_{LS} &= A^{T}X^{T}XA - Y^{T}XA - A^{T}X^{T}Y + Y^{T}Y \\
&= A^{T}X^{T}XA - 2A^{T}X^{T}Y + Y^{T}Y \end{aligned} \tag7$$  

$$ \frac{\delta E}{\delta A} = 2X^{T}XA - 2X^{T}Y = 0\tag8$$

$$ A = (X^{T}X)^{-1}X^{T}Y\tag9$$

