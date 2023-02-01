鉴于zhy妈妈的笔记过于全面，此系列笔记仅作一些补充和重点记忆强化。

<!--more-->

# 重点笔记

## 易混的英文单词

| 层         | 单元英文 | 单元中文 |
| ---------- | -------- | -------- |
| 网络层     | packets  | 报文     |
| 数据链路层 | frames   | 帧       |
| 运输层     | segments | 段       |



## OSI 和 TCP/IP模型对比

| 层次       | 特点                     | 关键字                   | 备注         | 对应的层   |
| ---------- | ------------------------ | ------------------------ | ------------ | ---------- |
| 物理层     | 二进制传输               | 信号和介质               | 属于数据流层 | 网络接入层 |
| 数据链路层 | 介质访问                 | 帧和介质访问控制         | 属于数据流层 | 网络接入层 |
| 网络层     | 路径选择                 | 路径选择，最优路径       | 属于数据流层 | 互联网层   |
| 传输层     | 终端到终端通信           | 可靠性，流控制，错误纠正 | 属于数据流层 | 传输层     |
| 会话层     | 进程之间通信如何用户交流 | 对话和交流               | 属于应用层   | 应用层     |
| 展示层     | 展示                     | 标准                     | 属于应用层   | 应用层     |
| 应用层     | 给用户展示交互接口       | 浏览                     | 属于应用层   | 应用层     |



## 网络拓扑Topology

分为物理拓扑和逻辑拓扑



总线bus

环型ring   双环dual ring

星型star

树型tree

渔网型complete（mesh）

蜂窝型cellular 非常低效



## 网络设备

| 名称      | 中文       | 层级   | 应用                                 |
| --------- | ---------- | ------ | ------------------------------------ |
| media     | 介质       | 第一层 | 携带信息流                           |
| repeaters | 中继器     | 第一层 | 延长网络的长度，转发，不做过滤       |
| hubs      | 集线器     | 第一层 | 不解决冲突，转发，不做过滤           |
| bridges   | 网桥       | 第二层 | 在lan上过滤流量，创建冲突域          |
| switches  | **交换机** | 第二层 | 结合网桥和交换机                     |
| routers   | **路由器** | 第三层 | 路径选择，ip逻辑划分，切换到最佳路由 |

![image-20221007092506024](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/image-20221007092506024.png)

hubs 总线式连接

交换机 星型连接



# 计算机互联网及其模型

## Overview of Computer Network

![image-20221205164819506](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/image-20221205164819506.png)

### data networks of classifications

![image-20221205164442393](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/image-20221205164442393.png)

1. 局域网，LAN，Local Area Networks
   - 作用范围比较窄
   - 多用户同时复用链路介质
   - 网络性能比较高(一般是一个公司来管理处理，可以达到GPS甚至是10GPS)
   - 出错率相对比较容易控制(低)
2. 广域网，WAN，Wide Area Networks
   - 在比较大的地理范围上进行连接
   - 要么串行连接(serial links)，要么光链路连接(optical links)
   - 传统上，传输速率比较低，因为一般是多公司管理，标准和介质等都不同
   - 出错率相对比较高

#### LAN Devices



![image-20221205164622432](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/image-20221205164622432.png)

1. Hub 集线器——工作在第一层。多端口中继器，连接PC。重复信号
2. Bridge 网桥——工作在第二层，将局域网分段，进行MAC地址的计算
3. Switch 交换机——工作在第二层，多端口网桥，全带宽，大规模集成电路实现【相对于网桥的优点】
4. Router 路由器——工作在第三层，路径选择，分组交换

#### WAN Devices

![image-20221205164714100](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/image-20221205164714100.png)

1. 路由器——路径选择、分组交换

2. Modem CSU/DSU TA/NT1(点对点连接终端设备)

   CSU/DSU digital-interface device used to connect data terminal equipment (DTE), such as a router to a digital circuit, such as a Digital Signal 1 (DS1) T1 line.

   - 有CSU的功能 channel service unit——The channel service unit(CSU) is responsible for the connection to the telecommunication network将终端用户和本地数字电话环路相连接
   - 有DSU的功能 data service unit ——the data service unit (DSU) is responsible for managing the interface with the DTE 把终端上物理层适配到通讯层上：模拟信号到数字信号进行转换
   - TA/NT1——终端适配器和网络适配器
   - 模拟到数字（analog 模拟）
   - 远端局域网链接

