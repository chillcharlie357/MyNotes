# 《互联网计算》名词解释

| 简写               | 全称                                                         | 中文                                | 解释                                                         |
| ------------------ | ------------------------------------------------------------ | ----------------------------------- | ------------------------------------------------------------ |
| LAN                | Local Area Networks                                          | 局域网                              | 是指在某一区域内由多台计算机互联成的计算机组                 |
| WAN                | Wide Area Networks                                           | 广域网                              | 指一种**跨地区的数据通讯网络**,通常包含一个国家或地区        |
| ISP                | Internet Service Providers                                   | 互联网服务提供商                    | 向广大用户综合提供互联网**接入业务**、信息业务、和增值业务的电信运营商 |
| ICP                | Internet Content Provider                                    | 互联网内容提供商                    | 向广大用户综合提供互联网信息业务和增值业务的电信运营商       |
| NAP                | Network Access Point                                         | 网络接入点                          | 是因特网的路由选择层次体系中的通信交换点                     |
| OSI                | Model **Open System Interconnection**                        | 开放式系统互联                      | OSI 将计算机网络体系结构（architecture）划分为以下七层：物理层、数据链路层、网络层、传输层、会话层、表示层、应用层 |
| ISO                | **International** Organization for Standardization           | 国际标准化组织                      | 在全世界范围内促进标准化工作的开展                           |
| TCP                | **Transmission** Control Protocol                            | 传输控制协议                        | 一种面向连接的、可靠的、基于字节流的传输层通信协议，不支持单播和组播 |
| UDP                | User Datagram Protocol                                       | 用户数据报协议                      | 是 OSI 参考模型中一种无连接的传输层协议，提供面向事务的简单不可靠、无连接、无确认、无流控制的信息传送服务 |
| IP                 | Internet Protocol                                            | 网际互联协议                        | 由 network ID 与 host ID 组成,0-127 Class A address,128-191 Class B  address,192-223 Class C address,224-239 Class D-Multicast,240-255 Class  E-Research |
| FTP                | File Transfer Protocol                                       | 文件传输协议                        | 用于在网络上进行文件传输的应用层协议，使用 TCP 传输。FTP是可靠的，面向连接的服务。基于**TCP 20和21端口**（*工作流程：首先通过套接字建立控制连接，然后建立数据连接，通过数据连接传输数据*） |
| HTTP               | Hypertext Transfer Protocol                                  | 超文本传输协议                      | HTTP 是**面向事务**、**无状态**、**无连接**的客户服务器协议。 |
| SMTP               | Simple Mail Transfer protocol                                | 简单邮件**发送**协议                | 它是一组用于由源地址到目的地址传送邮件的规则，由它来控制信件的中转方式。SMTP 协议属于 TCP/IP  协议族，它帮助每台计算机在发送或中转信件时找到下一个目的地。通过 SMTP 协议所指定的服务器,就可以把 E-mail  寄到收信人的服务器上了，整个过程只要几分钟。SMTP 服务器则是遵循 SMTP 协议的发送邮件服务器，用来发送或中转发出的电子邮件。用于：  用户代理把邮件传送到服务器；在邮件服务器之间的传送 |
| DNS                | Domain Name System                                           | 域名系统                            | 因特网上作为域名和 IP 地址相互映射的一个分布式数据库，将域名转化为 IP  地址，能够使用户更方便的访问互联网，而不用去记住能够被机器直接读取的 IP  数串。共分为三级域名、二级域名、顶级域名。因特网采用层次树状结构命名方法，由多个域名服务器共同完成。每个服务器接收到域名后尝试解析，如果不能解析则传给上一层服务器。 |
| TFTP               | **Trivial** File Transfer Protocol                           | **普通**文件传输协议                | TCP/IP 协议族中的一个用来在客户机与服务器之间进行简单文件传输的协议，<u>提供不复杂、开销不大的文件传输服务。无连接的服务。基于 UDP</u>，小且容易实现 |
| NIC                | Network Interface Card                                       | 网卡                                | 是电脑与局域网相互连接的设备，数据链路层                     |
| UTP                | Unshielded Twisted Pair                                      | 无屏蔽双绞线                        | 由 8 根不同颜色的线分成 4 对绞合在一起，无金属屏蔽材料；线缆不需要接地，因此便于在线缆末端加上连接器；<u>价格低廉</u>；<u>直径小</u>，因此<u>安装简单</u>且更适合安装在管道中；和其他铜传输介质具有一样的数据传输速率；使用 RJ 连接器后极大地降低了噪音的影响。 |
| TDM                | Time Division Multiplexing                                   | 时分复用                            | 采用同一物理连接的不同时段来传输不同的信号，达到多路传输的目的；或者是时分复用是将时间划分为一段段等长的时分复用（TDM）帧，每个时分复用的用户在每个 TDM 帧中占用固定序号的时隙 |
| STDM               | Static Division Multiplexing                                 | 统计时分复用                        | 是一种根据用户实际需要动态分配线路资源的时分复用方法         |
| FDM                | Frequency Division Multiplexing                              | 频分复用                            | 用户在分配到一定的频带后，在通信过程中自始至终都占用这个频带 |
| WDM                | Wavelength（波长） Division Multiplexing                     | 波分复用                            | 就是光的频分复用                                             |
| CDM                | **Code** Division Multiplexing                               | **码**分复用                        | 常用的名词是码分多址 CDMA（Code Division Multiple Access）靠不同的编码来区分各路原始信号的一种复用方式 |
| CDMA               | Code Division Multiple Access                                | 码分多址                            | 是一种多路方式，多路信号只占用一条信道                       |
| LLC                | Logical Link Control                                         | 逻辑链路控制                        | 是局域网中数据链路层的上层部分。用户的数据链路服务通过 LLC 子层为网络层提供统一的接口。逻辑上标志不同的协议类型并且封装起来。处理差错通知，网络拓扑和流控制。 |
| MAC                | Media Access Control                                         | 介质访问控制                        | OSI 模型中，数据链接层的子层。MAC 地址是 OSI 模型中对应数据链路层的地址，每个网络位置都有一个唯一的编号。定义了物理线路上怎样传输帧。解决了物理地址问题，定义网络拓扑和流水线 |
| CSMA/CD            | Carrier Sense Multiple Access with Collision Detection       | 带冲突检测的载波侦听多路访问        | 带冲突检测的载波侦听多路访问（Carrier Sense Multiple Access with Collision  Detection），“多点接入”就是说明这是总线型网络，“载波监听”就是用电子技术检测总线上有没有其他计算机也在发送。“碰撞检测”也就是“边发送边监听”，即适配器边发送数据边检测信道上的信号电压的变化情况，以便判断自己在发送数据时其他站是否也在发送数据。 |
| CSMA/CA            | Carrier Sense Multiple Access with Collision Avoidance       | 避免冲突的载波侦听多路访问          |                                                              |
| AP                 | Access Point                                                 | 接入点                              |                                                              |
| SAP                | Service Access Point                                         | 服务访问点                          |                                                              |
| DSAP               | Destination Service Access Point                             | 目标服务访问点                      |                                                              |
| SSAP               | the Source Service Access Point                              | 源服务访问点                        |                                                              |
| OUI                | Organizational Unique Identifier                             | 组织唯一标识符                      |                                                              |
| DSSS               | Direct Sequence Spread Spectrum                              | 直接序列扩频                        |                                                              |
| OFDM               | Orthogonal Frequency Division Multiplexing                   | 正交频分复用技术                    |                                                              |
| BSS                | Basic Service Set                                            |                                     |                                                              |
| ESS                | Extended Service Set                                         |                                     |                                                              |
| BS                 | Base Station                                                 |                                     |                                                              |
| SSID               | Service Set Identifier                                       |                                     |                                                              |
| WLAN               | Wireless Local Area Network                                  | 无线局域网                          | 指应用无线通信技术将计算机设备互联起来，构成可以互相通信和实现资源共享的网络体系 |
| ACK                | Acknowledgment                                               |                                     |                                                              |
| NAT                | Network Address Translations                                 | 网络地址转换                        | 网络地址转换，将网络内部的私有 IP 地址转换为公有 IP 地址以节省 IP 地址的方法。只能一对一映射 |
| PAT                | Port Address Translation                                     | 端口地址转换                        | 是对网络地址转换（NAT）的扩展，它允许<u>本地网（LAN）上的多个设备映射到一个单一的公共 IP 地址</u> |
| CIDR               | Classless InterDomain Routing                                | 无类域间路由                        | 无类域间路由（Classless Inter-Domain  Routing），是一个在因特网上创建附加地址的方法，这些地址提供给服务提供商（ISP），再由 ISP 分配给客户，（VLSM + CIDR  就形成了如 192.168.1.0/28 子网掩码 255.255.255.240 这样的形式） |
| RARP               | Reverse Address Resolution Protocol                          | 反向地址解析协议                    | 反向地址解析协议（Reverse Address Resolution Protocol），ARP 为 IP 到 MAC 的转换，而  RARP 为 MAC 到 IP 的转换，向 RARP 服务器请求分配 IP。主要流程：发出要反向解析的物理地址并希望返回其对应的 IP  地址。发送主机发送一个本地的 RARP 广播，在此广播包中，声明自己的 MAC 地址并且请求任何收到此请求的 RARP 服务器分配一个 IP  地址。 本地网段上的 RARP 服务器收到此请求后，检查其 RARP 列列表，查找该 MAC 地址对应的 IP 地址。 如果存在，RARP  服务器器就给源主机发送一个响应数据包并将此 IP 地址提供给对方主机使用；如果不存在，RARP 服务器对此不做任何的响应。 源主机收到从  RARP 服务器的响应信息，就利用得到的 IP 地址进行通讯；如果一直没有收到 RARP 服务器的响应信息，表示初始化失败。 |
| BOOTP              |                                                              |                                     |                                                              |
| DHCP               | Dynamic Host Configuration Protocol                          | 动态主机配置协议                    | 是一个局域网的网络协议，使用 UDP 协议工作，主要有两个用途：给内部网络或网络服务供应商自动分配 IP 地址，给用户或者内部网络管理员作为对所有计算机作中央管理的手段 |
| ARP                | Address Resolution Protocol                                  | 地址解析协议                        | 把 IP 地址解析为硬件地址，解决了同一个局域网上的主机或者路由器的 IP 地址和硬件地址的映射问题。ARP 的高速缓存可以大大减少网络上的通信量。只针对同一网段 |
| IGP                | Interior Gateway Protocols                                   | 内部网关协议                        |                                                              |
| EGP                | Exterior Gateway Protocols                                   | 外部网关协议                        |                                                              |
| DVP                | Distance Vector Protocols                                    | 距离矢量协议                        | 距离矢量路由算法是动态路由算法。它是这样工作的：每个路由器维护一张矢量表，表中列出了当前已知的到 每个目标的最佳距离，以及所使用的线路。通过在邻居之间相互交换信息，路由器不断地更新它们内部的表。 |
| LSP                | Link State Protocols                                         | 链路状态协议                        | <u>每个路由器都了解整个网络的拓扑结构，利用算法计算两个路由之间的最短路径</u>，更新由事件触发，每次更新都只向周围的路由器传递路由表的更新信息，包括 OSPF 等 |
| RIP                | **Routing Information Protocol**                             | 路由信息协议                        | 分布式<u>基于距离向量的路由选择协议，只适用于小型网络</u>。按固定的时间间隔与相邻路由器交换信息。交换的信息是自己当前的路由表，即到达本自治系统中所有网络的（最短）距离，以及到每个网络应经过的下一跳路由器。但是不知道全网的拓扑结构。 |
| IGRP               | Interior Gateway Route Protocol                              | 内部网关路由协议                    |                                                              |
| EIGRP              | Enhanced Interior Gateway Routing Protocol                   |                                     |                                                              |
| SPF                | Shortest Path First                                          | 最短路径优先算法                    | 最短路径算法，这个算法其实就是 Dijkstra 算法，是 LSP 中计算路径的一种方式 |
| OSPF               | Open Shortest Path First                                     | 最短路径优先协议                    | 基于分布式的链路状态协议，适用于大型互联网。OSPF 只在链路状态发生变化时，才用向本自治系统内的所有路由器，用洪泛法发送与本路由器相邻的所有路由器的链路状态信息，最终了解全网的拓扑结构图。 |
| VLSM               | Variable Length Subnet Mask                                  | 可变长度子网掩码                    | 规定了一个网络在划分子网时的不同部分使用不同的子网掩码，更有效的使用 IP 地址；使用路由汇总的能力更强 |
| ICMP               | Internet Control Message Protocol                            | 因特网控制报文协议                  | 因特网控制报文协议（Internet Control Message Protocol），是为了提高 IP 数据报交付成功的机会，允许主机或路由器报告差错情况和提供有关异常情况报告的协议，运行在 IP 层。 |
| IGMP               | Internet Group Management Protoco                            | 网络组管理协议                      |                                                              |
| PING               | Packet InterNet Groper                                       | 因特网包探测器                      | 用于测试网络连接量的程序。使用了 ICMP 回送请求和回送回答报文，是应用程序直接使用网络层 ICMP 的例子，没有通过 TCP/UDP |
| MSS                | Maximum Segment Size                                         | 最大报文段长度                      |                                                              |
| MTU                | Maximal Transfer Unit                                        | 最大传输单位                        |                                                              |
| EBCDIC             | Extended Binary Coded Decimal Interchange Code               | 扩展二进制编码的十进制交换码        |                                                              |
| ASCII              | American Strandard Code for Information Interchange          | 美国信息交换标准码                  |                                                              |
| JPEG               | Joint Photographic Experts Group                             | 联合图像专家组                      |                                                              |
| GIF                | Graphic Interchange Format                                   | 图像互换格式                        |                                                              |
| URL                | Uniform Resource Locator                                     | 统一资源定位符                      | 是因特网的万维网服务程序上用于指定信息位置的表示方法，<协议>：//<主机域名或者ip地址>：<端口号>/<路径> |
| HTML               | HyperText Markup Language                                    | 超文本标记语言                      | 定义了许多用于排版的命令(标签)是一种可以用任何文本编辑器创建的 ASCII 码文件 |
| POP3               | Post Office Protocol version 3                               | **邮局协议**版本3                   | 用于从服务器读取邮件。本协议主要用于支持使用客户端远程管理在服务器上的电子邮件 |
| MIME               | Multipurpose Internet Mail Extensions                        | 因特网协议扩充                      |                                                              |
| SNMP               | the Simple Network Management Protocol                       | 简单网络管理协定                    | 使用UDP的应用层协议                                          |
| TLD                | Top Level Domain                                             | 顶级域                              | 国家TLD:`.cn .us .uk`,通用TLD:`.com .net .org .edu`          |
| DHCP               | Dynamic Host Configuration Protocol                          | 动态主机配置协议                    |                                                              |
| IS-IS              | Intermediate System-to-Intermediate System                   |                                     |                                                              |
| EIGRP              | Enhanced Interior Gateway Routing Protocol                   |                                     |                                                              |
| STP                | the Spanning-Tree Protocol                                   | 生成树协议                          | 该协议可应用于在网络中建立树形拓扑，消除网络中的环路，并且可以通过一定的方法实现路径冗余，但不是一定可以实现路径冗余 |
| STP                | Shielded Twisted Pair                                        | 屏蔽双绞线                          | 是一种广泛用于数据传输的铜质双绞线                           |
| BPDU               | bridge protocol data units                                   | 桥接数据单元                        | 用于在STP中传递拓扑信息、选举等                              |
| CSU                | Channel Service Units                                        | 通道服务单元                        |                                                              |
| DSU                | Digital Service Units                                        | 数字服务单元                        |                                                              |
| PPP                | Point-to-Point Protocol                                      | 点对点协议                          | 是为在同等单元之间传输数据包这样的简单链路设计的数据链路层协议，是数据链路层使用最多的一种协议。这种链路提供全双工操作，并按照顺序传递数据包。特点为：简单；只检测差错而不纠正差错，不进行流量控制；支持多种网络层协议。 |
| HDLC               | High-Level Data Link Control                                 | 高级数据链路控制                    | 是一个在同步网上传输 数据、面向比特的数据链路层协议          |
| LAPB               | Link Access Procedure, Balanced                              | 平衡的链路访问程序                  |                                                              |
| ISDN               | Integrated Services Digital Networks                         | 综合数字服务网络                    | 以电话综合数字网为基础发展成的通信网，能提供端到端的数字连接，用来承载包括话音和非话音在内的多种电信业务。 |
| ADSL               | Asymmetric Digitla Subscriber Line                           | 非对称数字用户线路                  | 非对称数字用户线路（Aysmmetric Digital Subscriber Line），用数字技术对现有的模拟电话用户线进行改造，使它能承载宽带数字业务。ADSL 下行带宽远远大于上行带宽，因此得名“非对称”。 |
| HFC                | Synchronous Optical Network                                  | 同步光纤网                          |                                                              |
| SONET              | Synchronous Optical Network                                  | 同步光纤网                          |                                                              |
| CHAP               | Challenge Handshake Authentication  Protocol                 | 挑战握手认证协议                    | 挑战握手认证协议（Challenge Handshake Authentication  Protocol），链路建立阶段结束之后，认证者向对端点发送“challenge”消息；对端点用经过单向哈希函数计算出来的值做应答；认证者根据它自己计算的哈希值来检查应答，如果值匹配，认证得到承认，否则连接应该终止；经过一定的随机间隔，认证者发送一个新 challenge 给端点，重复上述步骤 |
| **PAP**            | Password Authentication Protocol                             | 密码认证协议                        | 远程节点不停的在链路上反复发送用户名/密码，直到验证通过或者连接终止。不健壮的身份认证协议，使用明文发送密码。连接建立前只有一次认证。 |
| IEEE MAC Sub-layer | Institute of Electrica and Electronics Engineers MAC Sub_layer | 电气与电子工程师学会的 MAC 子层划分 | IEEE 将数据链路层分成 LLC（Logical Link Control，逻辑链路控制）和  MAC（Media Access Control，介质访问控制）两个子层。MAC 控制各个 host 对 media 的使用权。MAC  子层定义了 frame 如何在物理线上运输，处理物理地址，定义网络拓扑和网线使用规则。 |
| Split Horizon      |                                                              | 水平分割                            | 是一种避免路由环路的出现和加快路由汇聚的技术。水平分割法的规则和原理是路由器从某个接口接收到的更新信息不允许再从这个接口发回去。 |
| Flow Control       |                                                              | 流量控制                            | 让发送方的发送速率不要太快，要让接收方来得及接收。           |
| ACL                | Access Control Lists                                         | 访问控制列表                        | 是路由器和交换机接口的指令列表，用来控制端口进出的数据包     |
| ARQ                | Automatic Repeat-reQuest                                     | 自动重传请求                        | 是 OSI 模型中**数据链路层**的错误纠正协议之一                |
| B Channel          | Barrier Channel                                              | B 信道                              | 用于电路交换（circuit-switch）的数据，通过 PPP 或 HDLC，B 信道工作在 64 kbps 的速率下，用于传输数据和语音流量 |
| BRI                | Basic Rate Interface                                         | 基本速率接口                        | BRI = 2B + D                                                 |
| CRC                | Cyclic Redundancy Check                                      | 循环冗余校验                        |                                                              |
| D Channel          | Delta Channel                                                | D 信道                              | 信号信息（signaling information），通过 LAPD（Link Access Procedure of D-Channel，D 信道链路规程），D 信道工作在 16 kbps 的速率下，用于告诉公用交换电话网络如何处理 B 信道 |
| DOS                | Disk Operation System                                        | 磁盘操作系统                        | 是个人计算机上的一类操作系统                                 |
| DR                 | Designated Router                                            | 指定路由器                          | 指定路由器，在 OSPF 多路访问网络中，在同一个区域内被选举出来代表所有路由的路由。为了减少在同一个网段中几个邻居互相交换信息的数量 |
| Internet           | Internet                                                     | 互联网                              | 互联网；指当前全球最大的、最开放的由众多网络相互连接而成的特定互连网，它采用 TCP/IP 协议族作为通信的规则，且其前身是美国的 ARPANET |
| internet           | internet                                                     | 互连网                              | 泛指多个计算机网络互连而成的计算机网络                       |
| NRZ                | Non-Return to Zero                                           | 不归零制码                          | 信号电平的一次反转代表 1，电平不变化表示 0，并且在表示完一个码元后，电压不需回到 0 |
| NVRAM              | **Non-volatile** Random Access Memory                        | 非易失随机存取存储器                | 非易失性 RAM，用来存储存储备份或启动配置文件，关机或重启后信息不会丢失 |
| POST               | Power On Self Test                                           | 开机自检                            |                                                              |
| RAM                | Random Access Memory                                         | 随机存取存储器                      |                                                              |
| ROM                | Read-only Memory                                             | 只读存储器                          |                                                              |
| RZ                 | Return to Zero                                               | 归零制码                            | 是信号电平在一个码元之内都要恢复到零的编码方式               |
| TTL                | Time to Live                                                 | 生存时间                            |                                                              |
| VLAN               | Virtual Local Area Network                                   | 虚拟局域网                          | 是一组逻辑上的设备和用户，这些设备和用户并不受物理位置的限制，可以根据功能、部门及应用等因素将它们组织起来，相互之间的通信就好像它们在同一个网段中一样，由此得名虚拟局域网。用于划分逻辑子网。工作在第二层和第三层。**可以分割广播域** |
| VPN                | Virtual Private Network                                      | 虚拟专用网络                        |                                                              |
|                    |                                                              |                                     |                                                              |
|                    |                                                              |                                     |                                                              |
|                    |                                                              |                                     |                                                              |
|                    |                                                              |                                     |                                                              |
|                    |                                                              |                                     |                                                              |

