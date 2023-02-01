# Lecture9- 局域网交换与VLAN

## 交换机

### 交换机基本功能

![第9讲：局域网交换与vlan_页面_03](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第9讲：局域网交换与vlan_页面_03.jpg)

1. 根据MAC地址建立和维护**交换表**(类似于网桥表)
2. 将帧切换出接口到目标

### 对称交换

![第9讲：局域网交换与vlan_页面_04](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第9讲：局域网交换与vlan_页面_04.jpg)

1. 对称交换可在具有相同带宽(10/10 Mbps或100/100 Mbps)的端口之间提供交换连接
2. 用户尝试访问其他网段上的服务器时，可能会导致瓶颈(对称交换可能会导致带宽不足)

### 不对称交换

![第9讲：局域网交换与vlan_页面_05](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第9讲：局域网交换与vlan_页面_05.jpg)

1. 通过将带有服务器的网段连接到**更高带宽的端口(100 Mbps)**，非对称交换(asymmetric switching)减少了服务器上潜在瓶颈的可能性
2. 非对称交换需要在交换器中进行内存缓冲
3. 非对称交换端口解决对称交换端口中的对称阻塞问题(进一步保证了服务器的稳定实现)

### 内存缓存

![第9讲：局域网交换与vlan_页面_06](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第9讲：局域网交换与vlan_页面_06.jpg)

1. 交换机中存储目标和传输数据的内存区域，直到可以将其切换出正确的端口为止。
   1. 基于端口(Port)的内存缓冲
      1. 数据包存储在每个端口的队列中
      2. 由于目标端口繁忙，一个数据包可能会延迟其他数据包的传输
      3. 其他端口存在不均衡的问题。
   2. 共享(Shared)内存缓冲
      1. 所有端口共享的公用内存缓冲
      2. 允许将数据包在一个端口上接收并在另一个端口上发送出去，而无需将其更改为其他队列。
      3. 需要自己记录端口的信息
2. 发生阻塞的时候，根据情况按照端口或者内存将包缓存下来

### 交换方式

![第9讲：局域网交换与vlan_页面_07](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第9讲：局域网交换与vlan_页面_07.jpg)

储存转发(Store-and-Forward，网桥、路由器等通过软件的设备)

1. 交换机**接收整个帧**，最后将其计算为CRC，然后再将其发送到目的地
2. 接收后，校验，正确再发送

Cut-through 直通

1. 转发会增加延迟:通过使用直通切换方法可以减少它
2. 快速转发切换:仅在立即转发帧之前检查目标MAC(只看到**帧的目的地址**就转发)
3. 碎片释放：读取前64个字节去减少碰撞和帧碎片，在转发帧之前

![第9讲：局域网交换与vlan_页面_08](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第9讲：局域网交换与vlan_页面_08.jpg)

三种查看方式

### 第二层交换机

![第9讲：局域网交换与vlan_页面_09](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第9讲：局域网交换与vlan_页面_09.jpg)

1. 大规模集成电路,保证链路效率,低时延,低成本
2. 有一个MAC地址

### 第三层交换机

![第9讲：局域网交换与vlan_页面_10](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第9讲：局域网交换与vlan_页面_10.jpg)

基于硬件的帧转发机制，较高的帧转发性能，低时延

较高速的计算

每一个端口的代价低

流控制

安全性更高

对数据流进行路由，生成MAC和IP的映射，直接经过第二层（？，智能性较差

### 第四层交换机

![第9讲：局域网交换与vlan_页面_11](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第9讲：局域网交换与vlan_页面_11.jpg)

### 多层交换机协议

![第9讲：局域网交换与vlan_页面_12](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第9讲：局域网交换与vlan_页面_12.jpg)



## STP the Spanning-Tree Protocol 生成树协议

![第9讲：局域网交换与vlan_页面_13](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第9讲：局域网交换与vlan_页面_13.jpg)

### 桥回路

![第9讲：局域网交换与vlan_页面_14](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第9讲：局域网交换与vlan_页面_14.jpg)

 

1. 出于各种原因，网络中可能会出现环路。
   1. 通常，网络中的环路是**故意提供冗余**的结果。
   2. 也可能由于配置错误而发生：在桥接网络中，环路可能是绝对灾难性的两个主要原因：
      1. 广播回路(广播风暴)，没有TTL
      2. 路由表的错误

### 冗余造成了路由回路

