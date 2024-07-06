---
title: 基于叉乘求解直线系数-相交问题
author: melon
date: 2023-09-06 22:37:00 +0800
categories: [Math]
tags: [typography]
pin: true
math: true
mermaid: true
---

这篇博客中主要探讨下利用**点乘**和**叉乘**相关知识点求解**直线方程的系数**，以及**多条直线相交**问题；  

已知直线上两个点$X_1(x_1,y_1)$，$X_2(x_2,y_2)$，则
1. 求解直线方程$ax+by+c=0$的系数$(a,b,c)$：  
   通过点在直线上可得：
  
   $$\begin{cases}ax_1+by_1+c*1=0 \\ ax_2+by_2+c*1=0 \end{cases}$$
  
   观察上面两个式子，我们会发现点$X_1$和$X_2$对应的齐次坐标（向量）和直线方程的系数向量的点乘(DotProduct)为0，即向量之间垂直；结合叉乘(CrossProduct)的含义可知，直线方程的系数（向量）可被点$X_1$和点$X_2$对应的齐次坐标的叉乘结果来表示：
   
   $$\begin{bmatrix}a \\b \\c\end{bmatrix} =
   \begin{bmatrix}0 &-1 &y_1 \\1 &0 &-x_1 \\-y_1 &x_1 &0\end{bmatrix}
   \begin{bmatrix}x_2 \\y_2 \\1\end{bmatrix}
   =\begin{bmatrix}y_1-y_2 \\x_2-x_1 \\x_1y_2-x_2y_1\end{bmatrix}$$

2. 求解两个直线的交点$P(x,y)$：

   $$\begin{cases} a_1x+b_1y+c_1=0 \\ a_2x+b_2y+c_2=0 \end{cases}$$

   可知两个直线的叉乘对应于它们的交点（齐次坐标）：

   $$\begin{bmatrix}0 &-c_1 &b_1 \\c_1 &0 &-a_1 \\-b_1 &a_1 &0\end{bmatrix}\times
   \begin{bmatrix}a_2 \\b_2 \\c_2\end{bmatrix} = 
   \begin{bmatrix}b_1c_2-c_1b_2 \\a_2c_1-a_1c_2 \\a_1b_2-a_2b_1\end{bmatrix}$$ 

   需要将上式转换为齐次坐标形式，即坐标前两项除以第三项$(a_1b_2-a_2b_1)$，因此我们需要判断第三项值是否为零：  
   - 当第三项$(a_1b_2-a_2b_1)$为零，意味着两直线平行；  
   - 当第三项$(a_1b_2-a_2b_1)$不为零，可得交点$P(x,y)$：

   $$\begin{bmatrix}P \\1\end{bmatrix} = 
   \begin{bmatrix}(b_1c_2-c_1b_2)/(a_1b_2-a_2b_1) 
   \\(a_2c_1-a_1c_2)/(a_1b_2-a_2b_1) \\1\end{bmatrix}$$  

  ```python
  import numpy as np
  def intersectLines(s1:np.array, e1:np.array, s2:np.array, e2:np.array):
      s1, e1, s2, e2 = [np.append(p, 1) for p in [s1, e1, s2, e2]]
      kx, ky, k = np.cross(np.cross(s1, e1), np.cross(s2, e2))
      return ([kx/k, ky/k] if k != 0 else None)
  ```  

3. 求解多条直线的交点$P(x,y)$:  
  借助求解两条直线交点的知识点，以及[最小二乘原理](https://rubin2020.github.io/posts/Linear-Least-Squares-Question/)，我们可以求出多条直线的交点$P(x,y)$；


**参考**：  
1. [Intersection-of-Lines-GeeksforGeeks](https://www.geeksforgeeks.org/point-of-intersection-of-two-lines-formula/)  
2. [line-intersections-with-cross-products-Blog](https://imois.in/posts/line-intersections-with-cross-products/)  
3. [Line-LineIntersection-Wolfram](https://mathworld.wolfram.com/Line-LineIntersection.html)