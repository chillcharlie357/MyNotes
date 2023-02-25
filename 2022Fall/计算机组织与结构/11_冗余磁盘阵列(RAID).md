---
title: 11_冗余磁盘阵列(RAID)
tags: 2022Fall_计算机组织与结构 课程
categories: 2022Fall_计算机组织与结构
date:  2023-02-03 21:57
modified:  星期五 24日 二月 2023 15:48:46
---


- 冗余磁盘阵列 / 独立磁盘冗余阵列：Redundant Arrays of Independent Disks (RAID)
- 基本思想
	- 将多个独立操作的磁盘按某种方式组织成磁盘阵列，以增加容量
	- 将数据存储在多个盘体上，通过这些盘并行工作来提高数据传输率
	- 采用**数据冗余**来进行错误恢复以提高系统可靠性
- 特性
	- 由一组物理磁盘驱动器组成，被视为单个的逻辑驱动器
	- 数据是分布在多个物理磁盘上
	- 冗余磁盘容量用于存储校验信息，保证磁盘万一损坏时能恢复数据


# 1.RAID分类
![68_VoIPSCreencastCoverWnd (2).png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/imgae/2023/02/06/87e4653208861ea242570d11e7e4233f_68_VoIPSCreencastCoverWnd%20-2-.png)

# 2.RAID 0
- 数据以**条带**的形式在可用的磁盘上分布
	- Eg：按照0、1、2、3顺序
- 不采用冗余来改善性能（不是RAID家族中的真正成员）
## 2.1 优点
	- 高数据传输率
	- 高速响应I/O请求：两个I/O请求所需要的数据块可能在不同的磁盘
## 2.2 缺点
	- 任何一个盘损坏文件就不可用
![69_VoIPSCreencastCoverWnd (2).png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/imgae/2023/02/06/cc4bd9bbc260174a4216ee4074ae214e_69_VoIPSCreencastCoverWnd%20-2-.png)
# 3.RAID 1
- 采用了数据**条带**
- 采用简单地**备份所有数据**的方法来实现冗余
## 3.1 优点
- 高速响应I/O请求：即便是同一个磁盘上的数据块，也可以由两组硬盘分别响应
	- **读请求**可以由包含请求数据的两个对应磁盘中的某一个提供服务，可以选择寻道时间较小的那个，**更快**
	- **写请求**需要更新两个对应的条带：可以并行完成，但受限于写入较慢的磁盘，**更慢**
- 单个磁盘损坏时不会影响数据访问，恢复受损磁盘简单
## 3.2 缺点
- 价格昂贵

## 3.3 用途
- 只限于用在存储系统软件、数据和其他关键文件的驱动器中

## 3.4 与RAID 0相比
- 如果有大批的读请求，则RAID 1能实现高速的I/O速率，性能可以达到RAID 0的两倍
- 如果I/O请求有相当大的部分是写请求，则它不比RAID 0的性能好多少![6a_VoIPSCreencastCoverWnd (2).png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/imgae/2023/02/06/1797eeb27985b87c4119e3b481de7c33_6a_VoIPSCreencastCoverWnd%20-2-.png)
## 3.5RAID 01 vs. RAID 10
- RAID 01 = RAID 0+1：先做RAID 0，再做RAID 1
- RAID 10 = RAID 1+0：先做RAID 1，再做RAID 0
- 两者在数据传输率和磁盘利用率上没有明显区别，主要区别是对磁盘损坏的**容错能力**
	-  RAID 10容错率更高
![6b_VoIPSCreencastCoverWnd (2).png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/imgae/2023/02/06/32b5edd92194b0423e9afced26069f4a_6b_VoIPSCreencastCoverWnd%20-2-.png)

# 4.RAID 2
- 采用并行存取技术
![6c_VoIPSCreencastCoverWnd (2).png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/imgae/2023/02/06/83dee816dc8e78b2e3bcb2354c8497d6_6c_VoIPSCreencastCoverWnd%20-2-.png)

## 4.1 目标
- **所有**磁盘都参与**每**个I/O请求的执行

## 4.2 特点
- 各个驱动器的轴是同步旋转的，因此每个磁盘上的每个磁头在任何时刻都位于同一位置
- 采用**数据条带**：**条带非常小**，经常只有一个字节或一个字

## 4.3 纠错
对位于同一条带的各个数据盘上的数据位**计算校验码（通常采用海明码）**，校验码存储在该条带中多个校验盘的对应位置

## 4.4 访问
- 读取：获取请求的数据和对应的校验码
- 写入：所有数据盘和校验盘都被访问