![第9讲：局域网交换与vlan_页面_15](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第9讲：局域网交换与vlan_页面_15.jpg)

### 第二层路由回路

![第9讲：局域网交换与vlan_页面_16](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第9讲：局域网交换与vlan_页面_16.jpg)

1. 广播和第2层回路可能是危险的组合。
2. 以太网帧没有TTL字段
3. 以太网帧开始循环后，它可能会继续下去，直到有人关闭其中一台交换机或断开链路为止(外部条件)
4. 交换机将抖动(flip flop)主机A的桥接表条目(创建极高的CPU利用率)。
5. 消耗CPU和内存



#### 泛洪单播帧

![第9讲：局域网交换与vlan_页面_17](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第9讲：局域网交换与vlan_页面_17.jpg)

- 过一段时间CAT-1和CAT-2没有收到Host-B的信息，删除表中的对应记录
- 在这之后，Host A发送给Host B信息，然后在CAT-1和CAT-2之间进行循环

### 生成树概述

![第9讲：局域网交换与vlan_页面_18](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第9讲：局域网交换与vlan_页面_18.jpg)

1. 生成树协议的元素
   1. 主要功能：在**交换/桥接网络**中允许**冗余路径**，而不会因环路的影响而引起延迟。
   2. STP通过计算**稳定的生成树**网络拓扑来防止环路
   3. **生成树帧**(称为桥协议数据单元-BPDU)用于确定生成树拓扑
2. 在正常情况下禁用一些端口来防止出现冗余

#### 决策顺序

![第9讲：局域网交换与vlan_页面_19](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第9讲：局域网交换与vlan_页面_19.jpg)

生成树始终使用相同的四步决策序列：

1. 在拓扑里面最低的root BID(网桥标识)【找到root路由器】
2. 找到 Root bridgh的最低路径成本
3. 每个路径都会选择一个最低BID的sender 这个是针对一个链路的，详见例子
4. 每个路径再指定一个最低的ID端口

#### BPDU Bridge Protocol Data Unit

![第9讲：局域网交换与vlan_页面_20](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第9讲：局域网交换与vlan_页面_20.jpg)

STP建立一个称为**根网桥的根节点**

生成的树源自根桥。

不属于最短路径树的冗余连接将被阻止。(block 端口，不转发，但是接收)（只会有一条路最短）

在阻塞的链接上收到的数据帧将被丢弃。

交换机发送的允许形成无环逻辑拓扑的消息是BPDU



#### stp bpdu

![第9讲：局域网交换与vlan_页面_21](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第9讲：局域网交换与vlan_页面_21.jpg)



#### Bridge Identification/BID

![第9讲：局域网交换与vlan_页面_22](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第9讲：局域网交换与vlan_页面_22.jpg)

1. 网桥ID(BID)：8个字节(2 + 6)
   1. 高阶BID子字段(2个字节)：网桥优先级
      1. 216个可能的值：0-65,535(默认值：32,768)
      2. 通常以十进制格式表示
   2. 低阶子字段(6个字节)：分配给交换机的MAC地址，以十六进制格式表示
2. STP成本值：成本越低越好。

#### 选举根路由器

![第9讲：局域网交换与vlan_页面_23](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第9讲：局域网交换与vlan_页面_23.jpg)

1. 交换机通过查找具有**最低BID**的交换机(通常称为根战争)来选择单个根交换机。
2. 如果所有交换机都使用默认的网桥优先级32768，则最低的MAC地址将作为平局。
3. 配置优先级来调整根桥

### 路径代价cost

![第9讲：局域网交换与vlan_页面_24](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第9讲：局域网交换与vlan_页面_24.jpg)

### 5个STP的状态

![第9讲：局域网交换与vlan_页面_25](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第9讲：局域网交换与vlan_页面_25.jpg)

1. 通过根据策略配置每个端口来建立状态
2. 然后，STP根据流量模式(traffic Patterns)和潜在环路(Protential Loops)修改状态
3. STP状态的默认顺序为：
   1. 阻塞:没有转发帧，听到了BPDU
   2. 监听:不转发任何帧，监听数据帧(确定自己可以参加的交换)，也会发送一些数据帧表示自己状态变了
   3. 学习:不转发帧，学习地址
   4. 转发:转发帧，学习地址
   5. 禁用:没有转发帧，没有听到BPDU

![image-20230109113837280](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/image-20230109113837280.png)

![image-20230109113900889](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/image-20230109113900889.png)

### 初始STP收敛

