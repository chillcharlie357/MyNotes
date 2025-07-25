---
aliases: 
tags: 
categories:
sticky:
thumbnail:
cover: 
excerpt: false
mathjax: true
comment: true
title: RQ-VAE
date:  2025-07-23 15:07
modified:  2025-07-25 20:07
---

# Encoder

把输入变成连续向量（latent vector）`z`

使用ResNet/CNN（图像）或 Transformer（文本/语音）

# Residual Quantizer Stack

将 Encoder 输出的 latent 表示 `z` 进行**多级向量量化**，每一级量化残差。

- **VQ-VAE**：用一个码本对 z 量化
- **RQ-VAE**：用 L 层码本对 z 逐级残差量化：

```
r0 = z
q1 = Quantize(r0)               # 用 codebook1
r1 = r0 - q1                    # 残差1
q2 = Quantize(r1)               # 用 codebook2
...
qL = Quantize(rL-1)

z_q = q1 + q2 + ... + qL
```

编码器最终输出的 latent 表示是多个码字的和

# Decoder

- 接收重构后的 latent 表示 `z_q`
- 生成与输入等尺寸的输出（图像/语音/视频帧等）
- 通常使用反卷积（ConvTranspose）或 Transformer 解码器

# Loss

## 损失函数组成

由2个损失函数组成

1. 重建损失：L1 或 L2 loss / cross entropy（图像或语音）
2. 各级码本的 commitment loss（防止表示崩塌）：

$$
L_{total} = L_{recon} + β * Σ_i ||sg[z_i] - e_i||^2 + γ * Σ_i ||z_i - sg[e_i]||^2
$$

`sg` 为 `stop-gradient`停止梯度传播，不是数学操作：

```python
sg[x] = x.detach()
```

## 为什么使用sg

深度学习模型通常使用**梯度下降**来训练参数，这需要**每个操作都是可导的**。但在 **VQ-VAE / RQ-VAE** 中，我们需要从一个**离散码本（codebook）中选最近的向量**。

因此需要用到Straight-Through Estimator（STE，直通估计器）技巧：正向传播用离散值，反向传播假装没离散。

RQ-VAE中STE使用：

```python
# z: encoder 输出
# e: 从 codebook 中查到的最近码字
z_q = e + (z - e).detach()
```

- 正向用 `e`，表示离散化后的向量
- 反向传播时：
    - `e` 部分不参与梯度（detach）
    - `z` 部分继续传梯度给 encoder

# Codebook

- 在训练过程中和encoder、decoder一起更新参数
- 初始数据：第一个batch中encoder输出的latent embedding，使用kmeans得到聚类中心作为初始的codebook
	- 可以避免codebook类别不均匀，提高codebook的使用率
	- 也可以直接随机初始化
- vq-vae只有一个codebook，rq-vae有多个并且粒度越来越细

# 归一化

## L2Norm

L2归一化将向量缩放到单位长度，使其L2范数（欧几里得范数）等于1。

### 数学公式

$$
normalized_x = x / ||x||_2
$$

其中 `||x||_2 = sqrt(sum(x_i^2))` 是L2范数。

### 代码实现

```python
def l2norm(x, dim=-1, eps=1e-12):
    return F.normalize(x, p=2, dim=dim, eps=eps)
```

### 特点

- **保持方向**：只改变向量的大小，不改变方向
- **固定长度**：所有向量都被缩放到单位长度
- **用途**：
    - 余弦相似度计算
    - 特征向量标准化
    - 防止梯度爆炸
- **几何意义**：将向量投影到单位球面上

## RMSNorm

### 原理

RMSNorm是LayerNorm的简化版本，只进行缩放而不进行平移（去除了减均值的步骤）。

### 数学公式

$$
RMS(x) = sqrt(mean(x^2))
$$

$$
normalized_x = (x / RMS(x)) * weight
$$

### 代码实现

```python
def _norm(self, x):
    return x * torch.rsqrt(x.pow(2).mean(-1, keepdim=True) + self.eps)

def forward(self, x):
    output = self._norm(x.float()).type_as(x)
    return output * self.weight
```

### 特点

- **可学习权重**：包含可训练的缩放参数 `weight`
- **无偏移**：不减去均值，只进行缩放
- **计算效率**：比LayerNorm更高效（少了计算均值的步骤）
- **用途**：
    - Transformer模型中的层归一化
    - 稳定训练过程
    - 加速收敛

```python
# L2归一化示例
x = torch.tensor([[3.0, 4.0], [1.0, 2.0]])
l2_normalized = l2norm(x)  # 每行向量长度变为1
print(l2_normalized)  # [[0.6, 0.8], [0.447, 0.894]]

# RMSNorm示例
rms_norm = RMSNorm(dim=2)
rms_normalized = rms_norm(x)  # 标准化 + 可学习缩放
```

z`L2归一化更适合特征标准化，RMSNorm更适合深度学习中的层归一化。