#### LAN/WAN Services

![image-20221205170052016](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/image-20221205170052016.png)



### Internet History

![image-20221205170127521](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/image-20221205170127521.png)



### Internet with Multi-layer ISP structure

![image-20221205170237775](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/image-20221205170237775.png)

![image-20221205170251358](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/image-20221205170251358.png)

ISP(Internet Service Providers)互联网服务提供商

ICP(Internet Content Provider)互联网内容提供商,不提供接入服务

NAP(Network Access Point):第一二层之间的接入点,也可以是google(大公司)直接和第一层ISP进行链接



第一层ISP是核心层，主要负责远距离连通，这种多层ISP结构可以将大量的流量本地化。在低层次的ISP可以解决的问题就不进入上一层进行解决，将大量的流量分流。



### Basic Concepts

#### Data

![image-20221205170625080](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/image-20221205170625080.png)

#### Data Packet

![image-20221205170650722](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/image-20221205170650722.png)

| 层         | 单元英文 | 单元中文 |
| ---------- | -------- | -------- |
| 网络层     | packets  | 报文     |
| 数据链路层 | frames   | 帧       |
| 运输层     | segments | 段       |

#### Protocol

![image-20221205170803254](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/image-20221205170803254.png)

#### source and destination

![image-20221205170827799](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/image-20221205170827799.png)

#### Media Types

![image-20221205170850296](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/image-20221205170850296.png)

传输方式

1. 电缆方式
   - 铜轴电缆方式
   - 双绞线方式
2. 光缆方式:相对比较稳定，高速率传输都是用光缆。
3. 空气方式

#### Digital Bandwidth 带宽

![image-20221205171032640](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/image-20221205171032640.png)

#### throughput 吞吐量

![image-20221205171126428](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/image-20221205171126428.png)

## OSI Reference Model

### 概述

![image-20221205171331468](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/image-20221205171331468.png)

### 层次（重要）

![image-20221205171439821](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/image-20221205171439821.png)



### 原因 Why al layered model?

![image-20221205171652270](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/image-20221205171652270.png)

### 3+4

![image-20221205171814839](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/image-20221205171814839.png)

![image-20221205171838094](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/image-20221205171838094.png)

| 层次                          | 特点                     | 关键字                                                       | 备注         | 对应的层   |
| ----------------------------- | ------------------------ | ------------------------------------------------------------ | ------------ | ---------- |
| the physical layer物理层      | 二进制传输               | signal and media信号和介质                                   | 属于数据流层 | 网络接入层 |
| the data link layer数据链路层 | 介质访问                 | frame, media access control帧和介质访问控制                  | 属于数据流层 | 网络接入层 |
| the network layer网络层       | 路径选择                 | path selection,routing, addressing路径选择，最优路径         | 属于数据流层 | 互联网层   |
| the transport layer传输层     | 终端到终端通信           | reliability, flow control, error correction可靠性，流控制，错误纠正 | 属于数据流层 | 传输层     |
| the session layer会话层       | 进程之间通信如何用户交流 | dialog and conversations对话和交流                           | 属于应用层   | 应用层     |
| the presentation layer展示层  | 展示                     | common format标准                                            | 属于应用层   | 应用层     |
| the application layer应用层   | 给用户展示交互接口       | broswer浏览                                                  | 属于应用层   | 应用层     |

#### Layer1: the Physical Layer

![image-20221205184459680](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/image-20221205184459680.png)

关键词：**信号和介质**

1. 定义终端系统(包括介质media)之间链路的电气和功能规范(specifications)

2. 定义电压电平(voltage levels)、电压变化的定时(timing of voltage changes)、物理数据速率、最大传输距离、物理连接器和其他类似属性。

3. 特点:对于信号不管理，对于信号正确性不做判断，只传递信号。

   

#### Layer 2:The Data Link Layer 数据链路层

