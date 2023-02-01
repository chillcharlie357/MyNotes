# Lecture08-路由协议RIP、OSPF

![第8讲：路由协议rip,ospf_页面_01](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第8讲：路由协议rip,ospf_页面_01.jpg)

![第8讲：路由协议rip,ospf_页面_02](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第8讲：路由协议rip,ospf_页面_02.jpg)

## RIP

### RIP历史

![第8讲：路由协议rip,ospf_页面_03](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第8讲：路由协议rip,ospf_页面_03.jpg)

RIP v1被认为是一种**内部网关协议**。

1. RIP v1是一种距离向量协议，它以预定间隔将其整个路由表广播到每个邻居路由器。默认间隔为**30秒**。
2. RIP使用**跳数**作为度量标准，最大跳数为**15**，达到16跳的报文自动抛弃。

RIP v1能够在多达六个等价路径上进行**负载平衡(Load Balancing)**，默认情况下为四个路径，最多6个，跳数相同才能完成负载均衡，跳数不同不满足条件

RIP最初是在RFC 1058中指定的

![第8讲：路由协议rip,ospf_页面_04](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第8讲：路由协议rip,ospf_页面_04.jpg)

RIP v1具有以下限制：

1. 它不会在其更新中发送子网掩码信息:意味着必须用同样的子网掩码，不支持VLSM或无类域间路由(CIDR，Classless Interdomain Routing)。
2. 它以255.255.255.255的广播形式发送更新:只能发给邻居，不能通过路由器转发。
3. 它不支持身份验证(authentication):只要启动RIP就可以接受到信息，也就意味着只要接入网络并且启动RIP进程，就可以了解到整个网络拓扑

### RIP配置

![第8讲：路由协议rip,ospf_页面_05](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第8讲：路由协议rip,ospf_页面_05.jpg)

`router rip`命令选择RIP作为路由协议。

network 命令分配基于NIC的网络地址，路由器将直接连接到该网络地址

![第8讲：路由协议rip,ospf_页面_06](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第8讲：路由协议rip,ospf_页面_06.jpg)

routerA启用端口，可以简化为`1.0.0.0` `2.0.0.0`

### RIP v2

![第8讲：路由协议rip,ospf_页面_07](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第8讲：路由协议rip,ospf_页面_07.jpg)

RIP v2是RIP v1的改进版本，并且新增了以下的功能：

1. 这是一种使用**跳数指标**的距离矢量协议。
2. 它使用**抑制计时器**来防止路由循环-默认值为**180秒**,6倍于交换时间
3. 它使用水平分割(Split Horizon)来防止路由循环(Routing Loops)。
4. 它使用16跳作为**无限距离的度量**。(15跳及以内可达)

### RIPv1 与 RIPv2的比较

![第8讲：路由协议rip,ospf_页面_08](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第8讲：路由协议rip,ospf_页面_08.jpg)

V2和V1不同的特点:

1. 支持有类路由:可以携带子网掩码
2. 使用主播地址`244.0.0.9`进行发送广播:特定给RIP接受，避免了接受后发现没有启动RIP进程耽误时间
3. 需要身份认证才确定是否继续进行接收。

### RIPv2 配置

![第8讲：路由协议rip,ospf_页面_09](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第8讲：路由协议rip,ospf_页面_09.jpg)

network命令导致实现以下三个功能：

1. 路由更新从接口多播。
2. 如果路由更新进入相同的界面，则将对其进行处理。
3. 广播直接连接到该接口的子网。

![第8讲：路由协议rip,ospf_页面_10](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第8讲：路由协议rip,ospf_页面_10.jpg)

需要设置version2

### 验证和故障排除

![第8讲：路由协议rip,ospf_页面_11](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第8讲：路由协议rip,ospf_页面_11.jpg)



![第8讲：路由协议rip,ospf_页面_12](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第8讲：路由协议rip,ospf_页面_12.jpg)

![第8讲：路由协议rip,ospf_页面_13](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第8讲：路由协议rip,ospf_页面_13.jpg)

1. The debug ip ripcommand displays RIP routing updates as they are  sent and received. In this example, the update is sent by 183.8.128.130. debug ip rip命令显示RIP路由更新的发送和接收。 在本示例中，更新是通过183.8.128.130发送的。
2. It reported on three routers, one of which is inaccessible because  its hop count is greater than 15. Updates were then broadcast through  183.8.128.2. 它报告了三台路由器，其中一台无法访问，因为其跳数大于15。然后通过183.8.128.2广播了更新。

