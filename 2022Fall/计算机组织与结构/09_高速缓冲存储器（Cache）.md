---
title: 09_高速缓冲存储器（Cache）
tags: 2022Fall_计算机组织与结构 课程
categories: 2022Fall_计算机组织与结构
date:  2023-01-28 17:03
modified:  星期五 24日 二月 2023 17:02:09
---
# 1.Cache的基本思路
- Cache存放主存的“副本”
- 解决内存墙
![49_coa22_第9讲.pdf_和另外_4_个页面_-_个人_-_Microsoft​_Edge.png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/imgae/2023/01/30/75f4ce2335d68f77c1a16c28ea2458a4_49_coa22_%E7%AC%AC9%E8%AE%B2.pdf_%E5%92%8C%E5%8F%A6%E5%A4%96_4_%E4%B8%AA%E9%A1%B5%E9%9D%A2_-_%E4%B8%AA%E4%BA%BA_-_Microsoft%E2%80%8B_Edge.png)


# 2.Cache的工作流程
![4a_coa22_第9讲.pdf_和另外_4_个页面_-_个人_-_Microsoft​_Edge.png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/imgae/2023/01/30/27712c3415ed48dfe2a922524c0e1736_4a_coa22_%E7%AC%AC9%E8%AE%B2.pdf_%E5%92%8C%E5%8F%A6%E5%A4%96_4_%E4%B8%AA%E9%A1%B5%E9%9D%A2_-_%E4%B8%AA%E4%BA%BA_-_Microsoft%E2%80%8B_Edge.png)

## 2.1 是否命中判断

- CPU通过**位置**对主存中的内容进行寻址，不关心存储在其中的内容
- Cache通过**标记（tags）** 来标识其内容在主存中的对应**位置**

## 2.2 平均访问时间

![4b_coa22_第9讲.pdf_和另外_4_个页面_-_个人_-_Microsoft​_Edge.png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/imgae/2023/01/30/3ad27bad2d1a52721bca99303e86df26_4b_coa22_%E7%AC%AC9%E8%AE%B2.pdf_%E5%92%8C%E5%8F%A6%E5%A4%96_4_%E4%B8%AA%E9%A1%B5%E9%9D%A2_-_%E4%B8%AA%E4%BA%BA_-_Microsoft%E2%80%8B_Edge.png)
- hit的情况要远远多于miss的情况
- 不考虑数据回传时间

## 2.3 cache访存过程
![capture-2023-01-30-15-23-02.jpg](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/imgae/2023/01/30/20a9fed2441f766b430adcb1bb4c90d3_20a9fed2441f766b430adcb1bb4c90d3_capture-2023-01-30-15-23-02.jpeg)

# 3.Cache设计要素

## 3.1 容量
扩大cache容量带来的结果：
- 增大了命中率$p$
- 增加了cache的开销和访问时间$T_c$

## 3.2 映射策略
- cache中的行数据:主存块数据+tag
- tag!=address，但可表示地址
- tag是额外信息，tag位数越少，可表示cache行越多
- 1块!=内存中的1个字
- 访存：**主存地址=块号+块内地址**
![4c_coa22_第9讲.pdf_和另外_4_个页面_-_个人_-_Microsoft​_Edge.png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/imgae/2023/01/30/4f2956541956c64c3a829bde396f99b9_4c_coa22_%E7%AC%AC9%E8%AE%B2.pdf_%E5%92%8C%E5%8F%A6%E5%A4%96_4_%E4%B8%AA%E9%A1%B5%E9%9D%A2_-_%E4%B8%AA%E4%BA%BA_-_Microsoft%E2%80%8B_Edge.png)

### 3.2.1 直接映射
- 直接映射：把主存的每一块直接映射到cache的**固定可用行**
	- 主存地址被分为三个字段：tag+cache行号+块内地址
	- ![4e_coa22_第9讲.pdf_和另外_4_个页面_-_个人_-_Microsoft​_Edge.png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/imgae/2023/01/30/eac06d1d89d2d24f1d3dadf5a613f8f2_4e_coa22_%E7%AC%AC9%E8%AE%B2.pdf_%E5%92%8C%E5%8F%A6%E5%A4%96_4_%E4%B8%AA%E9%A1%B5%E9%9D%A2_-_%E4%B8%AA%E4%BA%BA_-_Microsoft%E2%80%8B_Edge.png)



![capture-2023-01-30-15-49-44.jpg](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/imgae/2023/01/30/72d6e220f663fb124bebdde90d1aa5aa_capture-2023-01-30-15-49-44.jpg)
- cache行号 = 主存块号 mod cache行数
	- 假设cache有$2^c$行，主存有$2^m$块。cache行号就是m位主存块号的低c位
	- 对应关系为**多对一**