![image-20221205185000717](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/image-20221205185000717.png)

关键词:**帧和介质访问控制**

1. 通过物理链路提供**可靠**的数据传输
2. 涉及**物理**(而不是逻辑)寻址、网络拓扑、网络访问、错误通知、帧的有序传递和流控制，调节链路使用(涉及到一系列电路控制)
3. 和第一层区别:需要**检查电信号的正确性**，点对点的线路的链接，比如A-B之间的链接
4. 几个数据链路层:A-B,B-C,如果在两个链路则两个，反之则一个

#### Layer 3:The NetWork 网络层

![image-20221205185111613](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/image-20221205185111613.png)

关键词:**路径选择，最优路径**，基于逻辑**IP*地址的路径选择、路由和寻址，第三层要基于protocol生成路由表。

1. 在路由发生的两个终端系统之间提供连接和路径选择
2. 它们(终端设备)可能位于地理上(geographically)分离的网络上
3. 和第二层区别:
   - 第二层只涉及到物理链路上点对点
   - 第三层上实现的是很多链路上的数据连通和传输。可以跨很远，在广域网上进行链路控制(逻辑电路控制)。
4. IP地址:逻辑地址，由本层分发IP地址。
5. 基于Packets（报文）进行逻辑数据的管理。

####  Layer 4:The Transport Layer 运输层

![image-20221205185148208](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/image-20221205185148208.png)

关键词:**可靠性，流控制，错误纠正**

1. 将数据分段并重新组合(reassembles)为数据流
2. 关于如何在网络上实现可靠的传输
3. 负责终端结点之间的可靠网络通信，并为虚拟电路的建立、维护和终止、传输故障检测和恢复以及信息流控制提供机制
4. 和第三层区别:
   - 第三层实现设备到设备之间的连接，但是我们的操作系统是分时操作系统，需要网络系统进行分时处理，保证为对应的数据进程转发正确的数据。
   - 复杂数据校验交给终端设备，而不是中间设备，中间设备能够完成转发即可，降低工程量
   - 数据错误:请求第三层(下层)重传
   - 互相协商:调整数据传输效率

####  The Layer 5:The Session Layer 会话层

![image-20221205185240403](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/image-20221205185240403.png)

关键词:**对话和交流**

1. 建立、管理和终止通信主机之间的会话
2. 同步表示层实体之间的对话框并管理其数据交换
3. 提供高效的数据传输、服务类别以及会话、表示和应用层问题的异常报告
4. 管理表示层实体之间的数据交换
5. 和前四层相比:
   - 前四层不能处理具体的细节，所以需要我们在应用程序中完成应用的会话管理。
   - checkpoint:在相应时间检查数据是否同步。
   - 多进程的逻辑控制。

####  The Layer 6:The Presentation Layer 表示层

![image-20221205185255539](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/image-20221205185255539.png)

关键词:**标准**，不同标准有可能出现歧义

1. 确保一个系统的应用层发送的信息可以被另一个系统的应用层读取
2. 使用通用数据表示格式在多个数据表示格式之间转换
3. 关注数据结构和数据传输语法(syntax)的协商
4. 负责压缩和加密，防止泄密事情的出现。

####  Layer 7: The Application Layer 应用层

![image-20221205185310352](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/image-20221205185310352.png)

关键词:**浏览**，主要处理用户界面，将操作封装成机器可以理解的形式

1. 最接近用户的一层
2. 为用户应用程序提供网络服务
3. 不向任何其他OSI层提供服务



![image-20221205172339098](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/image-20221205172339098.png)

![image-20221205172400807](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/image-20221205172400807.png)

![image-20221205172428249](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/image-20221205172428249.png)

## TCP/IP Model

![image-20221205172512975](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/image-20221205172512975.png)

美国国防部(DoD,Department of Defense)



![image-20221205172626659](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/image-20221205172626659.png)



### Application应用层

![image-20221205185409442](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/image-20221205185409442.png)

1. 处理高级协议、表示(representation)、编码(encoding)和会话控制(session control)问题，包含7层上三层【应用层、表示层、会话层】的全部功能

