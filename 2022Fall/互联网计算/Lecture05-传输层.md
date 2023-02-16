  # 1. Lecture05-第四层传输层

第四层运输层主要是实现了主机之间的通信。数据通信是服务于主机上的**进程**(Session)。

## 1.1. 第四层概述

![第5讲：传输层原理与技术_页面_03](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第5讲：传输层原理与技术_页面_03.jpg)

1. 分割上层应用程序数据(新的数据单元-数据段)

2. 建立端到端(end to end)的通讯

3. 从一个终端主机向另一个终端主机发送**段segment**

   (第三层和第二层不进行可靠性检验，第四层完成可靠性检验，接受方认为数据错误，在第四层进行要求重传)

4. 流量控制和可靠性

   1. 可以比喻为与外国人交谈：通常，您会要求外国人重复他/她的话(可靠性)并慢声说话(流量控制)
   2. 双方主机的网络的处理能力不同，缓存能力不同

![第5讲：传输层原理与技术_页面_04](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第5讲：传输层原理与技术_页面_04.jpg)

1. 传输控制协议(TCP, Transmission Control Protocol)
2. 用户数据报协议(UDP, User Datagram Protocol)

![第5讲：传输层原理与技术_页面_05](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第5讲：传输层原理与技术_页面_05.jpg)

将传出邮件分成多个部分

在目标站重新组合消息

TCP: 可靠(效率比较低，早期网络应用少，需要可靠性)

1. 面向连接
2. 软件检查段segment
3. 重新发送丢失或错误的任何内容
4. 使用确认机制
5. 提供流量控制

UDP: 不可靠

1. 无连接
2. 不提供段的软件检查
3. 不使用确认
4. 不进行流量控制
5. 直接丢弃错误的报文，而不进行其他操作。

### 1.1.1. 服务模型

![第5讲：传输层原理与技术_页面_06](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第5讲：传输层原理与技术_页面_06.jpg)

1. TCP和UDP都使用**端口**来跟踪(track)同时穿越网络的不同会话

2. 应用软件开发人员已同意使用RFC1700中定义的知名端口号

3. 低于255的端口号(0-255)保留给TCP和UDP公共应用程序使用。

   0-1023是知名端口，有分发的规范，不应当被随意使用

   1024-49151的端口号进行登记使用，有的是应用程序已经的使用端口号，避免冲突

### 1.1.2. 套接字

![第5讲：传输层原理与技术_页面_07](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第5讲：传输层原理与技术_页面_07.jpg)

1. 套接字表示为(IP地址，端口)
2. 每个连接都表示为(socket  source ，socket  destination)，这是一个点对点全双工通道
3. **TCP不支持多播和广播**



## 1.2. TCP

![第5讲：传输层原理与技术_页面_09](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第5讲：传输层原理与技术_页面_09.jpg)

1. 可靠传输
2. 流控制
   1. 滑动窗口(窗口进行通信，一次数据传输是有上限发的，缓存问题，拥塞问题)
   2. 避免拥塞
3. 连接控制
   1. 建立连接——**三次**握手
   2. 断开连接——**四次**握手

### 1.2.1. TCP数据段格式

#### 1.2.1.1. 源端口 目的端口

![第5讲：传输层原理与技术_页面_10](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第5讲：传输层原理与技术_页面_10.jpg)

socket

#### 1.2.1.2. 序号

![第5讲：传输层原理与技术_页面_11](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第5讲：传输层原理与技术_页面_11.jpg)

我们**从小向大**进行使用，如果使用到最大之后，我们会从小再次重新开始分配。



#### 1.2.1.3. 确认号

![第5讲：传输层原理与技术_页面_12](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第5讲：传输层原理与技术_页面_12.jpg)

发数据的同时，对对方上一次的传输做确认



#### 1.2.1.4. 数据偏移

![第5讲：传输层原理与技术_页面_13](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第5讲：传输层原理与技术_页面_13.jpg)



#### 1.2.1.5. 保留

![第5讲：传输层原理与技术_页面_14](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第5讲：传输层原理与技术_页面_14.jpg)



#### 1.2.1.6. 标记位——URG

![第5讲：传输层原理与技术_页面_15](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第5讲：传输层原理与技术_页面_15.jpg)

比如说按Ctrl+C终止程序的信息可能会将URG置为1

#### 1.2.1.7. 标记位——ACK

![第5讲：传输层原理与技术_页面_16](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第5讲：传输层原理与技术_页面_16.jpg)



#### 1.2.1.8. 标记位——PSH

![第5讲：传输层原理与技术_页面_17](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第5讲：传输层原理与技术_页面_17.jpg)

根据网络条件调整，正常情况下缓存满了才会传输

#### 1.2.1.9. 标记位——RST

![第5讲：传输层原理与技术_页面_18](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第5讲：传输层原理与技术_页面_18.jpg)

连接失败

#### 1.2.1.10. 标记位——SYN

![第5讲：传输层原理与技术_页面_19](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第5讲：传输层原理与技术_页面_19.jpg)



#### 1.2.1.11. 标记位——FIN