![第8讲：路由协议rip,ospf_页面_14](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第8讲：路由协议rip,ospf_页面_14.jpg)

120/1：表示1跳到达，120/2：表示2跳到达



## OSPF Open Shortest Path First

![第8讲：路由协议rip,ospf_页面_16](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第8讲：路由协议rip,ospf_页面_16.jpg)

开放最短路径优先(OSPF，Open Shortest Path First)是基于开放标准的链路状态路由协议。

It is described in several standards of the Internet Engineering  Task Force (IETF) Internet 网络工程任务组(IETF，Internet Engineering Task  Force)的多个标准中对此进行了描述:The most recent description is RFC 2328. 最新的描述是RFC  2328。(已经不是最新的了)

与RIP v1和RIP v2相比，OSPF正在成为首选的IGP协议，因为它具有可伸缩性。

和RIP相比优势比较大，很多网络公司在研究OSPF的优化。

### 路由信息

![第8讲：路由协议rip,ospf_页面_17](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第8讲：路由协议rip,ospf_页面_17.jpg)

1. 链接的状态是对接口及其与其相邻路由器的关系的描述。
2. 链接状态的集合形成一个**链接状态数据库**，有时也称为**拓扑数据库**。
3. 路由器应用**Dijkstra最短路径优先**(SPF)算法来构建以自己为根的SPF树。
4. 路由器通过SPF树计算最佳路径，然后选择最佳路径并将其放置在**路由表**中。

### OSPF vs RIP

![第8讲：路由协议rip,ospf_页面_18](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第8讲：路由协议rip,ospf_页面_18.jpg)

用于大型网络，基于带宽，可以分层(将网络划分成2层)，收敛更快，支持多路负载均衡



![第8讲：路由协议rip,ospf_页面_19](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第8讲：路由协议rip,ospf_页面_19.jpg)

- 上面带宽大，2跳达到，下面带宽小，1跳到达。
- OSPF从上面走，RIP从下面走，但是上面会快一些

### OSPF 特征

![第8讲：路由协议rip,ospf_页面_20](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第8讲：路由协议rip,ospf_页面_20.jpg)

1. OSPF的特征克服了这些限制
   1. 更健壮
   2. 更具可扩展性
2. 大型OSPF网络使用分层设计。
   1. 将大的网络分成多个area，每一个area只和area 0相连，保证area没有回路
   2. 层次最多只有2个，一个area就是area 0。
   3. 层次维持树的关系

### OSPF术语

![第8讲：路由协议rip,ospf_页面_21](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第8讲：路由协议rip,ospf_页面_21.jpg)

Link:两个设备之间的物理链路

![第8讲：路由协议rip,ospf_页面_22](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第8讲：路由协议rip,ospf_页面_22.jpg)

- Neighbors:相邻的路由器
- Link-State:物理链路的信息:路由器连接关系、通过什么接口、链路带宽、网络类型(点对点、多路复用)等
- 不同网络类型处理代价不同

![第8讲：路由协议rip,ospf_页面_23](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第8讲：路由协议rip,ospf_页面_23.jpg)

Cost:不同网络链路处理的时候的代价，和链路带宽相关，成反比关系，一般是固定值除以带宽

![第8讲：路由协议rip,ospf_页面_24](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第8讲：路由协议rip,ospf_页面_24.jpg)

Area:一个有很多路由器的端口都属于的区域(相同)

![第8讲：路由协议rip,ospf_页面_25](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第8讲：路由协议rip,ospf_页面_25.jpg)

Autonomous System:多个Area形成一个自治系统

![第8讲：路由协议rip,ospf_页面_26](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第8讲：路由协议rip,ospf_页面_26.jpg)



![第8讲：路由协议rip,ospf_页面_27](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第8讲：路由协议rip,ospf_页面_27.jpg)

![第8讲：路由协议rip,ospf_页面_28](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第8讲：路由协议rip,ospf_页面_28.jpg)

- Neighbours必须在一个Area中才算是，Neighbour之间交换Topology Databases
- 一个Area中获得全部LS(Link State)后计算Tree，生成表

![第8讲：路由协议rip,ospf_页面_29](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第8讲：路由协议rip,ospf_页面_29.jpg)

- DR:指定路由器，只有在多路复用的情况下使用
- BDR:如果DR坏了，再次选举会出现问题，如果DR损坏，BDR立即成为DR

### OPSF域