![第9讲：局域网交换与vlan_页面_26](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第9讲：局域网交换与vlan_页面_26.jpg)

1. 当网络首次启动时，所有网桥都会混合使用BPDU信息来泛洪网络。(开始泛洪BPDU信息)
2. 立即，他们应用决策序列，允许他们BPDU进行PK，然后选择出来ROOT，从而形成整个网络的单个生成树。

```
(Step 1) 根交换机决定：选择一个根桥作为该网络的中心点
(Step 2) 选择根端口：所有剩余的网桥都会计算出一组根端口
(Step 3) 选择指定端口：其余所有网桥计算一组指定端口
```

#### 步骤1 根交换机决定

![第9讲：局域网交换与vlan_页面_27](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第9讲：局域网交换与vlan_页面_27.jpg)

1. （所有路由器都）宣布自己为根
2. 检查端口上收到的所有BPDU以及将在该端口上发送的BPDU
3. 对于每个到达的BPDU，如果其值小于为端口保存的现有BPDU
4. 旧值被替换（由于cat-A的BID最小，所有BC均替换）
5. BPDU的发送者被接受为新的根

#### 步骤2：选择根端口

![第9讲：局域网交换与vlan_页面_28](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第9讲：局域网交换与vlan_页面_28.jpg)

1. 每个非根桥必须选择一个根端口。
   1. 桥的根端口是最接近根桥的端口。
   2. 根路径成本是到根网桥的所有链接的累积(cumulative)成本。

#### 步骤3：选择网段的指定端口

![第9讲：局域网交换与vlan_页面_29](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第9讲：局域网交换与vlan_页面_29.jpg)

1. 每个网段都有一个指定的端口：充当单个网桥/交换机端口，该端口既向该网段又向根网桥发送流量，也从该网段和根网桥接收流量。
2. 包含给定网段的指定端口的网桥/交换机称为该网段的指定网桥。
3. 所有网桥/交换机将阻止它们上未指定的端口，根网桥上的每个活动端口都将成为指定端口
4. 每个链路只有一个指定端口，一旦选定其他就block了

![第9讲：局域网交换与vlan_页面_30](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第9讲：局域网交换与vlan_页面_30.jpg) 



![第9讲：局域网交换与vlan_页面_31](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第9讲：局域网交换与vlan_页面_31.jpg)



## Vlan

### vlan介绍

#### 现有的共享局域网配置

![第9讲：局域网交换与vlan_页面_33](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第9讲：局域网交换与vlan_页面_33.jpg)

1. 在典型的共享局域网中…
   1. 根据用户所插入(plug)的集线器对用户进行物理分组
   2. 路由器分割局域网并提供广播防火墙
2. 在虚拟局域网中
   1. 您可以按使用的功能，部门或应用程序对用户进行逻辑分组
   2. 通过专有软件进行配置

#### LAN和VLAN之间的差异

![第9讲：局域网交换与vlan_页面_34](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第9讲：局域网交换与vlan_页面_34.jpg)

虚拟局域网

1. 在第2层和第3层工作
2. 控制网络广播
3. 允许用户由网络管理员分配。
4. 提供更严格的网络安全性。

#### vlan标准

![第9讲：局域网交换与vlan_页面_35](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第9讲：局域网交换与vlan_页面_35.jpg)

特点

1. 不限于物理交换机网段的网络设备或用户的**逻辑分组**。
2. VLAN中的设备或用户可以按功能，部门，应用程序等进行分组，而**不管其物理网段的位置**如何。
3. VLAN**创建一个不限于物理网段**的单个广播域，并且将其视为子网。
4. VLAN设置是由网络管理员使用供应商的软件在交换机中完成的。

#### 分组用户

![第9讲：局域网交换与vlan_页面_36](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第9讲：局域网交换与vlan_页面_36.jpg)

1. VLAN可以从逻辑上将用户划分为不同的子网(广播域)
2. 广播帧仅在具有相同VLAN ID的一个或多个交换机的端口之间切换。(VLAN ID属于端口)
3. 可以通过基于以下内容的软件对用户进行逻辑分组：
   1. 端口号
   2. MAC地址
   3. 使用的协议
   4. 使用的应用

#### 有无vlan的网络广播

![第9讲：局域网交换与vlan_页面_37](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第9讲：局域网交换与vlan_页面_37.jpg)

![第9讲：局域网交换与vlan_页面_38](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第9讲：局域网交换与vlan_页面_38.jpg)