## 4.5 缺点
- 冗余盘依然比较多，价格较贵
- 适用于多磁盘易出错环境，对于单个磁盘和磁盘驱动器已经具备高 可靠性的情况没有意义

# 5.RAID 3
- 采用并行存取技术
	- 各个驱动器的轴同步旋转
	- 采用非常小的数据条带

## 5.1 校验
- 对所有数据盘上**同一位置**的数据计算奇偶校验码
	- 当某一磁盘损坏时，可以用于重构数据
![6d_VoIPSCreencastCoverWnd (2).png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/imgae/2023/02/06/f7562ab00bdaf086770ab9ba01f78f12_6d_VoIPSCreencastCoverWnd%20-2-.png)


## 5.2 优缺点
- 优点：能够获得非常高的数据传输率
	- 对于大量传送，性能改善特别明显
- 缺点：一次只能执行一个I/O请求
	- 在面向多个IO请求时，性能将受损


# 6.RAID 4
- 采用独立存取技术
	- 读可以独立，写无法独立
	- 每个磁盘成员的操作是独立的，各个I/O请求能够并行处理
	- 采用相对**较大的数据条带**

## 6.1 校验
根据各个数据盘上的数据来逐位计算奇偶校验条带，奇偶校验位存储在**奇偶校验盘**的对应条带上
![6e_VoIPSCreencastCoverWnd (2).png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/imgae/2023/02/06/521fcea4b354b9de72e3a4c5ffe926bd_6e_VoIPSCreencastCoverWnd%20-2-.png)

- 优化修改数据时**重新计算校验码**的读写次数
![capture-2023-02-06-11-06-09.jpg](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/imgae/2023/02/06/39de9d867ee7099a563e4d67ea0e97d2_capture-2023-02-06-11-06-09.jpg)


## 6.2 性能
- 当执行**较小规模**的I/O写请求时，RAID 4会遭遇**写损失**
	- 对于每一次写操作，阵列管理软件不仅要修改用户数据， 而且要修改相应的校验位
	- ![6f_VoIPSCreencastCoverWnd (2).png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/imgae/2023/02/06/13b04dadc923526ad38ea677a3fd5656_6f_VoIPSCreencastCoverWnd%20-2-.png)
- 当涉及所有磁盘的数据条带的较大I/O写操作时，只要用新的数据位来进行简单的计算即可得到奇偶校验位
- 每一次写操作必须涉及到**唯一的校验盘**，校验盘会成为瓶颈

# 7.RAID 5
- 与RAID 4 组织方式相似
- 在所有磁盘上都分布了奇偶校验条带
	- 避免潜在的I/O瓶颈问题
- 访问时的“两读两写”
![70_VoIPSCreencastCoverWnd (2).png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/imgae/2023/02/06/c4f46bafa944914cebc6c8989ebaf21f_70_VoIPSCreencastCoverWnd%20-2-.png)

## 7.1 RAID 50
- 先作RAID 5，再作RAID 0,也就 是对多组RAID 5彼此构成条带访问
- RAID 50在底层的任一组或多组RAID 5中出现1颗硬盘损坏时，仍能维持运作；如果任一组RAID 5中出现2颗或2颗以上硬盘 损毁，整组RAID 50就会失效
- RAID 50由于在上层把多组RAID 5进行条带化，性能比起单 纯的RAID 5高，但容量利用率比RAID5要低


# 8.RAID 6
- 采用**两种不同的校验码**，并将校验码以分开的块存于不同的磁盘中
- 优点
	- 提升数据可用性：只有在平均修复时间间隔内3个磁盘都出了故障，才会造成数据丢失
- 缺点
	- 写损失:每次写影响到两个校验块
![71_VoIPSCreencastCoverWnd (2).png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/imgae/2023/02/06/81ceda6a5d81fd50326e531d313bac7d_71_VoIPSCreencastCoverWnd%20-2-.png)
# 9.RAID比较
![72_VoIPSCreencastCoverWnd (2).png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/imgae/2023/02/06/2d0b20945c49a12772798b2459b890bb_72_VoIPSCreencastCoverWnd%20-2-.png)
![73_VoIPSCreencastCoverWnd (2).png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/imgae/2023/02/06/927619b8756054d87fa147a66555f92d_73_VoIPSCreencastCoverWnd%20-2-.png)
![74_VoIPSCreencastCoverWnd (2).png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/imgae/2023/02/06/051c92a3b2c43309c2ec65cf9f4d14eb_74_VoIPSCreencastCoverWnd%20-2-.png)