- 标记tag：地址中的高n位
	- $n=m-c$
	- 同一块中的字地址高位都相同

- 优点
	- 简单
	- 快速映射
	- 快速检查
- 缺点
	- 抖动现象（Thrashing）：如果一个程序重复访问两个需要映射到 同一行中且来自不同块的字，则这两个块不断地被交换到cache中， cache的命中率将会降低
- 适合大容量cache

## 3.2.2 全关联映射
- 关联映射：一个主存块可以装入cache的任意一行
	- ![4f_coa22_第9讲.pdf_和另外_4_个页面_-_个人_-_Microsoft​_Edge.png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/imgae/2023/01/30/f37b5607c9be01a6386016b9919aa730_4f_coa22_%E7%AC%AC9%E8%AE%B2.pdf_%E5%92%8C%E5%8F%A6%E5%A4%96_4_%E4%B8%AA%E9%A1%B5%E9%9D%A2_-_%E4%B8%AA%E4%BA%BA_-_Microsoft%E2%80%8B_Edge.png)



![4d_coa22_第9讲.pdf_和另外_4_个页面_-_个人_-_Microsoft​_Edge.png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/imgae/2023/01/30/abe1750361fd0b516dc4fff6c59a6dc3_4d_coa22_%E7%AC%AC9%E8%AE%B2.pdf_%E5%92%8C%E5%8F%A6%E5%A4%96_4_%E4%B8%AA%E9%A1%B5%E9%9D%A2_-_%E4%B8%AA%E4%BA%BA_-_Microsoft%E2%80%8B_Edge.png)
- 标记tag:地址中最高m位
	- tag为**块号**
	- 每行标记直接记录改行来自哪个块
- 优点
	- 避免抖动
- 缺点
	- 复杂
	- Cache搜索代价很大，即在检查的时候需要去访问cache的每一行
- 适合小容量cache

### 3.2.3 组关联映射
- 组关联映射：Cache分为若干组，每一组包含相同数量的行，每个主存块被映射到固定组的任意一行
	- ![51_coa22_第9讲.pdf_和另外_4_个页面_-_个人_-_Microsoft​_Edge.png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/imgae/2023/01/30/cc116d97518818902f99364e684336a6_51_coa22_%E7%AC%AC9%E8%AE%B2.pdf_%E5%92%8C%E5%8F%A6%E5%A4%96_4_%E4%B8%AA%E9%A1%B5%E9%9D%A2_-_%E4%B8%AA%E4%BA%BA_-_Microsoft%E2%80%8B_Edge.png)

- 假设s为cache组号，j为主存块号，S为组数，C为cache行数
	- s= j mode S
	- K路组关联映射：K=C/S


![capture-2023-01-30-16-25-09.jpg](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/imgae/2023/01/30/4897c15ee4cc8756d49afc7956f21ca1_capture-2023-01-30-16-25-09.jpg)
- 标记tag：地址最高$n=los_2 M - log_2 S$位

### 3.2.4 三种映射方式比较
- 如果 𝐾 = 1，组关联映射等同于直接映射
- 如果 𝐾 = 𝐶，组关联映射等同于关联映射

关联度：
![52_coa22_第9讲.pdf_和另外_4_个页面_-_个人_-_Microsoft​_Edge.png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/imgae/2023/01/30/98fc4046b9d8719f6b4b2afcb6a83033_52_coa22_%E7%AC%AC9%E8%AE%B2.pdf_%E5%92%8C%E5%8F%A6%E5%A4%96_4_%E4%B8%AA%E9%A1%B5%E9%9D%A2_-_%E4%B8%AA%E4%BA%BA_-_Microsoft%E2%80%8B_Edge.png)

## 3.3 替换算法
- 一旦cache行被占用，当新的数据块装入cache中时，原先存放的数据块将会被替换掉
- 对于直接映射，每个数据块都只有唯一对应的行可以放置，没有选择的机会
- 替换算法通过硬件来实现
### 3.3.1 最近最少使用算法（LRU）
![53_coa22_第9讲.pdf_和另外_4_个页面_-_个人_-_Microsoft​_Edge.png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/imgae/2023/01/30/0cddf04115a1bb5dd3f4c655bbdfb6da_53_coa22_%E7%AC%AC9%E8%AE%B2.pdf_%E5%92%8C%E5%8F%A6%E5%A4%96_4_%E4%B8%AA%E9%A1%B5%E9%9D%A2_-_%E4%B8%AA%E4%BA%BA_-_Microsoft%E2%80%8B_Edge.png)

