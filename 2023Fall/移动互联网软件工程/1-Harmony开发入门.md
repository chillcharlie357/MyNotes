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
title: 1-Harmony开发入门
date:  Friday,September 15th 2023
modified:  Friday,September 22nd 2023
---
- Moudle
	- Ability
	- Library

# 快速入门

## Stage模型

FA模型和Android每个应用都有一个独立虚拟机，Stage组件之间可以共享对象和状态。

多HAP的开发调试


# ArkTS Runtime
## ArkTS与JS Runtime区别

ArkTS在前端先把源代码编译成字节码
JS直接拿源代码

![image.png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/image%2F2023%2F10%2F13%2Ffe1d848c03c34327fb0f2a19ecce13d8_20231013151338.png)

## 执⾏引擎执行引擎对比

⽆论是JIT 编译器⽣成的优化代码，还是AOT编译器⽣成的优化代码，通常都是在⼀定优化假设或者优化推断的前提下⽣成的。如果这个前提在运⾏时不成⽴，则需要进⾏Deopt（逆优化），回退到解释器执⾏，这种情况⼀般较少发⽣。
### 解释器

- 解释器可直接运⾏前端编译器输出的字节码。
- 启动快，执⾏性能⼀般，内存占⽤⼩，**小设备**

### JIT编译器

- JIT编译器⼀般需要运⾏时执⾏代码⼀段时间，Profiler⽣成了profiling数据之后，根据profiling数据即时编译⽣成⾼质量的 机器码（上图Optimized Code II）来运⾏。（JIT可以根据代码执⾏情况实时编译⽣成最优机器指令）
- 启动需要预热，执⾏性能⾼，内存占⽤较⾼，**高端设备**

### AOT编译器

- AOT编译器则是在运⾏前根据静态信息直接编译⽣成⾼质量的⽬标机器码（上图Optimized Code I)在设备上运⾏，PGO （Profile Guided Optimization）配置⽂件可以作为AOT Compiler的输⼊之⼀，给AOT Compiler⼀些指示，⽐如编译的范 围以及编译某个⽅法时使⽤哪些优化技术。通常这种PGO配置⽂件由在同等规格的设备上经过运⾏时profiling或者⼤数据 分析⽣成
- 启动快，执⾏性能⾼，内存占⽤⾼，**高端设备/高配应用**

