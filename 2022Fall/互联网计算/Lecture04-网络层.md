# 1. Lecture04-网络层

## 1.1. 网络层概述

### 1.1.1. 网络层职责

![第4讲：网络层原理与技术20200416_页面_003](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第4讲：网络层原理与技术20200416_页面_003.jpg)

1. 通过网络移动数据：不同网段之间的通信，不同的广播域，两个广播域之间的进行了划分，互不干扰
2. 使用**分层寻址**方案(与MAC寻址相反，后者没有层次)
3. 细分网络并控制流量
4. 减少交通拥堵，基于IP做分段和传达，用来减少拥塞
5. 与其他网络通讯

### 1.1.2. 网络层设备

![第4讲：网络层原理与技术20200416_页面_004](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第4讲：网络层原理与技术20200416_页面_004.jpg)

1. 路由器
   1. 互连网段或网络(不同网段的分割)
   2. 根据IP地址做出合理的决定
   3. 确定最佳路径，根据路由表。
   4. 将数据包从入站端口切换到出站端口
2. 如果A网段的设备向路由器发送了一个B网段的广播地址，那么路由器会进行转发，然而如果A网段设备发送的是本网段的广播地址，路由器则不会进行转发。(广播域划分)

## 1.2. IP地址和子网划分

### 1.2.1. 第三层数据报格式

![第4讲：网络层原理与技术20200416_页面_006](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第4讲：网络层原理与技术20200416_页面_006.jpg)

### 1.2.2. 报文详解

#### 1.2.2.1. 首部

![第4讲：网络层原理与技术20200416_页面_007](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第4讲：网络层原理与技术20200416_页面_007.jpg)

#### 1.2.2.2. 首部部分

上面蓝框部分的整体是首部部分，包括固定部分和可变部分

#### 1.2.2.3. 版本号

占 4 bit，指IP协议的版本。

目前的 IP 协议版本号为 4 (即 IPv4)(6也就对应IPv6)

#### 1.2.2.4. 首部长度

占 4 bit，可表示的最大数值是15个单位(一个单位为 4 字节) 因此IP的首部长度的最大值是60字节。

一行是5个字节，固定部分有20个字节，可变部分最多有40个字节。

#### 1.2.2.5. 服务类型

占8bit，用来获得更好的服务，这个字段以前一直没有被人们使用。

#### 1.2.2.6. 总长度

占 16 bit，指**首部和数据**之和的长度，单位为字节，因此数据报的最大长度为 65535 字节(由于放到帧里面，所以大多数不比1500字节长)。总长度必须不超过最大传送单元 MTU。

#### 1.2.2.7. 标识

标识(identification)：占 16 bit，它是一个计数器，用来产生数据报的标识。

解决**报文分片**的问题。相同的标识可以合并成一个大报文。

#### 1.2.2.8. 标志

标志占 3 bit，最高位为 0

1. 让发送方对报文进行控制，让中间路由器对其进行控制
2. DF(Don’t fragment)：是否允许做分片，0允许做分片,1不允许做分片
3. MF(More Fragment)：MF为0表示最后一个分片,1是指后面还有分片

#### 1.2.2.9. 片偏移

片偏移(13 bit)指出：较长的分组在分片后某片在原分组中的相对位置。片偏移以**8个字节**为偏移单位。

1. 相同标识号，然后根据片偏移进行重排
2. 因为16-3 = 13，2^3 = 8(因为单位是字节，所以用13位就可以补齐)

![image-20230102115026718](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/image-20230102115026718.png)

![第4讲：网络层原理与技术20200416_页面_017](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第4讲：网络层原理与技术20200416_页面_017.jpg)

#### 1.2.2.10. 生存时间

生存时间(8 bit)记为 TTL (Time To Live) 数据报在网络中可通过的**路由器数**的最大值。

是通过**计数**的方式来进行统计，最大值是**255**(最多经过255个路由器)，路由器每转发一次，就会对生存时间-1，减小为0后，就会丢弃掉，并且通知给发送方我已经丢弃掉这个报文。

防止在环上进行传输，避免由于回路问题，造成过大的网络资源浪费

#### 1.2.2.11. 协议

协议(8 bit)字段指出此数据报携带的数据使用何种协议以便目的主机的IP层将数据部分上交给哪个处理过程

![第4讲：网络层原理与技术20200416_页面_020](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第4讲：网络层原理与技术20200416_页面_020.jpg)

