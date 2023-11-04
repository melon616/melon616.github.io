---
title: 基于高斯分布来理解卡尔曼滤波
author: melon
date: 2023-10-28 11:33:00 +0800
categories: [Math]
tags: [typography]
pin: true
math: true
mermaid: true
---

# 1. 贝叶斯公式

$$p(x,y) = p(y|x)p(x)$$

# 2. 高斯分布
一维高斯分布：  

$$p(x;\mu,\sigma) = \frac{1}{\sqrt{2\pi \sigma^2}}exp(-\frac{(x-\mu)^2}{2\sigma^2})$$

多维高斯分布：  

$$p(x;\mu,\Sigma) = \frac{1}{\sqrt{(2\pi)^n|\sigma|}}exp(-\frac{1}{2}(x-\mu)^T\Sigma^{-1
}(x-\mu))$$

定理：$X \thicksim\mathcal{N}(\mu, \Sigma)$，可知线性变换$Y = CX \thicksim\mathcal{N}(C\mu, C\Sigma{C^T})$  
定理：高斯随机向量的线性变换仍是符合高斯分布  
证明：已知随机向量$x\in \mathbb{R}^P$且$x\thicksim\mathcal{N}(\mu_x,\Sigma_x)$，噪声向量$w\in \mathbb{R}^N$且$w\thicksim\mathcal{N}(0,\Sigma_w)$，且两者相互独立，于是$y = Hx+w$和$x$组成的联合概率分布如下式： 

$$z = \begin{bmatrix}x \\ y\end{bmatrix}
=\begin{bmatrix}I_P &0 \\ H & I_N\end{bmatrix}
\begin{bmatrix}x \\ w\end{bmatrix}=A\begin{bmatrix}x \\ w\end{bmatrix}$$

$$\begin{bmatrix}x \\ w\end{bmatrix}
\thicksim \mathcal{N}(\begin{bmatrix}\mu \\ 0\end{bmatrix}, \begin{bmatrix}\Sigma_x &0 \\ 0 & \Sigma_y\end{bmatrix}
)$$

则线性变换的联合高斯分布：

$$\begin{aligned}\begin{bmatrix}x \\ y\end{bmatrix}
&\thicksim \mathcal{N}(\begin{bmatrix}I_P &0 \\ H & I_N\end{bmatrix}\begin{bmatrix}\mu \\ 0\end{bmatrix}, \begin{bmatrix}I_P &0 \\ H & I_N\end{bmatrix}\begin{bmatrix}\Sigma_x &0 \\ 0 & \Sigma_w\end{bmatrix}\begin{bmatrix}I_P &H^T \\ 0 & I_N\end{bmatrix}
) \\
&\thicksim \mathcal{N}(\begin{bmatrix}\mu \\ H\mu\end{bmatrix}, \begin{bmatrix}\Sigma_x &\Sigma_xH^T \\ H\Sigma_x & H\Sigma_wH^T+\Sigma_w\end{bmatrix}
) \end{aligned}$$

关于变量$y$的边缘分布:

$$y\thicksim \mathcal{N}(H\mu, H\Sigma_wH^T+\Sigma_w)$$

贝叶斯线性模型下的条件高斯分布：

$$y|x\thicksim\mathcal{N}(\mathbb{E}[y]+C_{yx}C_x^{-1
}(x-\mathbb{E}[x]), C_y-C_{yx}C_{xx}^{-1}C_{xy})$$

$$x|y\thicksim\mathcal{N}(\mathbb{E}[x]+C_{xy}C_y^{-1
}(y-\mathbb{E}[y]), C_x-C_{xy}C_{yy}^{-1}C_{yx})$$

其中

$$C = \begin{bmatrix}C_x &C_{xy} \\ C_{yx} &C_y\end{bmatrix}
=\begin{bmatrix}\Sigma_x &\Sigma_xH^T \\ H\Sigma_x & H\Sigma_wH^T+\Sigma_w\end{bmatrix}$$

# 3. 卡尔曼滤波
* 状态一步预测：$X_{k-1}, X_k$的联合分布 -> $X_k$边缘分布  

$$\hat{X}_{k/k-1} = \Phi_{k/k-1}\hat{X}_{k-1}$$

$$P_{k/k-1} = \Phi_{k/k-1}P_{k-1}\Phi_{k/k-1}^T + \Gamma_{k/k-1}Q_{k-1}\Gamma_{k/k-1}^T$$

* 滤波增益

$$K_k = P_{k/k-1}H_k^T(H_kP_{k/k-1}H_k^T + R_k)^{-1}$$

* 状态观测更新： $X_k, Z_k$的联合分布 -> $X_k$条件分布  

$$\hat{X}{_k}=\hat{X}_{k/k-1} + K_k(Z_k - \hat{X}_{k/k-1})$$

$$P_k = (I - K_kH_k)P_{k/k-1}$$