2. TCP/IP将所有与应用程序相关的问题合并到一个层中，并确保将这些数据正确打包到下一层

### Transport 传输层 TCP/UDP

![image-20221205185417496](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/image-20221205185417496.png)

1. 处理服务质量的可靠性、流程控制和错误纠正问题。
   - 传输控制协议(TCP, Transmission Control Protocol):代价比较大，效率比较低
   - 用户数据报协议(UDP, User Datagram Protocol)
   - 它将应用层信息打包成称为段segments的单元
2. 对应OSI的第4层：传输层

### Internet IP

![image-20221205185424251](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/image-20221205185424251.png)

1. 目的：从互联网上的任何网络发送源包，使它们独立于路径和网络到达目的地

2. 最佳路径确定和分组交换发生在这一层

3. 网际互联协议(IP,Internet protocol)

4. 和OSI的第三层：网络层对应，报文packets从一方发送给另一方，报文传输经过路由器进行路径选择

### Network Access / host-to-network 网络接入层

![image-20221205185431692](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/image-20221205185431692.png)

1. 也称为主机到网络层。

   - 合并了OSI下面两层:物理层和数据链路层

   - 完成物理实现和物理介质控制

2. 它涉及到IP数据包实际建立一个物理链路，然后再建立另一个物理链路所需的所有问题。

3. 它包括局域网和广域网的技术细节，以及OSI物理层和数据链路层的所有细节

### 常见的TCP/IP 协议

![image-20221205193912670](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/image-20221205193912670.png)

![image-20221205193924098](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/image-20221205193924098.png)

| 协议名称 | 协议全称                       | 中文名               | 备注                                        |
| -------- | ------------------------------ | -------------------- | ------------------------------------------- |
| FTP      | File Transfer Protocol         | 文件传输协议         | -                                           |
| HTTP     | Hypertext Transfer Protocol    | 超文本传输协议       | 主要用于浏览器                              |
| SMTP     | Simple Mail Transfer protocol  | 简单邮件**发送**协议 | 注意是发送                                  |
| DNS      | Domain Name System             | 域名解析系统         | 将域名解析成IP地址                          |
| TFTP     | Trivial File Transfer Protocol | 普通文件传输协议     | 基于UDP，在局域网发送，关于较小的文件的发送 |



### TCP/IP and  OSI 

![image-20221205172828862](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/image-20221205172828862.png)

1. 两者都有层次，网络专业人员需要知道两者，都通过分层的方案来完成具体的实现
2. 两者都有应用层，尽管它们包含非常不同的服务
3. 两者都有相同的传输层和网络层
4. 都采用分组交换(非电路交换)技术
5. OSI是基于报文交换来进行实现的，TCP/IP也是基于报文交换来完成实现的。

![image-20221205172918008](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/image-20221205172918008.png)

1. TCP/IP看起来更简单，因为它有更少的层
2. **TCP/IP协议是Internet发展的标准，因此TCP/IP模型正是因为它的协议才获得了可信性。**
3. 通常网络不是建立在OSI协议之上的，即使OSI模型被用作指南。
4. **TCP/IP标准是大家都在使用的标准的。(实施标准)**，5层和7层都只是讲课使用的
5. 本课程我们一般使用**5层**来进行分割讲解。

## Network Topology

![image-20221205172958683](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/image-20221205172958683.png)

- Physical topology
- Logical topology - token passing

![image-20221205173112068](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/image-20221205173112068.png)

### 总线bus

![image-20221205195348133](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/image-20221205195348133.png)

1. 物理角度:每个主机都连接到一条公用线(总线)。
   - 优点：所有主机都可以直接通信。
   - 缺点：电缆断开会使主机彼此断开连接。
   - 也就是说总线是很重要的，总线一旦断开是不能够通信的，也是不可以分成多段总线进行处理(在未处理的总线上会在断开的地方，反射电信号，形成电路震荡)
2. 逻辑角度：每个网络设备都可以看到来自所有其他设备的所有信号，实际上是**广播式**传播
3. 优点:比较简单，所有的设备都可以监听到总线的信号。
4. 缺点:
   1. 信号冲突，需要进行复杂的介质访问权限控制来保证通信正常
   2. 如果一处断开，则全部无法进行网络传输