### 3.3.2 先进先出算法（FIFO）
![54_coa22_第9讲.pdf_和另外_4_个页面_-_个人_-_Microsoft​_Edge.png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/imgae/2023/01/30/bb4508c0ae6564c231471c67bfb15a7b_54_coa22_%E7%AC%AC9%E8%AE%B2.pdf_%E5%92%8C%E5%8F%A6%E5%A4%96_4_%E4%B8%AA%E9%A1%B5%E9%9D%A2_-_%E4%B8%AA%E4%BA%BA_-_Microsoft%E2%80%8B_Edge.png)

### 3.3.3 最不经常使用算法（LFU）
![55_coa22_第9讲.pdf_和另外_4_个页面_-_个人_-_Microsoft​_Edge.png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/imgae/2023/01/30/f3b3c6fac508b93fc1aef7e0d5d7b275_55_coa22_%E7%AC%AC9%E8%AE%B2.pdf_%E5%92%8C%E5%8F%A6%E5%A4%96_4_%E4%B8%AA%E9%A1%B5%E9%9D%A2_-_%E4%B8%AA%E4%BA%BA_-_Microsoft%E2%80%8B_Edge.png)

### 3.3.4 随机替换算法（Random）
![56_coa22_第9讲.pdf_和另外_4_个页面_-_个人_-_Microsoft​_Edge.png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/imgae/2023/01/30/7a57b1333112050c402d89ad689bc113_56_coa22_%E7%AC%AC9%E8%AE%B2.pdf_%E5%92%8C%E5%8F%A6%E5%A4%96_4_%E4%B8%AA%E9%A1%B5%E9%9D%A2_-_%E4%B8%AA%E4%BA%BA_-_Microsoft%E2%80%8B_Edge.png)

## 3.4 写策略
- 主存和cache的一致性
	- 当cache中的某个数据块**被替换时**，需要考虑该数据块是否被修改
	- 如果没被修改，则该数据块可以直接被替换掉
	- 如果**被修改**，则在替换掉该数据块之前，必须将修改后的数据块写回到主存中对应位置
### 3.4.1 写回法（write back）
- 利用<font color="#ff0000">脏位(dirty bit)</font>标记块是否被修改
- 优点
	- 减少了访问主存的次数
- 缺点
	- 部分主存数据可能不是最新的
		- I/O模块存取时可能无法获得最新的数据，为解决该问题会使得电路设计更加复杂且有可能带来性能瓶颈
	- 多CPU时存在一致性问题

### 3.4.2 写直达（write through）
- 所有写操作都同时对cache和主存进行
- 优点
	- 确保主存中的数据总是和cache中的数据一致，总是最新的
- 缺点
	- 产生大量的主存访问，减慢写操作
## 3.5 行大小
行大小与cache命中率关系较复杂
![57_coa22_第9讲.pdf_和另外_4_个页面_-_个人_-_Microsoft​_Edge.png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/imgae/2023/01/30/103bb6b93c409bc87b3e8dc24b052e82_57_coa22_%E7%AC%AC9%E8%AE%B2.pdf_%E5%92%8C%E5%8F%A6%E5%A4%96_4_%E4%B8%AA%E9%A1%B5%E9%9D%A2_-_%E4%B8%AA%E4%BA%BA_-_Microsoft%E2%80%8B_Edge.png)

## 3.6 Cache数目
![58_coa22_第9讲.pdf_和另外_4_个页面_-_个人_-_Microsoft​_Edge.png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/imgae/2023/01/30/981cc93bdd84acadcb0ed4bd730b0436_58_coa22_%E7%AC%AC9%E8%AE%B2.pdf_%E5%92%8C%E5%8F%A6%E5%A4%96_4_%E4%B8%AA%E9%A1%B5%E9%9D%A2_-_%E4%B8%AA%E4%BA%BA_-_Microsoft%E2%80%8B_Edge.png)
![59_coa22_第9讲.pdf_和另外_4_个页面_-_个人_-_Microsoft​_Edge.png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/imgae/2023/01/30/09af25ea8612f5e082133b10c6320199_59_coa22_%E7%AC%AC9%E8%AE%B2.pdf_%E5%92%8C%E5%8F%A6%E5%A4%96_4_%E4%B8%AA%E9%A1%B5%E9%9D%A2_-_%E4%B8%AA%E4%BA%BA_-_Microsoft%E2%80%8B_Edge.png)