faculty studeng分别在自己的vlan中传输

#### vlan间通信

![第9讲：局域网交换与vlan_页面_39](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第9讲：局域网交换与vlan_页面_39.jpg)

![第9讲：局域网交换与vlan_页面_40](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第9讲：局域网交换与vlan_页面_40.jpg)

### vlan结构

![第9讲：局域网交换与vlan_页面_41](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第9讲：局域网交换与vlan_页面_41.jpg)

#### backbone

![第9讲：局域网交换与vlan_页面_42](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第9讲：局域网交换与vlan_页面_42.jpg)

1. VLAN配置需要支持互连的路由器和交换机之间的骨干数据传输。
2. 骨干网是用于VLAN间通信的区域
3. 骨干网应该是高速链路，通常为100Mbps或更高
4. BackBone可以跑多个VLAN，是骨干网



#### vlan中路由器的作用

![第9讲：局域网交换与vlan_页面_43](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第9讲：局域网交换与vlan_页面_43.jpg)

1. 路由器提供不同VLAN之间的连接
2. 例如，您有VLAN1和VLAN2。
   1. 在交换机内，位于不同VLAN上的用户无法相互通信(VLAN的好处！)
   2. 但是，VLAN1上的用户可以向VLAN2上的用户发送电子邮件，但他们需要路由器才能执行此操作。

#### 在vlan中帧的作用

![第9讲：局域网交换与vlan_页面_44](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第9讲：局域网交换与vlan_页面_44.jpg)

1. 交换机根据帧中的数据做出过滤和转发决策。
2. 使用了两种技术
   1. 帧过滤：检查有关每个帧的特定信息(MAC地址或第3层协议类型),特定的VLAN记录或者映射
   2. 帧标记：在整个网络骨干网中转发时，在每个帧的标题中放置一个唯一的标识符。

##### 帧过滤

![第9讲：局域网交换与vlan_页面_45](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第9讲：局域网交换与vlan_页面_45.jpg)





##### 帧标记

![第9讲：局域网交换与vlan_页面_46](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第9讲：局域网交换与vlan_页面_46.jpg)

1. 帧标记实施过程：
   1. 在整个网络骨干网中转发时，在每个帧的标题中放置一个VLAN标识符。
   2. 每个开关都可以理解和检查标识符。
   3. 当帧离开网络骨干网时，交换机会在帧发送到目标终端站之前删除标识符。只和端口绑定，而不影响主机
2. 帧标记在第2层起作用

![第9讲：局域网交换与vlan_页面_47](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第9讲：局域网交换与vlan_页面_47.jpg)

**主机并不知道vlan的存在**

##### 帧标签标准

![第9讲：局域网交换与vlan_页面_48](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第9讲：局域网交换与vlan_页面_48.jpg)

IEEE802.1Q:IEEE标准，在标头中插入VLAN的标签以标识所属的VLAN。(帧标记)。

ISL(Inter-Switch Link)：思科专有。ISL在数据帧的前面添加一个26字节的标头，并在末尾附加一个CRC(4字节)。

### vlan实现

![第9讲：局域网交换与vlan_页面_49](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第9讲：局域网交换与vlan_页面_49.jpg)



![第9讲：局域网交换与vlan_页面_50](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第9讲：局域网交换与vlan_页面_50.jpg)

实现VLAN的两种方法

1. 静态的
2. 动态的

每一个端口绑定给一个VLAN

1. 确保不共享同一VLAN的端口不共享广播。
2. 确保共享相同VLAN的端口将共享广播

实现途径:

1. 基于端口的虚拟局域网
2. 基于MAC地址的虚拟局域网
3. 基于IP地址的虚拟局域网
4. 基于上层协议的虚拟局域网

#### 静态vlan

![第9讲：局域网交换与vlan_页面_51](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第9讲：局域网交换与vlan_页面_51.jpg)



![第9讲：局域网交换与vlan_页面_52](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第9讲：局域网交换与vlan_页面_52.jpg)

定义：静态VLAN是指将交换机上的**端口**管理性地分配给VLAN的时间

优点：

1. 安全，易于配置和监控
2. 在控制移动的网络中效果很好

#### 动态vlan

![第9讲：局域网交换与vlan_页面_53](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第9讲：局域网交换与vlan_页面_53.jpg)



![第9讲：局域网交换与vlan_页面_54](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第9讲：局域网交换与vlan_页面_54.jpg)

