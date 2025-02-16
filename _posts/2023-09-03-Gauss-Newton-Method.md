---
title: 高斯牛顿法
author: melon
date: 2023-09-03 14:00:00 +0800
categories: [Math]
tags: [typography]
pin: true
math: true
mermaid: true
---

高斯牛顿法（Gauss-Newton Method）是一种用于求解非线性最小二乘问题的优化算法。它的原理是对目标函数进行线性化近似，将非线性问题转化为一系列线性问题，从而迭代求解，最终得到最优解。可调用Ceres，Gtsam等开源软件包进行问题求解。

## 1. 问题定义
非线性最小二乘问题的目标是找到一组参数 $\theta$，使得残差平方和最小化：

$$S(\theta) = \sum_{i=1}^n r_i(\theta)^2$$

其中，$r_i(\theta) = y_i - f(x_i, \theta)$ 是对应数据集中第 $i$ 个数据的残差项，$y_i$是观测量，$f(x_i, \theta)$ 是模型函数，$\theta$ 是待求解的参数。

## 2. 数学原理
高斯牛顿法的原理是对残差函数 $r_i(\theta)$ 进行一阶泰勒展开，将非线性问题线性化，然后通过求解线性最小二乘问题来更新参数。

### （1）残差的线性化

假设当前参数值为 $\theta_k$，我们对残差函数 $r_i(\theta)$ 在 $\theta_k$ 处进行一阶泰勒展开：

$$
r_i(\theta) \approx r_i(\theta_k) + J_i(\theta_k) (\theta - \theta_k)
$$

其中：
- $r_i(\theta_k)$ 是当前参数下的残差；
- $J_i(\theta_k)$ 是残差函数 $r_i(\theta)$ 对参数 $\theta$ 的雅可比矩阵（梯度向量）在 $\theta_k$ 处的值。

### （2）目标函数的近似

将残差的线性化形式代入目标函数 $S(\theta)$：

$$
S(\theta) \approx \sum_{i=1}^n \left[ r_i(\theta_k) + J_i(\theta_k) (\theta - \theta_k) \right]^2
$$

这是一个关于 $\theta$ 的二次函数，可以通过最小二乘法求解。

### （3）参数更新

将上述近似目标函数写成矩阵形式：

$$S(\theta) \approx \| r(\theta_k) + J(\theta_k) (\theta - \theta_k) \|^2$$

其中：
- $r(\theta_k)$ 是残差向量；
- $J(\theta_k)$ 是雅可比矩阵。  
对 $\theta$ 求导并令导数为零，可以得到参数更新公式：

$$\theta_{k+1} = \theta_k - (J^T J)^{-1} J^T r(\theta_k)$$

其中：
- $J^T J$ 是雅可比矩阵的转置与自身的乘积；
- $J^T r(\theta_k)$ 是梯度方向。

## 3. 优点

1. **线性化简化问题**：非线性最小二乘问题通常难以直接求解，但通过对残差函数进行一阶泰勒展开，可以将非线性问题转化为线性问题，从而简化求解过程。
2. **利用局部信息**：利用当前参数点 $\theta_k$ 处的局部信息（残差和雅可比矩阵）来构造一个近似的二次模型，并通过最小化二次模型来迭代更新参数。
3. **快速收敛**：当初始值接近最优解时，高斯牛顿法通常能够快速收敛，因为它利用了目标函数的二阶信息（通过 $J^T J$ 近似 Hessian 矩阵）。  

## 4. 缺点

1. **依赖初始值**：如果初始值远离最优解，算法可能无法收敛。
2. **雅可比矩阵的秩**：如果 $J^T J$ 是奇异矩阵（不可逆），算法会失效。
3. **非凸问题**：对于非凸问题，算法可能收敛到局部最优解。

## 5. 示例
```python
import numpy as np
import matplotlib.pyplot as plt


def Loss(param, x:np.array, y:np.array, is_calculateJ=False):
    y_estimate = param[0] * np.exp(param[1] * x)
    J = None
    if is_calculateJ:
        J = np.zeros((x.size, param.size))
        J[:, 0] = np.exp(param[1] * x) 
        J[:, 1] = x * y_estimate
    return y_estimate - y, J


if __name__ == "__main__":
    num_epochs = 10
    lambda_para = 0.001
    theta = np.array([1.0, 1.0])

    x_data = np.linspace(1, 10, 20)
    y_data = 0.5*np.exp(0.8*x_data) + np.random.normal(0, 8, x_data.size)
    for epoch in range(num_epochs):
        l, J = Loss(theta, x_data, y_data, True)
        print(f"loss:{l}")
        
        # J.T @ J @ theta = J.T @ y_data
        H = J.T @ J + lambda_para * np.eye(theta.size)
        delta = np.linalg.inv(H) @ (J.T @ l)

        # 
        theta -= delta
        print(f"theta:{theta}")
    
    plt.plot(x_data, y_data, 'o', label="Measure")
    plt.plot(x_data, theta[0] * np.exp(theta[1] * x_data), label="Optimizer")
    plt.legend()
    plt.show()
``` 