![第8讲：路由协议rip,ospf_页面_30](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第8讲：路由协议rip,ospf_页面_30.jpg)

1. 区域用32位数字标识
   1. 可以是IP格式，也可以是一个十进制值
   2. 区域0或区域0.0.0.0
2. 区域0：区域编号为0的单个区域
3. OSPF使用2级分层模型：逻辑上必须是2层结构，而物理实现上可能有一定的差异，如果更多需要进行逻辑配置。
4. 在多区域OSPF网络中，要求所有区域都连接到区域0(主干)
5. Example:Area是和端口相关(注意端口)

![第8讲：路由协议rip,ospf_页面_31](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第8讲：路由协议rip,ospf_页面_31.jpg)

### OSPF行为

![第8讲：路由协议rip,ospf_页面_32](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第8讲：路由协议rip,ospf_页面_32.jpg)

1. OSPF使用邻居的邻接关系(Adjacencies)来全面了解网络。
2. OSPF操作包括五个步骤：
   1. 步骤1：建立邻接关系
   2. 步骤2：选择DR和BDR(如果需要):多路复用的时候才需要
   3. 步骤3：发现路线
   4. 步骤4：选择适当的路线
   5. 步骤5：维护路线信息
3. OSPF具有七个状态。简而言之，它们是：
   1. Init, 2Way, Ex Start, Exchange, Loading, Full
   2. 初始化，双向操作，预先启动，交换，加载，完成

![第8讲：路由协议rip,ospf_页面_33](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第8讲：路由协议rip,ospf_页面_33.jpg)

### 选择DR和BDR

![第8讲：路由协议rip,ospf_页面_34](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第8讲：路由协议rip,ospf_页面_34.jpg)

OSPF网络类型

1. 广播多路复用网络，例如以太网
2. 点对点网络
3. 非广播多路复用网络(NBMA, Nonbroadcasr multi-access)

![第8讲：路由协议rip,ospf_页面_35](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第8讲：路由协议rip,ospf_页面_35.jpg)

只有多路复用才需要选择DR和BDR

原先，每一个都要建立10(5 * 4/2)个链接，如果有了DR就只需要4个连接

对于所有OSPF路由器，DR使用224.0.0.5(自己的IP)的**主播地址**向该网段上的所有其他路由器发送链接状态信息。