1. 当工作站最初连接到未分配的端口时，交换机会检查表中的条目，并使用正确的VLAN动态配置端口
2. 优点
   1. 添加或移动用户时减少管理(更多前期工作)
   2. 集中通知未授权用户

#### 以端口为中心的vlan

![第9讲：局域网交换与vlan_页面_55](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第9讲：局域网交换与vlan_页面_55.jpg)



![第9讲：局域网交换与vlan_页面_56](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第9讲：局域网交换与vlan_页面_56.jpg)

1. 同一VLAN中的所有节点都连接到同一路由器接口
2. 使管理更容易，因为…
   1. 通过路由器端口分配用户
   2. VLAN易于管理。
   3. 提供更高的安全性
   4. 数据包不会"泄漏"到其他域

#### access and trunk links

![第9讲：局域网交换与vlan_页面_57](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第9讲：局域网交换与vlan_页面_57.jpg)

分为两类:

1. 接入链路:通过一个VLAN报文
2. 骨干链路:通过多个VLAN报文

##### access

![第9讲：局域网交换与vlan_页面_58](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第9讲：局域网交换与vlan_页面_58.jpg)

1. 访问连接是仅作为一个VLAN成员的交换机上的连接。
2. 此VLAN被称为端口的本机VLAN，连接到端口的任何设备都完全不知道VLAN存在。

##### trunk links

![第9讲：局域网交换与vlan_页面_59](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第9讲：局域网交换与vlan_页面_59.jpg)

1. 主干链路能够支持多个VLAN。
2. 主干链路通常用于将交换机连接到其他交换机或路由器。
3. 交换机在快速以太网和千兆位以太网端口上都支持骨干链路。
4. 也存在访问和骨干链接
5. 一般Trunk就是BackBone

![第9讲：局域网交换与vlan_页面_60](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第9讲：局域网交换与vlan_页面_60.jpg)



![第9讲：局域网交换与vlan_页面_61](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第9讲：局域网交换与vlan_页面_61.jpg)

骨干链路不属于特定的VLAN：充当交换机和路由器之间VLAN的通道。

可以将骨干链路配置为传输所有VLAN或有限数量的VLAN。

但是，骨干链路可能具有本地VLAN。

如果骨干线链路由于任何原因失败，则骨干线的本地VLAN是该骨干线使用的VLAN。

#### 配置vlan

![第9讲：局域网交换与vlan_页面_62](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第9讲：局域网交换与vlan_页面_62.jpg)

在Cisco 29xx交换机上配置VLAN时，必须遵循以下准则：

1. VLAN的最大数量取决于交换机本身。
2. VLAN 1是出厂默认VLAN之一。(native VLAN往往是VLAN1，以及广播也是)
3. VLAN 1是默认的以太网VLAN。
4. 思科发现协议(CDP)和VLAN骨干协议(VTP)通告在VLAN 1上发送。
5. 默认情况下，Catalyst 29xx IP地址在VLAN 1广播域中。

![第9讲：局域网交换与vlan_页面_63](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第9讲：局域网交换与vlan_页面_63.jpg)



![第9讲：局域网交换与vlan_页面_64](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第9讲：局域网交换与vlan_页面_64.jpg)

![第9讲：局域网交换与vlan_页面_65](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第9讲：局域网交换与vlan_页面_65.jpg)

![第9讲：局域网交换与vlan_页面_66](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第9讲：局域网交换与vlan_页面_66.jpg)

### vlan间中的路由

![第9讲：局域网交换与vlan_页面_67](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第9讲：局域网交换与vlan_页面_67.jpg)

![第9讲：局域网交换与vlan_页面_68](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第9讲：局域网交换与vlan_页面_68.jpg)

1. 每个端口连接一个VLAN，每个IP和一个VLAN连接
2. 如下图，我们使用串口线，物理上是一个一个接口，划分成多个IP和子接口

![image-20230109125803577](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/image-20230109125803577.png)

![第9讲：局域网交换与vlan_页面_69](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第9讲：局域网交换与vlan_页面_69.jpg)

![第9讲：局域网交换与vlan_页面_70](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第9讲：局域网交换与vlan_页面_70.jpg)

![第9讲：局域网交换与vlan_页面_71](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第9讲：局域网交换与vlan_页面_71.jpg)

![第9讲：局域网交换与vlan_页面_72](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第9讲：局域网交换与vlan_页面_72.jpg)

