# Lecture10-WAN广域网

![第10讲：广域网(wan)_页面_01](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第10讲：广域网(wan)_页面_01.jpg)

## 广域网技术和设备

![第10讲：广域网(wan)_页面_02](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第10讲：广域网(wan)_页面_02.jpg)

### 广域网服务

![第10讲：广域网(wan)_页面_03](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第10讲：广域网(wan)_页面_03.jpg)

定义：WAN是通过WAN服务提供商连接LAN的通信网络

WAN在OSI的前三层运行，但**主要集中在物理和数据链路层**。

广域网和局域网相比相对低效

### 公司的发展

![第10讲：广域网(wan)_页面_04](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第10讲：广域网(wan)_页面_04.jpg)

- 广域网的最小单位是公司
- 随着公司逐渐的发展才发展(公司的发展是需求)
- 最上角:公司刚成立的时候，小的局域网就可以搞定了(几台主机)，对外提供服务少，局域网协同办公。
- 右上角:随着公司的发展，一家发展到几十家，需要将不同的项目分开，每一个项目都有对应的项目经理和开发人员，多个局域网组成一个AS(自治系统)。还是一个出口，ASP要求高，VLAN隔离和防火墙
- 左下角:再次发展，有多个分支机构，区域办事处等，物理上隔离的很远，这时候建立一个数据中心(存放全部业务数据)，保证团队可以在任何位置访问，公司向ISP请求租用一个广域网链路。
- 右下角:最后进一步发展，覆盖全球:公司规模足够大，考虑成本，需要部署站点到站点之间的VPN，保证效率更高。

### 广域网物理结构

![第10讲：广域网(wan)_页面_05](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第10讲：广域网(wan)_页面_05.jpg)

通过NetWork远程接入，通过WSP提供的CO Swtich来连接到中心局

CPE:位于公司本地的设备(主要是接入设备)，可以向ISP购买或者租用，购买上网服务(猫)

### 广域网虚拟电路

![第10讲：广域网(wan)_页面_06](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第10讲：广域网(wan)_页面_06.jpg)

交换虚拟电路(SVC，Switched Virtual Circuits)是到目的地的WAN路径，可根据需要建立(established)和终止(terminated)

广域网虚拟电路的三个阶段(phases)

1. 电路建立–创建虚拟电路(逻辑确定)
2. 数据传输–发送和接收用户数据(含有虚电路号等)
3. 电路中断–拆除虚拟电路

永久(Permanent)虚拟电路(PVC)是采用以下一种模式的永久建立的电路：数据传输

1. X.25和帧中继使用PVC
2. 减少带宽使用，但增加成本

![image-20230110112813672](C:/Users/QUAS/AppData/Roaming/Typora/typora-user-images/image-20230110112813672.png)

### 链接类型和带宽

![image-20230110112856731](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/image-20230110112856731.png)

1. T：美国标准
2. E：欧洲标准



![第10讲：广域网(wan)_页面_07](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第10讲：广域网(wan)_页面_07.jpg)

### 交换电路连接

![第10讲：广域网(wan)_页面_08](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第10讲：广域网(wan)_页面_08.jpg)

ISDN:多个B信道和P信道组合

- BRI:2个B和一个D
- PRI:T1:23B + D 和 E1:30B + D

### 网络连接