![第5讲：传输层原理与技术_页面_20](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第5讲：传输层原理与技术_页面_20.jpg)



#### 1.2.1.12. 窗口

![第5讲：传输层原理与技术_页面_21](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第5讲：传输层原理与技术_页面_21.jpg)



#### 1.2.1.13. 检验和

![第5讲：传输层原理与技术_页面_22](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第5讲：传输层原理与技术_页面_22.jpg)



#### 1.2.1.14. 紧急指针

![第5讲：传输层原理与技术_页面_23](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第5讲：传输层原理与技术_页面_23.jpg)

#### 1.2.1.15. 可选部分

![第5讲：传输层原理与技术_页面_24](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第5讲：传输层原理与技术_页面_24.jpg)

MSS Maximum Segment Size最大报文段长度



#### 1.2.1.16. 填充

![第5讲：传输层原理与技术_页面_25](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第5讲：传输层原理与技术_页面_25.jpg)



### 1.2.2. TCP协议

![第5讲：传输层原理与技术_页面_26](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第5讲：传输层原理与技术_页面_26.jpg)

主机使用网段(TPDU)交换数据

每个段都有：

1. 首部为20个字节(可选部分除外)
2. 0或更多数据字节(请求连接的时候)

段的大小必须与IP数据包匹配，并且还必须满足底层的需求

1. 例如，以太网的MTU(最大传输单位)为1500字节
2. 是面向字节的传输。

每个字节都有一个32位序号

### 1.2.3. 可靠连接——两军问题

![第5讲：传输层原理与技术_页面_27](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第5讲：传输层原理与技术_页面_27.jpg)

蓝军必须一起攻打才能打败白军

蓝军信息可能被白军篡改或者阻碍

结论：无论通信多少次，都不能有一个完全可信的消息（进入死循环）

### 1.2.4. 建立连接

#### 1.2.4.1. 第一次握手

![第5讲：传输层原理与技术_页面_28](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第5讲：传输层原理与技术_页面_28.jpg)

1. 服务器：执行LISTEN和ACCEPT原语，并进行被动监视
2. 客户端：执行CONNECT原语，生成SYN = 1和ACK = 0的TCP段，代表连接请求

#### 1.2.4.2. 第二次握手

![第5讲：传输层原理与技术_页面_29](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第5讲：传输层原理与技术_页面_29.jpg)

服务器检查是否存在监视端口的服务进程

1. 如果没有任何进程，请使用RST = 1回答一个TCP段
2. 如果存在进程，则决定拒绝或接受请求
3. 如果接受连接请求，则发送SYN = 1和ACK = 1的段

#### 1.2.4.3. 第三次握手

![第5讲：传输层原理与技术_页面_30](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第5讲：传输层原理与技术_页面_30.jpg)

客户端发送一个SYN = 0和ACK = 1的段以确认连接

![第5讲：传输层原理与技术_页面_31](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第5讲：传输层原理与技术_页面_31.jpg)



![第5讲：传输层原理与技术_页面_32](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第5讲：传输层原理与技术_页面_32.jpg)



![第5讲：传输层原理与技术_页面_33](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第5讲：传输层原理与技术_页面_33.jpg)

### 1.2.5. 传输控制

数据传输——停止等待协议

![第5讲：传输层原理与技术_页面_34](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第5讲：传输层原理与技术_页面_34.jpg)



![第5讲：传输层原理与技术_页面_35](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第5讲：传输层原理与技术_页面_35.jpg)



![第5讲：传输层原理与技术_页面_36](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第5讲：传输层原理与技术_页面_36.jpg)

### 1.2.6. 可靠通信ARQ

![第5讲：传输层原理与技术_页面_37](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第5讲：传输层原理与技术_页面_37.jpg)

ARQ (Automatic Repeat reQuest) 自动重传请求：这表示"重新发送请求"为自动发送并且接收方无需请求发送方重新发送错误段



![第5讲：传输层原理与技术_页面_38](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第5讲：传输层原理与技术_页面_38.jpg)



![第5讲：传输层原理与技术_页面_39](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第5讲：传输层原理与技术_页面_39.jpg)



![第5讲：传输层原理与技术_页面_40](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第5讲：传输层原理与技术_页面_40.jpg)



![第5讲：传输层原理与技术_页面_41](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第5讲：传输层原理与技术_页面_41.jpg)



![第5讲：传输层原理与技术_页面_42](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第5讲：传输层原理与技术_页面_42.jpg)



### 1.2.7. TCP 释放连接

![第5讲：传输层原理与技术_页面_43](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第5讲：传输层原理与技术_页面_43.jpg)



![第5讲：传输层原理与技术_页面_44](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第5讲：传输层原理与技术_页面_44.jpg)



![第5讲：传输层原理与技术_页面_45](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第5讲：传输层原理与技术_页面_45.jpg)

server持续发完数据

FIN = 1，表示数据处理完成

![第5讲：传输层原理与技术_页面_46](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第5讲：传输层原理与技术_页面_46.jpg)



![第5讲：传输层原理与技术_页面_47](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第5讲：传输层原理与技术_页面_47.jpg)



