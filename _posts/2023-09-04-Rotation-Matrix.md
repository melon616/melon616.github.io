---
title: 2D/3D旋转矩阵
author: melon
date: 2023-09-04 20:34:00 +0800
categories: [Math]
tags: [typography]
pin: true
math: true
mermaid: true
---

定位与建图中旋转变换主要是指以下两种类型：
* 同一坐标系下，向量的旋转变换；
* 坐标系的旋转变换，导致同一向量在变换前后坐标系中有不同表示； 

# 1. 2D旋转矩阵

如下图所示，二维平面坐标下，点$P$逆时针旋转$\theta$度后得到$P^{'}$；然后根据三角函数相关知识，可以将点$P$和$P^{'}$联系起来，构建出二维旋转矩阵，如下：

$$\begin{aligned}P^{'} &= \begin{bmatrix}rcos(\alpha+\theta) \\ rsin(\alpha+\theta)\end{bmatrix} \\&= \begin{bmatrix}cos\theta & -sin\theta\\ sin\theta & cos\theta\end{bmatrix}
\begin{bmatrix}rcos\alpha \\ rsin\alpha\end{bmatrix}\\&= \begin{bmatrix}cos\theta & -sin\theta\\ sin\theta & cos\theta\end{bmatrix}P\end{aligned}
$$

![](/images/2d-rotation-explanation.png){: data-action=”zoom”}
# 2. 3D旋转矩阵
参考坐标系下点$P$经过连续三次旋转后得到$P^{'}$，即第一次绕X轴旋转$\alpha$度，第二次绕Y轴旋转$\beta$度，第三次绕Z轴旋转$\gamma$，其旋转矩阵为$R_{zyx}$：

$$\begin{aligned}R_{zyx} &= R_zR_yR_x 
\\&=\begin{bmatrix}cos\gamma &-sin\gamma &0 \\sin\gamma &cos\gamma &0 \\0 &0 &1\end{bmatrix}
\begin{bmatrix}cos\beta &0 &sin\beta \\0 &1 &0 \\-sin\beta &0&cos\beta \end{bmatrix}
\begin{bmatrix}1 &0 &0 \\0 &cos\alpha &-sin\alpha \\0& sin\alpha &cos\alpha \end{bmatrix}
\\ &=\begin{bmatrix}cos\beta cos\gamma &sin\alpha sin\beta cos\gamma-\cos\alpha sin\gamma &cos\alpha sin\beta cos\gamma+sin\alpha sin\gamma \\cos\beta sin\gamma &sin\alpha{sin\beta}{sin\gamma} +cos\alpha{cos\gamma} &cos\alpha{sin\beta}{sin\gamma}-sin\alpha{cos\gamma} \\-sin\beta &sin\alpha{cos\beta} &cos\alpha{cos\beta} \end{bmatrix}
\end{aligned}$$

1). 绕X轴旋转

$$R_x = \begin{bmatrix}1 &0 &0 \\0 &cos\alpha &-sin\alpha \\0& sin\alpha &cos\alpha \end{bmatrix}$$

2). 绕Y轴旋转

$$R_y = \begin{bmatrix}cos\beta &0 &sin\beta \\0 &1 &0 \\-sin\beta &0&cos\beta \end{bmatrix}$$

3). 绕Z轴旋转

$$R_z = \begin{bmatrix}cos\gamma &-sin\gamma &0 \\sin\gamma &cos\gamma &0 \\0 &0 &1\end{bmatrix}$$
