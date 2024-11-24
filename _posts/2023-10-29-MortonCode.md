---
title: Morton Code
author: melon
date: 2023-10-29 20:00:00 +0800
categories: [Math, Algorithm]
tags: [typography]
pin: true
math: true
mermaid: true
---

# MortorCode(莫顿编码)
莫顿编码是一种将多维空间坐标映射到整数的方法；常应用于2D/3D坐标信息处理，用于数据压缩或者快速搜索等场景中；该编码有一个非常好的属性：当两个2D/3D坐标相差距离较小时，它们对应的莫顿码也是接近的；  
二维坐标$P(x,y)$的莫顿码计算流程：
* 将x，y转换为二进制表示后，x的每位设置到莫顿码的偶数位，y的每位设置到莫顿码的奇数位，如下图所示：

1. 使用循环计算

```python
def MortonCodeLoop(x, y):
  res = 0
  for i in range(32):
    res |= (x &(1<<i))<<i | (y &(1<<i))<<(i+1)
  return res
```
2. 使用魔术数字计算  

```python  
def MortonCodeMagic(x, y):
  B = [0x55555555, 0x33333333, 0x0F0F0F0F, 0x00FF00FF]
  S = [1, 2, 4, 8]

  x = (x | (x << S[3])) & B[3]
  x = (x | (x << S[2])) & B[2]
  x = (x | (x << S[1])) & B[1]
  x = (x | (x << S[0])) & B[0]

  y = (y | (y << S[3])) & B[3]
  y = (y | (y << S[2])) & B[2]
  y = (y | (y << S[1])) & B[1]
  y = (y | (y << S[0])) & B[0]
  return x | (y << 1)
```

3. 查询已编码的莫顿表计算
上述三种方式运行性能： 方式3 > 方式2 > 方式1  

**参考**  
1. [Z-order_curve](https://en.wikipedia.org/wiki/Z-order_curve)  
2. [libmorton-a-library-for-morton-order-encoding-decoding](https://www.forceflow.be/2016/01/18/libmorton-a-library-for-morton-order-encoding-decoding/ )
3. [Interleave bits the obvious way](https://graphics.stanford.edu/~seander/bithacks.html#InterleaveTableObvious)  