### 1.2.8. 为什么必须等待2MSL

MSL(Maximum Segment Lifetime)报文最长存活时间

![第5讲：传输层原理与技术_页面_48](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第5讲：传输层原理与技术_页面_48.jpg)

1. 为了确保A发送的最后一个ACK可以到达B

2. 防止出现任何无效的连接请求段

   等待2 MSL之后，我们可以确保连接上的所有段均已消失



### 1.2.9. Tcp中的计时器

![第5讲：传输层原理与技术_页面_49](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第5讲：传输层原理与技术_页面_49.jpg)

1. 重传计时器:多长时间进行重传

2. 坚持计时器:避免死锁(WIN = 0的时候修改WIN但是没有办法发送过去)：收到WIN = 0 的时候，开始进行计时，到时间主动询问

3. 保持计时器:

   1. 发送数据段后，刷新
   2. 如果到达一定的时间，则再次询问是不是还要保持连接。
   3. 长期没有数据，和对方协商是否可以终止

4. 时间等待计时器

   



### 1.2.10. TCP的有限状态机

![第5讲：传输层原理与技术_页面_50](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第5讲：传输层原理与技术_页面_50.jpg)

1. 粗线:正常的服务器端
2. 虚线:正常客户端
3. 细线:异常状态的问题



## 1.3. UDP

### 1.3.1. 概述

![第5讲：传输层原理与技术_页面_52](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第5讲：传输层原理与技术_页面_52.jpg)

为什么我们需要UDP？

1. 没有建立连接(避免延时)
2. 简单：发送方，接收方无连接状态
3. 小段header
4. 没有拥塞控制：UDP可以按照期望的速度传输



![第5讲：传输层原理与技术_页面_53](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第5讲：传输层原理与技术_页面_53.jpg)

无连接：没有复杂控制，头部简单

1. UDP发送方，接收方之间没有握手(HandShake，包含进程等信息的)
2. 每个UDP段都独立处理

常用于流媒体(Stream)多媒体(multimedia)应用

1. 容忍损失:无非就是降低帧率
2. 这类应用是**速率敏感**的应用，而不一定是质量敏感的应用。

UDP用于：

1. RIP:定期发送路由信息(periodically)
2. DNS:避免延迟建立TCP连接(DNS需要快速找到)
3. SNMP:SNMP：拥塞时(congestion)，SNMP必须仍然可运行。在没有拥塞和可靠性控制机制的情况下，UDP在这种情况下的性能要优于TCP。(主播和多播，大量信息传输)
4. 其他协议包括TFTP，DHCP

### 1.3.2. UDP帧结构

![第5讲：传输层原理与技术_页面_54](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第5讲：传输层原理与技术_页面_54.jpg)

1. UDP的数据段很简单
2. UDP只有8个字节的首部
3. 源端口、目的端口、长度、校验(data)、Data
4. 校验也要对data一并校验，如果出现错误，直接丢弃。
5. 应用层进行数据切片，决定如何进行发送，UDP直接发送

## 1.4. 应用：NAT和PAT

### 1.4.1. 什么是NAT？

![第5讲：传输层原理与技术_页面_56](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第5讲：传输层原理与技术_页面_56.jpg)

1. NAT，是在IP数据包头中将一个地址交换为另一个地址的过程
   1. 网络地址转换
   2. 是网络地址即将用完的解决方案
2. 实际上，NAT用于允许私下寻址的主机访问Internet。
3. IP地址耗尽的解决方案之一
   1. 保留注册(合法)地址
   2. 连接到Internet时增加灵活性
4. RFC 1631 - Network Address Translator (NAT)



![第5讲：传输层原理与技术_页面_57](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第5讲：传输层原理与技术_页面_57.jpg)

### 1.4.2. NAT类型

![第5讲：传输层原理与技术_页面_58](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第5讲：传输层原理与技术_页面_58.jpg)



### 1.4.3. NAT地址类型

![第5讲：传输层原理与技术_页面_59](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第5讲：传输层原理与技术_页面_59.jpg)



![第5讲：传输层原理与技术_页面_60](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第5讲：传输层原理与技术_页面_60.jpg)

![第5讲：传输层原理与技术_页面_61](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第5讲：传输层原理与技术_页面_61.jpg)

![第5讲：传输层原理与技术_页面_62](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第5讲：传输层原理与技术_页面_62.jpg)

### 1.4.4. NAT优缺点

![第5讲：传输层原理与技术_页面_63](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第5讲：传输层原理与技术_页面_63.jpg)

1. 优点：由于并非每个内部主机都需要同时进行外部访问，因此您可以使用少量的全局唯一地址池来服务相对大量的私有寻址主机。
2. 缺点：**一一映射**，并没有从根本上解决地址短缺的问题。
3. 也就是说，如果专用地址空间为/8，但公用地址为/ 24，则一次只能有254个主机可以访问Internet，主要内网不是同时有很多主机上网，就可以如上操作，进一步降低地址压力(类似并行和串行的区别)



















TCP不支持多播和广播