### 环型ring   双环dual ring

![image-20221205195359360](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/image-20221205195359360.png)

1. 物理角度
   - 所有的设备直接首尾相连，组成一个菊花链(daisy-chain)
   - 可以将信息传送给链上的所有的设备，但是一般是固定顺时针或者逆时针进行传输
2. 逻辑角度
   - 为了使信息流动，每个站点必须将信息传递给其相邻的站点。
   - 我们需要对于链路进行访问控制，防止很多设备同时使用环，我们使用token来进行控制访问权力
3. 缺点:环上只要有一个地方断开就会破坏整个环
4. 令牌环拓扑主要用于控制领域，比较适用于实时系统的处理

![image-20221205195407101](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/image-20221205195407101.png)

1. 物理视角：
   - 双环拓扑结构与环拓扑结构相同，只是有第二个冗余环**连接相同**的设备。
2. 逻辑视角：
   - 双环拓扑就像两个独立的环，同一时间只有一个环被应用。
   - 有token令牌才有发送权力发送信息(使用总线)
3. 优点：提供可靠性和灵活性
4. Eg.优先使用外环，如果外环出现物理错误，则切换到内环上使用，并且对外环进行物理修复。
5. 双环拓扑是指一个结点有两个点，同时只能一个环在传输信息，两个环的传输时的方向是不能确定的。

### 星型star

![image-20221205195416746](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/image-20221205195416746.png)

1. 物理视角：星型拓扑结构有一个中心节点，所有的链路都从它辐射(radiating)出去。
2. 逻辑视角：所有信息的流动将通过一个设备。
3. 优点: 优点：它允许所有其他节点相互通信，方便。出于安全或限制访问的原因，它也可能是可取的
4. 缺点：如果中心节点出现故障，整个网络就会断开连接。根据使用的网络设备类型，冲突可能是一个问题，中心点会有很大的负担，并且容易造成通信阻塞。
5. 扩展星型拓扑:设置次级中心结点:和Internet的层次结点类似

### 树型tree

![image-20221205195428151](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/image-20221205195428151.png)

1. 树拓扑使用一个主干节点(Trunk Node)，从该节点分支到其他节点。
   - 二叉树(每个节点分成两个链接)
   - 主干树(主干有分支节点，其上挂有链接)。
2. 物理观点：主干是一条有几层分支的电线。
3. 逻辑观点：信息流是层次性的。
4. 在根一级数据结点可以对数据进行汇总和统计
5. 类似电信网络:中心点不仅仅是转存和发送，还要控制和统计，而星形拓扑是不需要控制统计的。
6. 当前节点不能处理的部分，则交给父结点处理

### 渔网型complete（mesh）



![image-20221205195436548](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/image-20221205195436548.png)

1. 物理视角:有明显的优点和缺点
2. 逻辑角度:完整或网格拓扑的行为在很大程度上取决于所使用的设备。
3. 优点：最大的连接性和可靠性。
4. 缺点：链接的媒体数量和到链接的连接数量变得非常庞大。
5. 全连接拓扑
   - 缺点:成本高、路径选择多:添加选择最合理的路径的机制
   - 优点:鲁棒性高，抗干扰能力强。
6. 常使用在比较使用重要的情况下:通常Internet就是使用Mesh的拓扑

### 蜂窝型cellular 

![image-20221205195445290](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/image-20221205195445290.png)

非常低效

1. 物理视角
   - 蜂窝拓扑结构是用于无线技术的拓扑结构
   - 有时接收节点移动(如手机)，有时发送节点移动(如卫星)
2. 逻辑视角:节点之间直接通信(尽管有时非常困难)，或者只与相邻的单元通信，这是**非常低效**的。
3. 每一个结点都是无线的连通方式:远结点需要进行转发
4. 使用场景
   1. 无线电话
   2. 卫星

## Network Devices



NICs - MAC address - layer 2

![image-20221205173500374](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/image-20221205173500374.png)





![image-20221205173327786](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/image-20221205173327786.png)