为确保DR/BDR看到所有路由器在网段上发送的链接状态，使用了所有DR/BDR的多播地址224.0.0.6。(DR和BDR之间）

![第8讲：路由协议rip,ospf_页面_36](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第8讲：路由协议rip,ospf_页面_36.jpg)



### OSPF报文

![第8讲：路由协议rip,ospf_页面_37](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第8讲：路由协议rip,ospf_页面_37.jpg)



![第8讲：路由协议rip,ospf_页面_38](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第8讲：路由协议rip,ospf_页面_38.jpg)

包，并继续以固定的时间间隔(intervals)发送hello。

控制(govern)OSPF hello数据包交换的规则称为Hello协议。

Hello数据包的地址为224.0.0.5。

默认情况下，广播多路访问和点对点网络上**每10秒**发送一次Hello报文。

在连接到NBMA网络的接口(例如帧中继)上，默认时间是30秒。

保持心跳，确定还活着。Hello几乎是空报文，给所有跑OSPF的路由器发送

![第8讲：路由协议rip,ospf_页面_39](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第8讲：路由协议rip,ospf_页面_39.jpg)

![第8讲：路由协议rip,ospf_页面_40](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第8讲：路由协议rip,ospf_页面_40.jpg)

### 哪个路由器将成为DR？

![第8讲：路由协议rip,ospf_页面_41](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第8讲：路由协议rip,ospf_页面_41.jpg)

1. 优先级+路由器ID，最大的是DR，第二大的是BDR。
2. 优先级：1-255，默认值:1
3. 路由器ID
   1. 环回IP地址(逻辑端口)，避免端口宕机出现问题。
   2. 如果没有环回IP地址，则接口IP为最高值地址(Active的端口上的IP作为参考)
   3. 如果接口出现故障，则路由器必须重新建立邻接关系并重新转换(readvertising)LSA

### OSPF机制

![第8讲：路由协议rip,ospf_页面_42](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第8讲：路由协议rip,ospf_页面_42.jpg)



![第8讲：路由协议rip,ospf_页面_43](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第8讲：路由协议rip,ospf_页面_43.jpg)

- 准备交换数据库(Exstart Starts)
- 首先确认主方(发送方)、从方(接受方)，保证数据有序，简单就是谁的Router ID高
- Router ID高的(主方)发送自己DBD报文，从方对主方发送的DBD接受处理并发送

![第8讲：路由协议rip,ospf_页面_44](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第8讲：路由协议rip,ospf_页面_44.jpg)

交换完成后，各自检查自己是不是有全部的信息

- 如果有完整的信息，则发送LSAck
- 如果发现有没有的，则发送LSR，等待LSU(整个链路的详细信息，不是LSA)来进行学习，之后收到完成后发送LSAck

### OSPF操作

![第8讲：路由协议rip,ospf_页面_45](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第8讲：路由协议rip,ospf_页面_45.jpg)

#### 建立邻接关系

![第8讲：路由协议rip,ospf_页面_46](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第8讲：路由协议rip,ospf_页面_46.jpg)

路由器每隔一段时间发送一次hello数据包,Hello报文的TTL是1，表明不会跨路由传播。

如果邻居被发现了：将邻居添加到邻居数据库

发现网络类型

1. 如果是多路复用网络，进入DR/BDR选举过程，然后进入步骤2。
2. 如果是点对点或点对多点网络，则不会举行DR/BDR选举过程，并跳过步骤2。
3. 如果hello数据包标头中的DR/BDR字段已被占用(即DR / BDR对已经存在)，则不会进行DR/BDR选举，并跳过步骤2。

如果对方的DP/BDP优于我的DP/BDP，则接受对方的

#### 选举DR和BDR

![第8讲：路由协议rip,ospf_页面_47](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第8讲：路由协议rip,ospf_页面_47.jpg)

1. 如果没有其他路由器联机，则该路由器将成为DR。下一个要"启动"的路由器将是BDR。
2. 如果多个路由器(两个或更多)同时联机，则
   1. 优先级最高的路由器成为DR：优先级为零表示"从不DR"
   2. 如果存在平局，则具有最高路由器ID的路由器将成为DR：路由器ID是最高的环回或接口IP地址
   3. 具有第二高优先级或路由器ID的路由器成为BDR

![第8讲：路由协议rip,ospf_页面_48](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第8讲：路由协议rip,ospf_页面_48.jpg)

1. 如果DR无效，则BDR变为DR。
2. 然而
   1. 如果新的OSPF路由器以更高的优先级或路由器ID加入网络，则当前的DR和BDR**不会更改**。
   2. 仅当当前DR失败时，它才成为新的BDR；或者仅当当前DR和BDR失败时，才成为新的DR。

#### 发现路线

![第8讲：路由协议rip,ospf_页面_49](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第8讲：路由协议rip,ospf_页面_49.jpg)

1. 这一步从Ex Start状态转换到完整状态
2. 路由器确定"主/从(master/slave)"关系
3. 多路复用网络中的DR/BDR交换LSA，并且所有其他DR将其Type 2 DBD发送给DR/BDR。
4. 如有必要，路由器可以通过发送请求更多信息的LSR进入负载状态:所有路由器必须在"加载状态"中等待，直到完全更新请求的路由器。
5. 路由器现在进入完整状态



#### 选择适当的路线

![第8讲：路由协议rip,ospf_页面_50](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第8讲：路由协议rip,ospf_页面_50.jpg)

现在，将与网络上的所有其他路由器并行地计算SPF算法。

1. 切记：在发生这种情况之前，所有路由器必须具有相同的链接状态数据库。
2. SPF使用Cost作为指标
3. SPF将从其自身到目的地的每条路径的成本相加，并以路由器为根来构建树
4. OSPF然后在路由表中安装成本最低的路径：最多将安装4条等价路径以进行负载共享



#### 维护路由信息

![第8讲：路由协议rip,ospf_页面_51](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第8讲：路由协议rip,ospf_页面_51.jpg)

1. 常规的Hello交换是OSPF用于检测新邻居或故障(downed)邻居的机制。
2. 根据网络的类型，Hello数据包以不同的默认间隔发送。(确定对方是不是还好)
   1. 对于速度为T1(1.544 Mbps)或更高的链接，每10秒：广播多路访问和点对点链接
   2. 对于小于T1的链接，每30秒：非广播多路访问链接
   3. "死间隔"是问候间隔的四倍。(如果在这样子对方还没有成功则对方死了)

### 链路状态出现变化

![第8讲：路由协议rip,ospf_页面_52](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第8讲：路由协议rip,ospf_页面_52.jpg)

- Router A tells all OSPF DRs on 224.0.0.6
- Event触发交换:比如A连接的网段断掉了
- A使用LSU告知**DR**

![第8讲：路由协议rip,ospf_页面_53](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第8讲：路由协议rip,ospf_页面_53.jpg)

- DR tells others on 224.0.0.5
- DR 通过LSU告知所有的路由器

![第8讲：路由协议rip,ospf_页面_54](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第8讲：路由协议rip,ospf_页面_54.jpg)

如果B连接了别的Area，则继续进行交换

![第8讲：路由协议rip,ospf_页面_55](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第8讲：路由协议rip,ospf_页面_55.jpg)

所有的路由信息交换完毕后，同时更新路由表

### 基本的OSPF配置

![第8讲：路由协议rip,ospf_页面_56](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第8讲：路由协议rip,ospf_页面_56.jpg)

1. 在路由器上启动OSPF
   1. `Router (config)# router ospf process-id`
   2. 进程号:process-id
      1. 取值: 1 ~ 65535
      2. 在一台路由器上识别多个OSPF进程
      3. 通常在整个AS(自治系统)中保持相同的进程ID
2. 在路由器上识别IP网络
   1. `Router (config-router) # network address wildcardmask area area-id`
   2. 网络地址可以是整个网络，子网或接口的地址。
   3. address:IP地址



![第8讲：路由协议rip,ospf_页面_57](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第8讲：路由协议rip,ospf_页面_57.jpg)

- 只有一个Area，则为0
- **Wild-card Mask和子网掩码相反**:子网掩码是255.255.255.0，则Wild-card Address就是0.0.0.255
- 写IP和写网段最后都是一样的

### 配置回路地址

![第8讲：路由协议rip,ospf_页面_58](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第8讲：路由协议rip,ospf_页面_58.jpg)

为OSPF路由器ID添加稳定性

1. 必须在OSPF进程开始之**前**配置回环接口:会涉及到主从关系确定和DR的选举
2. 配置环回地址时，请使用/32掩码以避免潜在的路由问题
3. I建议您在基于OSPF的网络中的所有关键路由器上使用环回地址(专用或公用地址)。
4. 一旦配置立刻生效，不需要no shutdown的命令即可



![第8讲：路由协议rip,ospf_页面_59](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第8讲：路由协议rip,ospf_页面_59.jpg)

![第8讲：路由协议rip,ospf_页面_60](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第8讲：路由协议rip,ospf_页面_60.jpg)

操纵DR/BDR选举

1. `Router (config-if) # ip ospf priority number`
2. 优先级:越大越高
   1. 值：0-255,默认为1
   2. 优先级0表示接口不能被选为DR或BDR

操作OSPF的端口的优先级：`Router # show ip ospf [interface type number]`

![第8讲：路由协议rip,ospf_页面_61](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第8讲：路由协议rip,ospf_页面_61.jpg)

###  OSOF成本 = 标准

![第8讲：路由协议rip,ospf_页面_62](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第8讲：路由协议rip,ospf_页面_62.jpg)

1. Cost适用于所有路由器连接路径
2. 16位数字(1 – 65,535)
3. 较低的Cost->更理想
4. 路径决定是基于路径的总成本。
5. 指标受到带宽的影响
6. 用一个很大的数字去除以当前的带宽得到代价，计算方法如下

### OSPF Path Cost

![第8讲：路由协议rip,ospf_页面_63](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第8讲：路由协议rip,ospf_页面_63.jpg)



![第8讲：路由协议rip,ospf_页面_64](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第8讲：路由协议rip,ospf_页面_64.jpg)

1. 需要更改成本的常见情况是在多供应商(multi-vendor)路由环境中。成本更改将确保一个供应商的成本值与另一供应商的成本值匹配。
2. 另一种情况是使用千兆以太网。默认成本将最低成本值1分配给100 Mbps链路。

### 设置OSPF计时器

![第8讲：路由协议rip,ospf_页面_65](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第8讲：路由协议rip,ospf_页面_65.jpg)

OSPF区域中的所有路由器必须在相同的hello间隔和相同的死间隔上达成一致，默认情况下：

1. T1或更高链接(广播)为10秒
2. 慢于T1的链接为30秒(非广播)
3. 死亡间隔= **4** *问候间隔

![第8讲：路由协议rip,ospf_页面_66](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第8讲：路由协议rip,ospf_页面_66.jpg)