有的协议是上层的，有的协议是第三层协议，具体协议的情况如上

#### 1.2.2.12. 首部检验和

![第4讲：网络层原理与技术20200416_页面_022](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第4讲：网络层原理与技术20200416_页面_022.jpg)

首部检验和(16 bit)字段:只检验数据报的首部，不包括数据部分。这里不采用 CRC 检验码而采用简单的计算方法。

一般不用，一是只检验首部，不检验数据；二是消耗性能

#### 1.2.2.13. 源地址和目的地址

各占4个字节

### 1.2.3. 网络层地址

![第4讲：网络层原理与技术20200416_页面_024](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第4讲：网络层原理与技术20200416_页面_024.jpg)

1. IP地址为32位长(Ipv4中)

2. 它们以点分十进制格式表示为四个八位字节：133.14.17.0

3. IP地址包含两个组成部分：

   - 网络ID

   - 主机ID

![第4讲：网络层原理与技术20200416_页面_025](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第4讲：网络层原理与技术20200416_页面_025.jpg)

#### 1.2.3.1. 网络地址：用来标识网段

一个网络中，共享一个网络地址

1. 原来由ARIN(美国互联网号码注册机构，[www.arin.net](http://www.arin.net))分配，现在已经更换
2. 标识设备所连接(attached)的网络
3. 可以由前三个八位位组(octets)中的一个，两个或三个来标识

#### 1.2.3.2. 主机ID：IP地址后面占据1-3个字节

1. 由网络管理员分配
2. 识别该网络上的特定设备
3. 可以由最后三个八位位组中的一个，两个或三个来标识

### 1.2.4. IP地址

![第4讲：网络层原理与技术20200416_页面_026](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第4讲：网络层原理与技术20200416_页面_026.jpg)

不同的类地址为地址的网络部分和主机部分保留不同数量的位

#### 1.2.4.1. 分类

![第4讲：网络层原理与技术20200416_页面_027](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第4讲：网络层原理与技术20200416_页面_027.jpg)

**A类第一位必为0，B开头为10，C为110**

根据第一个地址的数值确定是哪类地址



![第4讲：网络层原理与技术20200416_页面_028](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第4讲：网络层原理与技术20200416_页面_028.jpg)

1. 每个类别的最大主机数量各不相同。(不包含网络号)

   - A类拥有16,777,214个可用主机(2^24^ – 2)

   - Class B has 65,534 available hosts (2^16^ – 2) B类具有65,534个可用主机(2^16^ – 2)

   - Class C has 254 available hosts (2^8^ – 2) C类具有254个可用主机(2^8^ –2)

2. 为什么每一类地址中都要减去2？

   - 每个网络中的第一个地址都保留用于该网络地址

   - 最后一个地址是为广播地址保留的。

#### 1.2.4.2. 保留地址

![第4讲：网络层原理与技术20200416_页面_029](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第4讲：网络层原理与技术20200416_页面_029.jpg)

网络地址:在地址的**主机部分**中以二进制0结尾的IP地
1. A类网络地址示例：113.0.0.0
2. 网络上的主机只有具有相同网络ID的其他主机才能直接通信。(用来确定是不是在一个网段里面)



![第4讲：网络层原理与技术20200416_页面_030](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第4讲：网络层原理与技术20200416_页面_030.jpg)

广播地址:用于将数据发送到网络上的所有设备。(一般是一个网段之间的)

1. 广播IP地址在地址的**主机部分**中以二进制1结尾。
2. B类地址的广播地址的示例:176.10.255.255 (decimal 255 = binary 11111111)



![第4讲：网络层原理与技术20200416_页面_031](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第4讲：网络层原理与技术20200416_页面_031.jpg)

例子

![第4讲：网络层原理与技术20200416_页面_032](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第4讲：网络层原理与技术20200416_页面_032.jpg)

用于局域网内部使用

IP地址耗尽（IP address delpetion）

![image-20230102123731364](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/image-20230102123731364.png)

### 1.2.5. 子网划分

![第4讲：网络层原理与技术20200416_页面_033](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第4讲：网络层原理与技术20200416_页面_033.jpg)

网络管理员有时需要将网络划分为较小的网络，称为**子网**，以提供**额外的灵活性**.

从主机字段借来的位被指定为子网字段(Subnet Fields)

#### 1.2.5.1. 子网的基本概念

![第4讲：网络层原理与技术20200416_页面_034](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第4讲：网络层原理与技术20200416_页面_034.jpg)

1. 子网是网络的较小部分
   - 提供寻址灵活性
2. 子网地址通常由网络管理员在本地分配
3. 子网减少了广播域：使得广播域变小，提高网络利用率，避免接受到大量的无用的广播，广播只能在对应子网中进行广播。

#### 1.2.5.2. 我们可以借多少位？

![第4讲：网络层原理与技术20200416_页面_035](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第4讲：网络层原理与技术20200416_页面_035.jpg)

**最少借2位** 


![第4讲：网络层原理与技术20200416_页面_036](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第4讲：网络层原理与技术20200416_页面_036.jpg)

1. 借用的最小位数是2，为什么？

   I如果只借用1位以创建一个子网，那么您将只有一个网络号-.0网络-和广播号-.1网络，没有可以使用的专用网络。

   两位的时候，01和10给Host，00给网络ID，11位广播地址

2. 可以借用的最大位数可以是保留至少2位主机号的任何数字(给Host至少保留2位，因为1位的话，要么一个是NET无法使用，要么一个是广播地址)



#### 1.2.5.3. 副作用：浪费地址

![第4讲：网络层原理与技术20200416_页面_037](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第4讲：网络层原理与技术20200416_页面_037.jpg)

我们必须在所需的子网数，每个子网可接受的主机以及地址的浪费之间取得平衡(strike a balance)。b

#### 1.2.5.4. 子网掩码

![第4讲：网络层原理与技术20200416_页面_038](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第4讲：网络层原理与技术20200416_页面_038.jpg)

别名:扩展网络前缀

定义我们用来构建网络的位数，以及描述主机地址的位数

#### 1.2.5.5. 计算一个子网

![第4讲：网络层原理与技术20200416_页面_039](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第4讲：网络层原理与技术20200416_页面_039.jpg)

![第4讲：网络层原理与技术20200416_页面_040](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第4讲：网络层原理与技术20200416_页面_040.jpg)

![第4讲：网络层原理与技术20200416_页面_041](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第4讲：网络层原理与技术20200416_页面_041.jpg)

![第4讲：网络层原理与技术20200416_页面_042](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第4讲：网络层原理与技术20200416_页面_042.jpg)

![第4讲：网络层原理与技术20200416_页面_043](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第4讲：网络层原理与技术20200416_页面_043.jpg)![第4讲：网络层原理与技术20200416_页面_044](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第4讲：网络层原理与技术20200416_页面_044.jpg)

![第4讲：网络层原理与技术20200416_页面_045](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第4讲：网络层原理与技术20200416_页面_045.jpg)

![第4讲：网络层原理与技术20200416_页面_046](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第4讲：网络层原理与技术20200416_页面_046.jpg)

![第4讲：网络层原理与技术20200416_页面_047](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第4讲：网络层原理与技术20200416_页面_047.jpg)

![第4讲：网络层原理与技术20200416_页面_048](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第4讲：网络层原理与技术20200416_页面_048.jpg)

![第4讲：网络层原理与技术20200416_页面_049](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第4讲：网络层原理与技术20200416_页面_049.jpg)

![第4讲：网络层原理与技术20200416_页面_050](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第4讲：网络层原理与技术20200416_页面_050.jpg)

![第4讲：网络层原理与技术20200416_页面_051](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第4讲：网络层原理与技术20200416_页面_051.jpg)



![第4讲：网络层原理与技术20200416_页面_052](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第4讲：网络层原理与技术20200416_页面_052.jpg)

路由器需要做一个与运算，交换机不用



![第4讲：网络层原理与技术20200416_页面_053](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第4讲：网络层原理与技术20200416_页面_053.jpg)

![第4讲：网络层原理与技术20200416_页面_054](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第4讲：网络层原理与技术20200416_页面_054.jpg)

## 1.3. 第三层设备——路由器

### 1.3.1. 路径选择

![第4讲：网络层原理与技术20200416_页面_056](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第4讲：网络层原理与技术20200416_页面_056.jpg)

路由器选择下一路径，根据带宽、跳数、延迟等

### 1.3.2. IP地址

![第4讲：网络层原理与技术20200416_页面_057](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第4讲：网络层原理与技术20200416_页面_057.jpg)

1. IP地址是用**软件实现**的，是指设备所在的网络。
2. 路由器连接网络，每个网络必须具有**唯一的网络号**才能成功进行寻找路径。
3. 唯一的网络号包含在分配(incorporated)给该网络上每个设备的IP地址中
4. IP地址是**逻辑**的，是我们配置的。(不同于MAC地址)
5. IP地址是有层次，做**转发的依据是网段**而不是具体的IP，同一网段设备都有相同的IP地址，也就是我们只要到达网段即可

![第4讲：网络层原理与技术20200416_页面_058](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第4讲：网络层原理与技术20200416_页面_058.jpg)

路由器每个端口需要配一个地址，和所连接的网段是同一个信息的

### 1.3.3. 路由器转发实例

![第4讲：网络层原理与技术20200416_页面_059](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第4讲：网络层原理与技术20200416_页面_059.jpg)

A5发到B5

![第4讲：网络层原理与技术20200416_页面_060](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第4讲：网络层原理与技术20200416_页面_060.jpg)

![第4讲：网络层原理与技术20200416_页面_061](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第4讲：网络层原理与技术20200416_页面_061.jpg)

![第4讲：网络层原理与技术20200416_页面_062](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第4讲：网络层原理与技术20200416_页面_062.jpg)

查询路由表

![第4讲：网络层原理与技术20200416_页面_063](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第4讲：网络层原理与技术20200416_页面_063.jpg)

形成一个新的帧，MAC地址是B1的

![第4讲：网络层原理与技术20200416_页面_064](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第4讲：网络层原理与技术20200416_页面_064.jpg)

1. 接口是路由器连接到网络的附加装置，在IP路由中也可以称为端口。这个IP地址往往被作为这个网络的网关

2. 每个接口必须具有一个单独的唯一网络地址。

   比如上图中S1和S2不能是相同的IP地址，否则会发生歧义，S0不知道转发给谁，路由器的连接的网段一定要是不同的

### 1.3.4. IP地址分配

![第4讲：网络层原理与技术20200416_页面_065](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第4讲：网络层原理与技术20200416_页面_065.jpg)

1. 静态地址分配(Static addressing)

   1. 为每个单独的设备配置一个IP地址
   2. 您应该保留非常细致的记录，因为如果使用重复的IP地址，可能会出现问题。

2. 动态地址分配(Dynamic addressing)

   有几种不同的方法可用于动态分配IP地址：

   - RARP: Reverse Address Resolution Protocol. RARP：反向地址解析协议。发起请求
   - BOOTP: BOOTstrap Protocol. BOOTP：BOOTstrap协议。用于工作栈
   - DHCP: Dynamic Host Configuration Protocol. (**比较多用**) DHCP：动态主机配置协议



## 1.4. ARP 协议

Address Resolution Protocol 地址解析协议

![第4讲：网络层原理与技术20200416_页面_067](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第4讲：网络层原理与技术20200416_页面_067.jpg)

1. 为了使设备进行通信，发送设备需要目标设备的**IP地址和MAC地址**。
2. ARP使计算机能够查找与IP地址关联的计算机的MAC地址。

![第4讲：网络层原理与技术20200416_页面_068](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第4讲：网络层原理与技术20200416_页面_068.jpg)

1. 目的方IP地址 -> 目的方MAC地址
2. 需要知道对方的MAC地址，来形成数据地址。

![第4讲：网络层原理与技术20200416_页面_069](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第4讲：网络层原理与技术20200416_页面_069.jpg)

ARP Table

ARP缓存

### 1.4.1. ARP操作，MAC地址解析

![第4讲：网络层原理与技术20200416_页面_070](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第4讲：网络层原理与技术20200416_页面_070.jpg)

此时目的地MAC地址不知道

![第4讲：网络层原理与技术20200416_页面_071](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第4讲：网络层原理与技术20200416_页面_071.jpg)

向目的地地址请求，发出**广播**

![第4讲：网络层原理与技术20200416_页面_072](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第4讲：网络层原理与技术20200416_页面_072.jpg)

![第4讲：网络层原理与技术20200416_页面_073](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第4讲：网络层原理与技术20200416_页面_073.jpg)

C回应

![第4讲：网络层原理与技术20200416_页面_074](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第4讲：网络层原理与技术20200416_页面_074.jpg)

写入ARP缓存，发送正常的帧

### 1.4.2. 目的地本地

![第4讲：网络层原理与技术20200416_页面_075](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第4讲：网络层原理与技术20200416_页面_075.jpg)

### 1.4.3. 网络交流

![第4讲：网络层原理与技术20200416_页面_076](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第4讲：网络层原理与技术20200416_页面_076.jpg)



#### 1.4.3.1. 默认网关

![第4讲：网络层原理与技术20200416_页面_077](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第4讲：网络层原理与技术20200416_页面_077.jpg)

1. 为了使设备与另一网络上的另一设备通信，您必须为其**提供默认网关**。
2. **默认网关是路由器上连接到源主机所在网段的接口的IP地址。**
3. 为了使设备将数据发送到另一个网段上的设备的地址，源设备将数据**发送到默认网关**。

#### 1.4.3.2. 代理

![第4讲：网络层原理与技术20200416_页面_078](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第4讲：网络层原理与技术20200416_页面_078.jpg)

**无法设置默认网关的情况**

1. 代理ARP是ARP的一种变体(variation)。
2. 如果源主机未配置默认网关。

### 1.4.4. 目的地址不是本地

![第4讲：网络层原理与技术20200416_页面_079](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第4讲：网络层原理与技术20200416_页面_079.jpg)

路由器把自己的MAC地址给Host Y

### 1.4.5. ARP流程图

![第4讲：网络层原理与技术20200416_页面_080](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第4讲：网络层原理与技术20200416_页面_080.jpg)



## 1.5. 网络层服务

### 1.5.1. 面向连接的网络服务

![第4讲：网络层原理与技术20200416_页面_082](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第4讲：网络层原理与技术20200416_页面_082.jpg)

1. 面向连接的网络服务——在数据传输之前，在传输方和接收方之间建立连接

   - 就是任何发送数据的行为之前，先要建立好连接，协商好参数才会开始传输，所有数据进行有序传输

   - 网络情况导致数据出现问题，需要接受方进行一定处理来保证数据正确

2. 传输过程中要保持连接距离，只有完成传输后才能断开连接。

3. 传输比较可靠，代价高。

![第4讲：网络层原理与技术20200416_页面_083](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第4讲：网络层原理与技术20200416_页面_083.jpg)

1. （虚）电路交换vs面向连接的网络服务

   但是，两个名词并不一样。

2. **面向连接**：在数据传输之前，与接收方建立一个连接

3. 所有packet（报文）在同一条道路上依次传输，更普遍的，是在同一条虚电路上

**虚电路**要强于面向连接的，传输更加可靠，保证**传输先后关系**。

### 1.5.2. 无连接的网络服务

![第4讲：网络层原理与技术20200416_页面_084](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第4讲：网络层原理与技术20200416_页面_084.jpg)

他们分别对待每个数据包。

IP是**无连接系统**。

### 1.5.3. 报文交换

![第4讲：网络层原理与技术20200416_页面_085](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第4讲：网络层原理与技术20200416_页面_085.jpg)

1. 无连接网络与数据包交换:这两个词都不一样
2. 当数据包从源传递到目标时，它们可以：
   1. 切换到其他路径。(每一报文有各自的发送方和接收方，可以根据当前的网络情况，进行路由选择)
   2. 乱序到达。
3. 设备根据**各种标准**为每个数据包**进行路径选择**。不同的报文可能有不同的标准。

大部分的Connetionless network都是基于packet switched进行实现，控制网络拥塞。



## 1.6. 路由协议

### 1.6.1. 网络协议操作

![第4讲：网络层原理与技术20200416_页面_087](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第4讲：网络层原理与技术20200416_页面_087.jpg)

ABC之间都是通过帧进行计算的，直到第三层。

### 1.6.2. 被动路由协议

![第4讲：网络层原理与技术20200416_页面_088](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第4讲：网络层原理与技术20200416_页面_088.jpg)

1. 为网络层提供支持的协议称为路由协议或可路由协议。
2. IP是网络层协议，因此，它可以通过互联网络进行路由。

### 1.6.3. 不可路由协议

![第4讲：网络层原理与技术20200416_页面_089](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第4讲：网络层原理与技术20200416_页面_089.jpg)

1. 不可路由协议是**不支持第3层**的协议。

2. 这些不可路由协议中最常见的是NetBEUI。

   - 直接根据目的方的地址在局域网中进行生成定位

   - 这个协议不支持第三层，也就是跨局域网是不可以的。

3. NetBEUI是一种小型，快速且高效的协议，仅限于在一个网段上运行

### 1.6.4. 可路由协议的寻址

![第4讲：网络层原理与技术20200416_页面_090](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第4讲：网络层原理与技术20200416_页面_090.jpg)



![第4讲：网络层原理与技术20200416_页面_091](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第4讲：网络层原理与技术20200416_页面_091.jpg)

### 1.6.5. 分类1：静态vs动态

![第4讲：网络层原理与技术20200416_页面_092](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第4讲：网络层原理与技术20200416_页面_092.jpg)

1. 静态路由：网络管理员在路由器中手动输入路由信息。

2. 动态路由

   - 路由器可以在运行过程中互相学习信息。

   - 使用路由协议更新路由信息。

   - RIP, IGRP, EIGRP, OSPF …

![第4讲：网络层原理与技术20200416_页面_093](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第4讲：网络层原理与技术20200416_页面_093.jpg)

1. 静态路由

   1. 用于**隐藏**部分网络。

      安全(不必进行路由表的交换)

   2. 测试网络中的特定链接。

   3. 在到达目标网络的路径只有一条通路时，维护路由表。

2. 动态路由

   1. 维护路由表。
   2. 以路由更新的形式及时分发信息。
   3. 依靠路由协议共享知识。
   4. 路由器可以调整以适应不断变化的网络状况。
   5. 打开后会启动**进程**，按照不同的协议，和网上的不同设备学习信息，然后根据**算法**生成路由表

### 1.6.6. 主动路由协议

![第4讲：网络层原理与技术20200416_页面_094](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第4讲：网络层原理与技术20200416_页面_094.jpg)

### 1.6.7. 被动路由协议和主动路由协议

![第4讲：网络层原理与技术20200416_页面_095](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第4讲：网络层原理与技术20200416_页面_095.jpg)

1. Routed Portocol用于路由器之间，用来保证路由器之间连通(完成转发)。
2. Routing Protocol用于做各自的路由表的生成：路由器彼此交换信息。
3. Routing Protocol 决定 Routed Protocals

### 1.6.8. 分类2：IGP VS EGP

![第4讲：网络层原理与技术20200416_页面_096](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第4讲：网络层原理与技术20200416_页面_096.jpg)

1. 动态路由

2. 内部网关协议Interior Gateway  Protocols

   RIP，IGRP，EIGRP，OSPF

   可在自治系统(autonomous  system，大的单位或者管理方)中使用，该系统是一个主管部门下的路由器网络，例如公司(corporate)网络，学区的网络或政府机构的网络。

2. 外部网关协议Exterior Gateway Protocols

   EGP，BGP

   用于在自治系统之间路由数据包。

![第4讲：网络层原理与技术20200416_页面_097](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第4讲：网络层原理与技术20200416_页面_097.jpg)

自治系统是**逻辑**的划分,而未必是物理层次的划分。

### 1.6.9. 分类3：IGP分为两类：DVP VS LSP

![第4讲：网络层原理与技术20200416_页面_098](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第4讲：网络层原理与技术20200416_页面_098.jpg)

**DVP**

距离矢量协议Distance-Vector Protocols

RIP, IGRP

1. 从**邻居**的角度查看网络拓扑。(注意不基于全局)
2. 在路由器之间添加距离向量。(根据跳数来决定，经过一个路由器+1一次)
3. 经常定期(periodic)更新。（**定时**）
4. 将路由表的**副本**传递到邻居路由器。

**LSP**

链路状态协议Link State Protocols

 OSPF

1. 获取**全局**网络拓扑的通用视图。
2. 计算到其他路由器的**最短路径**。(基于带宽计算出来的cost，形成cost拓扑图，然后计算出对应的路径代价作为评判依据)
3. **事件**触发的更新。
4. 将链接状态路由更新传递给其他路由器

![第4讲：网络层原理与技术20200416_页面_099](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第4讲：网络层原理与技术20200416_页面_099.jpg)



![第4讲：网络层原理与技术20200416_页面_100](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第4讲：网络层原理与技术20200416_页面_100.jpg)

Link State

用SPF算法

### 1.6.10. RIP路由信息协议（DVP）

![第4讲：网络层原理与技术20200416_页面_101](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第4讲：网络层原理与技术20200416_页面_101.jpg)

1. 很受欢迎
2. 内部网关协议
3. 距离矢量协议
4. 基于跳数
5. 最远可达跳数15
6. 每30秒更新
7. 不选择最快路径（选择**跳数最短**的路径）
8. 产生很多网络流量（network traffic)
9. v2是v1的一个进阶版本

### 1.6.11. IGRP vs EIGRP(DVP)

![第4讲：网络层原理与技术20200416_页面_102](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第4讲：网络层原理与技术20200416_页面_102.jpg)

1. 思科知识产权的。
2. 内部网关协议。
3. 距离矢量协议
4. 指标由**带宽(bandwidth)，负载(load)，延迟(delay)和可靠性(reliability)** 组成。加权进行运算。
5. IGRP最大跳数为255。
6. 每90秒更新一次。
7. EIGRP是IGRP的高级版本，它是**混合**路由协议(不全是根据跳数来计算)。

比RIP性能好很多

### 1.6.12. OSPF（LSP）

![第4讲：网络层原理与技术20200416_页面_103](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第4讲：网络层原理与技术20200416_页面_103.jpg)

1. 最短路径优先协议
2. **内部网关**协议
3. 链路状态协议，消耗内存和CPU
4. 指标由带宽，速度，流量，可靠性和安全性组成，本科阶段只考虑带宽的。
5. 事件触发的更新。
6. 最快和什么有关？(最快指的是带宽)
   1. 和实时各条链路上的通信冗余有关，也和管理方案有关，简单来说是和带宽有关
   2. 带宽表示为代价，带宽和代价成**反比**。



## 1.7. VLSM(Variable Length Subnet Mask) 可变长度子网掩码

### 1.7.1. 经典路由和可变长度子网掩码

![66_VoIPSCreencastCoverWnd (2).png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/imgae/2023/02/03/badddf683b59945390f1996bc7504838_66_VoIPSCreencastCoverWnd%20-2-.png)


- 有类路由

  有类的路由协议要求单个网络使用相同的子网掩码。

  例如：网络192.168.187.0必须仅使用一个子网掩码，例如255.255.255.0。

- 可变长度子网掩码

  VLSM只是一个特征，它允许单个自治系统的网络具有不同的子网掩码。

  可有效解决网络号浪费的问题

![67_VoIPSCreencastCoverWnd.png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/imgae/2023/02/03/5df9642f79a004b3c63232cc6179cd66_67_VoIPSCreencastCoverWnd.png)


1. 使用VLSM，网络管理员可以在主机少的网络上使用长掩码，而在主机多的子网上使用短掩码。(提供了很高的灵活性)
2. 如果路由协议允许VLSM
   1. 在路由网络连接上使用30位子网掩码255.255.255.252（两个路由器相连）
   2. 用户网络的24位掩码255.255.255.0
   3. 或者，对于最多1000个用户的网络，甚至是22位掩码255.255.252.0。(保留10位)
3. 在CIDR的基础上发展的，报文中包含有子网掩码。

### 1.7.2. 为什么使用VLSM

![第4讲：网络层原理与技术20200416_页面_107](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第4讲：网络层原理与技术20200416_页面_107.jpg)

1. VLSM允许组织在同一网络地址空间内使用多个子网掩码。
2. 实施VLSM通常被称为"子网划分"，可用于最大化寻址效率。
3. VLSM是有助于缩小IPv4和IPv6之间差距的修改(modifications)之一。

### 1.7.3. VLSM的优缺点

![第4讲：网络层原理与技术20200416_页面_108](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第4讲：网络层原理与技术20200416_页面_108.jpg)

1. 高效使用IP地址
2. 更好的路由聚合(aggregation):构建超网

很多协议都支持VLSM协议，只有RIP v1不支持

![第4讲：网络层原理与技术20200416_页面_109](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第4讲：网络层原理与技术20200416_页面_109.jpg)

会导致地址空间的浪费:广播地址和网络号都无法被使用。

1. 过去，建议不要使用第一个和最后一个子网。但是我们可以使用Cisco IOS ver12.0中的子网0。
2. 从IOS ver12.0起，Cisco路由器默认使用零子网。
3. 如果想要禁止零子网，使用该指令:`router(config)#no ip subnet-zero()`

![第4讲：网络层原理与技术20200416_页面_110](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第4讲：网络层原理与技术20200416_页面_110.jpg)



![第4讲：网络层原理与技术20200416_页面_111](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第4讲：网络层原理与技术20200416_页面_111.jpg)

路由器之间不需要那么多地址，可以进行优化

### 1.7.4. 例子

![第4讲：网络层原理与技术20200416_页面_112](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第4讲：网络层原理与技术20200416_页面_112.jpg)



![第4讲：网络层原理与技术20200416_页面_113](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第4讲：网络层原理与技术20200416_页面_113.jpg)

为了计算VLSM子网，各个主机首先从地址范围分配最大的需求。需求级别应从**最大到最小**列出。

![第4讲：网络层原理与技术20200416_页面_114](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第4讲：网络层原理与技术20200416_页面_114.jpg)

Octet八位字节

![第4讲：网络层原理与技术20200416_页面_115](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第4讲：网络层原理与技术20200416_页面_115.jpg)



![第4讲：网络层原理与技术20200416_页面_116](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第4讲：网络层原理与技术20200416_页面_116.jpg)



![第4讲：网络层原理与技术20200416_页面_117](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第4讲：网络层原理与技术20200416_页面_117.jpg)



![第4讲：网络层原理与技术20200416_页面_118](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第4讲：网络层原理与技术20200416_页面_118.jpg)



![第4讲：网络层原理与技术20200416_页面_119](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第4讲：网络层原理与技术20200416_页面_119.jpg)



![第4讲：网络层原理与技术20200416_页面_120](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第4讲：网络层原理与技术20200416_页面_120.jpg)



![第4讲：网络层原理与技术20200416_页面_121](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第4讲：网络层原理与技术20200416_页面_121.jpg)



![第4讲：网络层原理与技术20200416_页面_122](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第4讲：网络层原理与技术20200416_页面_122.jpg)



![第4讲：网络层原理与技术20200416_页面_123](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第4讲：网络层原理与技术20200416_页面_123.jpg)

**没被用过的子网才能进一步划分**

### 1.7.5. 路由聚集

![第4讲：网络层原理与技术20200416_页面_124](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第4讲：网络层原理与技术20200416_页面_124.jpg)

Classless InterDomain Routing(CIDR) 无类域间路由

将3个/24的子合并成一个/16的网络

![第4讲：网络层原理与技术20200416_页面_125](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第4讲：网络层原理与技术20200416_页面_125.jpg)

多层聚集

#### 1.7.5.1. 如何计算路由聚集

![第4讲：网络层原理与技术20200416_页面_126](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第4讲：网络层原理与技术20200416_页面_126.jpg)

提取尽可能多的相同的位作为net位，其他作为host位

![第4讲：网络层原理与技术20200416_页面_127](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第4讲：网络层原理与技术20200416_页面_127.jpg)

- 减少路由表条目的数量。

- 可用于隔离拓扑更改

## 1.8. ICMP

![第4讲：网络层原理与技术20200416_页面_129](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第4讲：网络层原理与技术20200416_页面_129.jpg)

Internet Control Message Protocol 因特网控制报文协议

### 1.8.1. ICMP报文格式

![第4讲：网络层原理与技术20200416_页面_130](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第4讲：网络层原理与技术20200416_页面_130.jpg)

### 1.8.2. 两种ICMP报文

![第4讲：网络层原理与技术20200416_页面_131](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第4讲：网络层原理与技术20200416_页面_131.jpg)



![第4讲：网络层原理与技术20200416_页面_132](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第4讲：网络层原理与技术20200416_页面_132.jpg)

### 1.8.3. ICMP差错报告报文的数据字段的内容

![第4讲：网络层原理与技术20200416_页面_133](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第4讲：网络层原理与技术20200416_页面_133.jpg)

### 1.8.4. 不应发送ICMP差错报告报文的几种情况


![第4讲：网络层原理与技术20200416_页面_134](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第4讲：网络层原理与技术20200416_页面_134.jpg)

### 1.8.5. PING

![第4讲：网络层原理与技术20200416_页面_135](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第4讲：网络层原理与技术20200416_页面_135.jpg)

PING Packet InterNet Groper



## 1.9. 一些其他

基于IP地址，而不是MAC地址。

【IP地址和MAC地址的区别】IP地址是一个框架，有逻辑和层次。MAC地址较为平坦（谁生产的商品）

G 0/0/0 模块 板子 接口

s 0/1/0 模块 板子 接口



802.3

The maximum size of the L-PDU for a 10Mbps network is 1500 bytes. Because 8 bytes are used within the L-PDU for the LLC header, this means that the maximum size of the data field is **1492 bytes**.

802.11

The frame body of the 802.11 packet can range from 0-**2312 bytes**.





Routed protocol

被动路由协议

基于路由表



Non-routable protobal 

不基于路由表



多播地址

D类地址，开头为1110

多播地址范围为224.0.0.0～239.255.255.255