[![img](https://spricoder.oss-cn-shanghai.aliyuncs.com/2020-Internet-computing/img/lec10/5.png)](https://spricoder.oss-cn-shanghai.aliyuncs.com/2020-Internet-computing/img/lec10/5.png)

- 直接连接到运营商，DSL接入(以太网转换成DSL信号)

[![img](https://spricoder.oss-cn-shanghai.aliyuncs.com/2020-Internet-computing/img/lec10/6.png)](https://spricoder.oss-cn-shanghai.aliyuncs.com/2020-Internet-computing/img/lec10/6.png)

- 永久在线连接，用于有线电视传输等，共享电缆开关等

[![img](https://spricoder.oss-cn-shanghai.aliyuncs.com/2020-Internet-computing/img/lec10/5.jpg)](https://spricoder.oss-cn-shanghai.aliyuncs.com/2020-Internet-computing/img/lec10/5.jpg)

- 无线
  - 地面无线信道
  - 无线信道

![第10讲：广域网(wan)_页面_09](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第10讲：广域网(wan)_页面_09.jpg)

### 广域网设备

![第10讲：广域网(wan)_页面_10](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第10讲：广域网(wan)_页面_10.jpg)

为了连接到专线(leased line)，客户必须具备以下条件：

1. 访问服务提供商的电路
2. 可用的适当路由器端口
3. CSU/DSU，调制解调器，ISDN终端适配器等。

#### 调制解调器

![第10讲：广域网(wan)_页面_11](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第10讲：广域网(wan)_页面_11.jpg)

1. 通道服务单元 CSU,Channel Service Units/数字服务单元 DSU,Digital Service Units
2. 与语音级(voice-grade)连接接口，以便将模拟信号转换为数字信号。

## 广域网和OSI模型

![第10讲：广域网(wan)_页面_12](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第10讲：广域网(wan)_页面_12.jpg)

### 广域网标准

![第10讲：广域网(wan)_页面_13](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第10讲：广域网(wan)_页面_13.jpg)

WAN标准主要描述OSI模型的哪些层？**物理层和数据链路层**，物理层提供电器标准，数据链路层封装到远程的部分:帧标准

### wan物理层

![第10讲：广域网(wan)_页面_14](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第10讲：广域网(wan)_页面_14.jpg)

1. 描述如何为WAN服务提供电气，机械，操作和功能连接的协议。
2. 这些服务通常是从WAN服务提供商，备用运营商，电话后和电报(PTT)机构获得的。
3. 描述数据终端设备(DTE, Data Terminal Equipment)和数据电路终端设备(DCE. Data Circuit-terminating Equipment)之间的接口。

![第10讲：广域网(wan)_页面_15](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第10讲：广域网(wan)_页面_15.jpg)

1. 通常，DCE是服务提供商，而DTE是连接的设备。
2. 在此模型中，通过调制解调器或 CSU / DSU 提供给DTE的服务。

![第10讲：广域网(wan)_页面_16](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第10讲：广域网(wan)_页面_16.jpg)

指定DTE和DCE之间此接口的几种物理层标准是…

1. EIA/TIA-232 (RS-232):计算机常用
2. EIA/TIA-449
3. V.24
4. V.35
5. X.21
6. G.703
7. EIA-530

### wan数据链路层

![第10讲：广域网(wan)_页面_17](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第10讲：广域网(wan)_页面_17.jpg)

1. WAN数据链路协议描述了如何在单个数据链路上的系统之间承载帧。
2. 它们包括旨在在专用(dedicated)点对点，多点和多址交换服务上运行的协议。
3. WAN 标准由许多公认的机构定义和管理，包括以下机构：ITU-T，ISO，IETF和EIA
4. 不是那么可靠，帧结构和以太网帧不同，协议是点对点，点对多点，多链路交换机切换
5. 为了确保正确:需要为每一个串口指定一个方式组成帧

![第10讲：广域网(wan)_页面_18](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第10讲：广域网(wan)_页面_18.jpg)

WAN数据链路层定义了如何封装数据以传输到远程站点

1. **点对点协议(PPP,Point-to-Point Protocol)**:由IETF开发。PPP包含用于识别网络层协议的协议字段(包含一个协议单元，指定网络协议)
2. **高级数据链路控制(HDLC, High-Level Data Link Control)**:ISO标准，不同供应商之间不兼容的HDLC，因为每个供应商都选择了实现方式。HDLC支持点对点/多点配置(抽象规范和约束，各个厂商不同)
3. **帧中继(Frame Relay)**：使用简化的封装，对高质量的数字设备不进行纠错。(比较高速)
4. **ISDN**：通过现有电话线传输语音和数据的一组数字服务。
5. **平衡的链路访问程序(LAPB, Link Access Procedure, Balanced)**：用于在X.25堆栈的第2层封装数据包的数据包交换网络。 提供点对点的可靠性和流量控制。

## 广域网访问方法

![第10讲：广域网(wan)_页面_19](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第10讲：广域网(wan)_页面_19.jpg)

### PPP/ HDLC PPP（重要考试考）

点对点的标准

以思科厂商为标准

工作在串行链路上的

如果都是同一个厂商的可以用HDLC，不然使用PPP

#### 串行线框字段

![第10讲：广域网(wan)_页面_20](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第10讲：广域网(wan)_页面_20.jpg)

1. 两种最常见的点对点WAN封装是HDLC(High-level Data Protocol)和PPP(Point to Poing Protocol)
2. 所有串行线封装共享一个通用的帧格式，该格式具有以下字段
3. 封装协议的选择取决于WAN技术和通信设备

#### PPP and HDLC

![第10讲：广域网(wan)_页面_21](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第10讲：广域网(wan)_页面_21.jpg)

1. PPP是一种标准的

   串行线路

   封装方法

   1. 由IETF(The Internet Engineering Task)开发;取代SLIP(Serial Line Internet Protocol)
   2. 包含标识网络层协议的字段
   3. PPP可以在建立连接期间检查链接质量
   4. 通过密码认证协议(PAP)和质询握手认证协议(CHAP)提供认证。

2. HDLC是Cisco串行线的默认封装

   1. 没有窗口或流量控制
   2. 框架中插入了专有类型(所有权)代码，这意味着HDLC帧不能与其他供应商的设备互操作。
   3. 当专用线路连接的两端是运行Cisco IOS的路由器时使用
   4. 不做出窗口控制和流控制

### ppp

![第10讲：广域网(wan)_页面_22](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第10讲：广域网(wan)_页面_22.jpg)

1. 串行链路上使用最广泛的第2层协议
2. 从SLIP开发，
   1. 仅支持IP协议
   2. 不支持动态IP分配
   3. 不支持身份验证
   4. 不支持压缩
   5. 不支持错误检测
3. PPP提供以下功能
   1. 网络协议多路复用
   2. 动态分配IP地址
   3. 验证：PAP，CHAP
   4. 压缩
   5. 错误检测

#### PPP组件

![第10讲：广域网(wan)_页面_23](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第10讲：广域网(wan)_页面_23.jpg)

使用HDLC(ISO HDLC，而非Cisco HDLC)作为封装第3层数据报的基础

实现LCP(链接控制协议)以：

1. 建立连接
2. 连接配置选项
3. 链接质量测试

实施NCP(网络控制协议，Network Control Protocol)以选择和配置第3层协议

#### PPP帧格式

![第10讲：广域网(wan)_页面_24](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第10讲：广域网(wan)_页面_24.jpg)

1. Flag: 01111110 标记：帧的开头或结尾，01111110，一位可能会连续接受到多个帧
2. Address：11111111，广播地址
3. Control：00000011，用户数据作为无序帧传输
4. Protocol: 数据字段中的协议类型
5. Data: 数据报，最大默认值为1500字节
6. FCS: 2或者4字节

#### PPP会话建立/终止

![第10讲：广域网(wan)_页面_25](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第10讲：广域网(wan)_页面_25.jpg)

1. 为了通过点对点链路建立通信，PPP经历四个不同的阶段：
   1. 步骤一:链接建立和配置协商(negotiation)(LCP)。
   2. 步骤二:链接质量测试。
   3. 步骤三:网络层协议配置(NCP)。
   4. 步骤四:链接终止。
2. 图示如下

![第10讲：广域网(wan)_页面_26](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第10讲：广域网(wan)_页面_26.jpg)

##### 连接建立

![第10讲：广域网(wan)_页面_27](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第10讲：广域网(wan)_页面_27.jpg)

1. 建立链接是交换任何网络层数据报之前的第一阶段
   1. 每个PPP设备发送LCP来打开连接
   2. LCP数据包包含一个配置选项字段，该字段允许设备协商选项的使用，例如**压缩和身份验证协议**等。
   3. 如果LCP数据包中未包含配置选项，则采用该配置选项的**默认值**。
   4. 当已发送和接收配置**确认**帧时，此阶段完成。
2. 在完成这个步骤前不会传输具体数据帧的。

##### 链路质量确定

![第10讲：广域网(wan)_页面_28](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第10讲：广域网(wan)_页面_28.jpg)

1. 发送和接收LCP数据包以测量链路上的错误率(如果已配置)
2. 身份验证(如果使用)在网络层协议配置阶段开始之前进行。(可选)
3. LCP可以延迟网络层协议信息的传输，直到完成此阶段。
4. 在这之前不能传输网络帧。

##### 网络层协议配置

![第10讲：广域网(wan)_页面_29](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第10讲：广域网(wan)_页面_29.jpg)

1. 在此阶段，PPP设备发送NCP数据包以选择和配置一个或多个网络层协议(例如IP)。
2. 配置了每个选定的网络层协议后，可以通过链接发送来自每个网络层协议的数据报。

##### 连接终止

![第10讲：广域网(wan)_页面_30](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第10讲：广域网(wan)_页面_30.jpg)

CP可以随时终止链接：

1. 应用户要求；(一方请求终止)
2. 链接质量
3. 超时

当LCP关闭链接时，它将通知网络层协议，以便它们可以采取适当的措施

#### PAP

![第10讲：广域网(wan)_页面_31](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第10讲：广域网(wan)_页面_31.jpg)

![第10讲：广域网(wan)_页面_32](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第10讲：广域网(wan)_页面_32.jpg)

1. 链接的发起方(Calling Side)输入身份验证信息，以帮助确保用户具有网络管理员的许可来进行连接。
2. 远程节点使用双向握手PAP建立其身份。
3. 远程节点**重复**发送用户名/密码对，直到确认身份验证或连接终止
4. 密码以明文形式通过链接发送。
5. 在建立连接阶段之后，仅对远程节点进行一次身份验证。

![第10讲：广域网(wan)_页面_33](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第10讲：广域网(wan)_页面_33.jpg)



![第10讲：广域网(wan)_页面_34](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第10讲：广域网(wan)_页面_34.jpg)

#### CHAP

![第10讲：广域网(wan)_页面_35](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第10讲：广域网(wan)_页面_35.jpg)



![第10讲：广域网(wan)_页面_36](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第10讲：广域网(wan)_页面_36.jpg)

1. 被叫方使用三向握手CHAP协议定期验证主叫方。
2. CHAP不允许呼叫者在没有Challenge(随机数)的情况下尝试进行身份验证。(Challenge->随机数)
3. 主机(称为参与者)将质询消息发送到远程节点。
4. 远程节点以一个值(加密的值，包括：接收到的质询，其用户名和密码)进行响应:value是challenge和密钥生成的
5. 主机根据自己的价值检查响应
   1. 如果值匹配，则确认身份验证
   2. 否则，连接终止

![第10讲：广域网(wan)_页面_37](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第10讲：广域网(wan)_页面_37.jpg)

- RTB请求连接RTA
- 他们都存储一个用户名密码，但是用户名不同，密码相同
- RTB发送一个连接请求
- RTA找一个时间来发起挑战
- 挑战中内容:
  - 编号
  - id是第几次挑战
  - random:生成的随机数
  - RTA:谁发起的挑战

![第10讲：广域网(wan)_页面_38](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第10讲：广域网(wan)_页面_38.jpg)

- RTB进行应答，
- RTB操作:pass + random 使用 MD5 算法 -> 哈希值

![第10讲：广域网(wan)_页面_39](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第10讲：广域网(wan)_页面_39.jpg)

RTA收到RTB的回复，然后比较是否相同

![第10讲：广域网(wan)_页面_40](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第10讲：广域网(wan)_页面_40.jpg)

![第10讲：广域网(wan)_页面_41](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第10讲：广域网(wan)_页面_41.jpg)

### ISDN

![第10讲：广域网(wan)_页面_42](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第10讲：广域网(wan)_页面_42.jpg)

![第10讲：广域网(wan)_页面_43](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第10讲：广域网(wan)_页面_43.jpg)

1. 集成服务数字网络允许通过现有电话线传输数字信号:提供远程站点的连接
2. ISDN具有以下优点：
   1. 可以携带语音，视频和数据
   2. 使用带外D(或Delta)信道比调制解调器(有时<1s)更快的呼叫建立
   3. 使用B(或屏障)通道以64kps提供更快的数据传输

#### BRI(Basic Rate Interface) and PRI(Primary Rate Interface)

![第10讲：广域网(wan)_页面_44](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第10讲：广域网(wan)_页面_44.jpg)

1. ISDN服务有两种：
   1. BRI(基本速率接口, Basic Rate Interface),用户虚拟电路数据传，HDLC,PPP
   2. PRI(主速率接口,Primary Rate Interface)，发送控制信息，LAPD
2. ISDN BRI服务提供两个B通道和一个D通道。
3. ISDN BRI将144kbps(2B + D = 144kps)线路的总带宽传送到三个单独的通道中。
4. BRI B信道服务以64 kbps的速率运行，旨在承载用户数据和语音流量。
5. 第三个通道，D通道，是一个16 kbps信令通道，用于承载指令，这些指令告诉电话网络如何处理每个B通道。
6. BRI和DRI都是基于电话信道的

![第10讲：广域网(wan)_页面_45](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第10讲：广域网(wan)_页面_45.jpg)

![第10讲：广域网(wan)_页面_46](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第10讲：广域网(wan)_页面_46.jpg)

1. ISDN利用一套(suit)ITU-T标准套件，涵盖OSI参考模型的物理，数据链路和网络层。
2. 有几种封装选择。两种最常见的封装是PPP和HDLC。
3. ISDN默认为HDLC。但是，PPP更为健壮，因为它为兼容链接和协议配置的身份验证和协商提供了出色的机制。
4. ISDN接口仅允许使用一种封装类型,不允许混合使用封装。

#### 非对称数字用户线路(ADSL,Asymmetric Digital Subscriber Line)

![第10讲：广域网(wan)_页面_47](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第10讲：广域网(wan)_页面_47.jpg)

![第10讲：广域网(wan)_页面_48](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第10讲：广域网(wan)_页面_48.jpg)

![第10讲：广域网(wan)_页面_49](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第10讲：广域网(wan)_页面_49.jpg)

![第10讲：广域网(wan)_页面_50](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第10讲：广域网(wan)_页面_50.jpg)

![第10讲：广域网(wan)_页面_51](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第10讲：广域网(wan)_页面_51.jpg)

![第10讲：广域网(wan)_页面_52](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第10讲：广域网(wan)_页面_52.jpg)

![第10讲：广域网(wan)_页面_53](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第10讲：广域网(wan)_页面_53.jpg)

![第10讲：广域网(wan)_页面_54](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第10讲：广域网(wan)_页面_54.jpg)

![第10讲：广域网(wan)_页面_55](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第10讲：广域网(wan)_页面_55.jpg)

![第10讲：广域网(wan)_页面_56](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第10讲：广域网(wan)_页面_56.jpg)

### SONET

![第10讲：广域网(wan)_页面_57](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第10讲：广域网(wan)_页面_57.jpg)

![第10讲：广域网(wan)_页面_58](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第10讲：广域网(wan)_页面_58.jpg)

![第10讲：广域网(wan)_页面_59](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第10讲：广域网(wan)_页面_59.jpg)

![第10讲：广域网(wan)_页面_60](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第10讲：广域网(wan)_页面_60.jpg)

![第10讲：广域网(wan)_页面_61](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第10讲：广域网(wan)_页面_61.jpg)

![第10讲：广域网(wan)_页面_62](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第10讲：广域网(wan)_页面_62.jpg)

![第10讲：广域网(wan)_页面_63](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第10讲：广域网(wan)_页面_63.jpg)

![第10讲：广域网(wan)_页面_64](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第10讲：广域网(wan)_页面_64.jpg)

### HFC

![第10讲：广域网(wan)_页面_65](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第10讲：广域网(wan)_页面_65.jpg)

![第10讲：广域网(wan)_页面_66](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第10讲：广域网(wan)_页面_66.jpg)

![第10讲：广域网(wan)_页面_67](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第10讲：广域网(wan)_页面_67.jpg)

![第10讲：广域网(wan)_页面_68](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第10讲：广域网(wan)_页面_68.jpg)

![第10讲：广域网(wan)_页面_69](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第10讲：广域网(wan)_页面_69.jpg)

![第10讲：广域网(wan)_页面_70](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第10讲：广域网(wan)_页面_70.jpg)

![第10讲：广域网(wan)_页面_71](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第10讲：广域网(wan)_页面_71.jpg)

![第10讲：广域网(wan)_页面_72](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第10讲：广域网(wan)_页面_72.jpg)

![第10讲：广域网(wan)_页面_73](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第10讲：广域网(wan)_页面_73.jpg)

![第10讲：广域网(wan)_页面_74](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第10讲：广域网(wan)_页面_74.jpg)