1. Frame Relay；帧中继；是一种用于连接计算机系统的面向分组的通信方法。它主要用在公共或专用网上的局域网互联以及广域网连接。大多数公共电信局都提供帧中继服务，把它作为建立高性能的虚拟广域连接的一种途径。
2. Data；数据；数据是二进制序列的表示。
3. Protocol；协议；协议定义消息传输和传递的详细方式。
4. Data Packets；数据分组；为了传输方， 计算机数据通常被分解成小的、易传输的单元，称为数据分组。
5. Symmetric Switching： 对称交换。交换机上所有端口带宽一样
6. Asymmetric Switching： 非对称交换。不同端口带宽不同
7. Store-and-Forward： **储存转发式交换**。交换机收到帧后，先校验 CRC 再转发
8. Cut-through： **直通式交换**。**不校验**就转发
9. Fast forward Switching： **快速转发交换**。**只查看目的 MAC** 地址后就转发。
10. Fragment Free： 免碎片。转发前查看帧前 64 字节以减少线路上噪声造成的错误。
11. L2 Switching： Layer 2 Switching，二层交换
12. L3 Switching： Layer 3 Switching，三层交换
13. L4 Switching： Layer 4 Switching，四层交换
14. Multilayer Switching： 多层交换
15. backbone： 主干。用于 VLAN 间的通信。
16. Frame Filtering： 帧过滤。阻止不符合条件的帧。
17. Frame Tagging： 帧标记。在每个要被在主干线路上转发的帧的头部加上一个独特的标签，用来标识它来自哪一个 VLAN。离开主干线路时被去除。
18. Static VLAN： 静态 VLAN。直接指派端口所属的 VLAN。
19. Dynamic VLAN： 动态 VLAN。当有新的节点插入端口时，交换机查表来动态配置这个端口所属的 VLAN
20. Port-Centric VLAN： 以端口为中心的 VLAN。同一 VLAN 下的所有节点接入到同一个路由器接口上，或者反过来说，接入同一个路由器端口的节点被划分到同一个 VLAN 下。
21. Access Link： 接入链路。其上只有一个 VLAN 的链路。这个 VLAN 被称为这个链路对应的端口的本地 VLAN。
22. Trunk Link： Trunk 链路（就这么叫吧，硬要叫的话是主干链路）。**其上有多个 VLAN  的链路**。用于连接交换机与交换机或路由器。（总之其实就是一根线上多个 VLAN 的帧在跑，所以这些帧得打上标签标识它来自于哪一个  VLAN，不然就搞混了。到达对面的交换机之后再根据标签把这些帧转发到对应的 VLAN 里面去。Trunk  链路最大的好处只是省端口和方便配置，以牺牲一点性能为代价。）
23. Trunk 链路也可以有本地 VLAN，即在 trunk 链路因为一些原因失败的时候使用的 VLAN。
24. Routing Between VLANs： VLAN 间路由
25. Infrastructure Mode：基础建设模式
26. Toll Network ：长途通信网
27. CO Switch ：中心局交换机
28. 流量控制 Flow Control：让发送方的发送速率不要太快，要让接收方来得及接收。
29. 拥塞控制 Congestion Control：防止过多的数据注入到网络中，这样可以使网络中的路由器或链路不致过载。
30. Socket：TCP 连接的端点，表示为（IP address：port）。一个连接表示为（socket_sourse，socket_des）
31. Computer virus 病毒：编制者在计算机程序中插入的破坏计算机功能或者数据的代码，能影响计算机使用，能自我复制的一组计算机指令或者程序代码
32. simplex transmission：单工。只能有一个方向的通信，没有反方方向的交互。
33. half-duplex transmission：半双工。信号可双向传输，但不能同时发送或同时接收。
34. full-duplex transmission：全双工。通信的双方可同时发送和接收消息，信号可同时双向传输。
35. Split horizon：水平分割是一种避免路由环路的出现和加快路由汇聚的技术。水平分割法的规则和原理是路由器从某个接口接收到的更新信息不允许再从这个接口发回去。
36. 冲突域（物理分段）：连接在同一导线上的所有工作站的集合，或者说是同一物理网段上所有节点的集合或以太网上竞争同一带宽的节点集合。
37. 广播域：接收同样广播消息的节点的集合。
