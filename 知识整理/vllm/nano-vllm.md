---
aliases:
tags:
  - 技术分享
  - vllm
  - AIInfra
categories:
sticky:
thumbnail:
cover:
excerpt: false
mathjax: true
comment: true
title: nano-vllm
date: 2025-12-25 16:12
modified: 2025-12-29 11:12
---

Nano-vLLM 是一个从零开始构建的轻量级 vLLM (Virtual Large Language Model) 实现。它的核心目标是在保持极简代码库（约 1200 行 Python 代码）的同时，提供与原始 vLLM 相当的高性能推理能力。

---

# 核心架构设计

- LLM Engine & Scheduler ( nanovllm/engine/ ) :

- Scheduler : 实现了 FCFS（先来先服务）调度策略，支持 Prefill（预填充） 和 Decode（解码） 阶段的动态切换。
- BlockManager : 实现了类似 PagedAttention 的内存管理机制，通过将 KV Cache 划分为固定大小的块（Blocks）来减少内存碎片。
- Sequence : 封装了单个推理请求的状态、Token 序列及对应的物理内存映射。
- Model Execution ( nanovllm/engine/model_runner.py ) :

- 张量并行 (Tensor Parallelism) : 支持多 GPU 推理，利用 torch.distributed 和 nccl 后端进行模型切分和通信。
- 多进程架构 : 使用 multiprocessing 启动多个进程，主进程负责调度，子进程负责 GPU 上的模型计算，通过共享内存同步状态。
- Neural Network Layers ( nanovllm/layers/ ) :

- 提供了自定义的 Attention 、 Linear 、 RotaryEmbedding 等层。
- Flash Attention : 集成了 flash-attn 以加速注意力机制计算。
- Triton Kernels : 编写了自定义的 Triton 内核（如 store_kvcache_kernel ），用于高效地将 KV 向量存入 Paged Cache。