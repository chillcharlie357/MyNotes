# Lecture6-会话层、展示层应用层

# 会话层

![第6讲：会话、表示、应用层_页面_03](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第6讲：会话、表示、应用层_页面_03.jpg)



![第6讲：会话、表示、应用层_页面_04](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第6讲：会话、表示、应用层_页面_04.jpg)

1. 双向同步通讯？
   1. 全双工通信
   2. 半双工通信
   3. 单工通信
2. 双向交替控制？
   1. 会话连接、活动开始、数据校验(同步)
   2. 令牌转换等
3. 是否同步了您的会话的主题？

![第6讲：会话、表示、应用层_页面_05](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第6讲：会话、表示、应用层_页面_05.jpg)

**同步点(CheckPoint)**用于分隔会话的各个部分，以前称为对话(dialogues)

1. 同步点:发送一定数据后设置同步点
2. 次同步点:作为同步点的一个子集，进行数据校验
3. 主同步点:按照主同步点进行校验确认
4. 如果错误，恢复到上次都已经同步的主同步点

对话分离(Seperation)是通信的有序启动，终止和管理

尽量保证了通话的效率和可靠性

![第6讲：会话、表示、应用层_页面_06](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第6讲：会话、表示、应用层_页面_06.jpg)





# 展示层

## 概述

![第6讲：会话、表示、应用层_页面_08](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第6讲：会话、表示、应用层_页面_08.jpg)

表示层负责以接收**设备可以理解**的形式表示数据。

1. 传送语法协商
2. 接受语法协商

表示层具有3个主要功能：

1. 数据格式(format)
2. 数据压缩(compression):早期网络比较慢，倾向于先压缩在发送
3. 数据加密(encryption)

## 数据格式

![第6讲：会话、表示、应用层_页面_09](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第6讲：会话、表示、应用层_页面_09.jpg)

1. 想象两个不同(dissimilar)的系统。
   1. 一种使用扩展二进制编码的十进制交换码(EBCDIC,Extended Binary Coded Decimal Interchange Code)格式化文本
   2. 另一种使用**美国信息交换标准码(ASCII)**格式化文本
   3. 选择大家都能识别的编码形式传输，保证大家都能理解
2. 第6层提供了这两种不同类型的代码之间的转换

### 图形文件格式

![第6讲：会话、表示、应用层_页面_10](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第6讲：会话、表示、应用层_页面_10.jpg)

1. 互联网通常使用两种二进制文件格式来显示图像：
   1. 图形交换格式(GIF，Graphic Interchange Format)
   2. 联合图像专家组(JPEG，Joint Photographic Experts Group)。
2. 任何具有读取器的GIF和JPEG文件格式的计算机都可以读取这些文件类型，而与计算机的类型无关。

### 多媒体文件格式

![第6讲：会话、表示、应用层_页面_11](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第6讲：会话、表示、应用层_页面_11.jpg)

多媒体文件格式是另一种二进制文件，它存储声音，音乐和视频。

1. 这些文件可以完全下载，然后播放，也可以在播放时下载。
2. 后一种方法称为流音频。

## 数据加密和压缩

![第6讲：会话、表示、应用层_页面_12](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第6讲：会话、表示、应用层_页面_12.jpg)

1. 第6层负责数据加密：数据加密可在信息传输过程中保护信息。
2. 表示层还负责文件的压缩。

# 应用层

![第6讲：会话、表示、应用层_页面_14](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第6讲：会话、表示、应用层_页面_14.jpg)

第七层对应的是应用界面

## 应用层概述

![第6讲：会话、表示、应用层_页面_15](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第6讲：会话、表示、应用层_页面_15.jpg)

应用层（最接近用户）支持一个应用的交流模块

应用层：

- 确定并建立预期的通信合作伙伴的可用性
- 同步合作的应用程序
- 建立有关错误恢复程序的协议
- 控制数据完整性

## HTTP HyperText Transfrer Protocol超文本传输协议

![第6讲：会话、表示、应用层_页面_16](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第6讲：会话、表示、应用层_页面_16.jpg)



### URL统一资源定位符

![第6讲：会话、表示、应用层_页面_17](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第6讲：会话、表示、应用层_页面_17.jpg)

```
<URL的访问方式>://<主机>:<端口>/<路径>
```

1. 访问方式:协议HTTPS 或者 HTTP
2. 主机:域名的方式
3. 端口对应进程
4. 路径对应具体的文件

##  HTTP协议

![第6讲：会话、表示、应用层_页面_18](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第6讲：会话、表示、应用层_页面_18.jpg)

无状态的

## HTTP报文结构

![第6讲：会话、表示、应用层_页面_19](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第6讲：会话、表示、应用层_页面_19.jpg)

应答码：

1. 2xx:成功
2. 3xx:重定向
3. 4xx:错误
4. 5xx:服务器内部错误

### HTTP请求报文的一些方法

![第6讲：会话、表示、应用层_页面_20](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第6讲：会话、表示、应用层_页面_20.jpg)

### html超文本标记语言

![第6讲：会话、表示、应用层_页面_21](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第6讲：会话、表示、应用层_页面_21.jpg)

## FTP 和 TFTP

![第6讲：会话、表示、应用层_页面_22](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第6讲：会话、表示、应用层_页面_22.jpg)

FTP是一种可靠的，**面向连接**的服务，它使用TCP传输文件。

1. FTP首先在客户端和服务器(端口21)之间建立**控制连接**
2. 然后，建立第二个连接，这是计算机之间通过其传输数据的链接。(端口20)

TFTP是使用UDP的**无连接**服务(简化的FTP)

1. 体积小，易于实施。更加方便
2. 例如。 TFTP在路由器上用于传输配置文件和Cisco IOS映像
3. 不支持交互，没有目录浏览功能

互联网早期的时候，文件传输量是很大的。

### 主进程工作步骤

![第6讲：会话、表示、应用层_页面_23](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第6讲：会话、表示、应用层_页面_23.jpg)

![第6讲：会话、表示、应用层_页面_24](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第6讲：会话、表示、应用层_页面_24.jpg)

![第6讲：会话、表示、应用层_页面_25](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第6讲：会话、表示、应用层_页面_25.jpg)

![第6讲：会话、表示、应用层_页面_26](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第6讲：会话、表示、应用层_页面_26.jpg)

![第6讲：会话、表示、应用层_页面_27](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第6讲：会话、表示、应用层_页面_27.jpg)

![第6讲：会话、表示、应用层_页面_28](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第6讲：会话、表示、应用层_页面_28.jpg)

![第6讲：会话、表示、应用层_页面_29](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第6讲：会话、表示、应用层_页面_29.jpg)

![第6讲：会话、表示、应用层_页面_30](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第6讲：会话、表示、应用层_页面_30.jpg)

![第6讲：会话、表示、应用层_页面_31](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第6讲：会话、表示、应用层_页面_31.jpg)

![第6讲：会话、表示、应用层_页面_32](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第6讲：会话、表示、应用层_页面_32.jpg)

![第6讲：会话、表示、应用层_页面_33](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第6讲：会话、表示、应用层_页面_33.jpg)

![第6讲：会话、表示、应用层_页面_34](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第6讲：会话、表示、应用层_页面_34.jpg)

![第6讲：会话、表示、应用层_页面_35](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第6讲：会话、表示、应用层_页面_35.jpg)

![第6讲：会话、表示、应用层_页面_36](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第6讲：会话、表示、应用层_页面_36.jpg)

![第6讲：会话、表示、应用层_页面_37](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第6讲：会话、表示、应用层_页面_37.jpg)

![第6讲：会话、表示、应用层_页面_38](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第6讲：会话、表示、应用层_页面_38.jpg)

## Telnet
![第6讲：会话、表示、应用层_页面_39](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第6讲：会话、表示、应用层_页面_39.jpg)

Telnet客户端软件提供了登录到运行Telnet服务器应用程序的远程Internet主机，然后从命令行执行命令的功能。



## SMTP POP

![第6讲：会话、表示、应用层_页面_40](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第6讲：会话、表示、应用层_页面_40.jpg)

电子邮件服务器使用SMTP发送和POP接收邮件相互通信。

1. SMTP (Simple Mail Transfer Protocol) SMTP(简单邮件传输协议)邮件发送，登录发送等操作
2. POP3 (Post Office Protocol version 3) 邮件接收，邮件到达邮件服务端，由客户端和服务端联系接收邮件。

![第6讲：会话、表示、应用层_页面_41](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第6讲：会话、表示、应用层_页面_41.jpg)

发送者先登录到服务器，通过服务器根据SMTP传输到对应的服务器，然后用户登录后通过POP3协议收邮件到本地

### MIME Multipurpose Internet Mail Extensions 因特网协议扩充

![第6讲：会话、表示、应用层_页面_42](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第6讲：会话、表示、应用层_页面_42.jpg)

将非ASCII码的文件转换成ASCII文件

![第6讲：会话、表示、应用层_页面_43](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第6讲：会话、表示、应用层_页面_43.jpg)

## SNMP Simple Network Management Protocol 简单网络管理协议

![第6讲：会话、表示、应用层_页面_44](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第6讲：会话、表示、应用层_页面_44.jpg)

一种促进管理信息交换的应用层协议

网管,通过下发请求对上网的所有的主机关于流量等等信息进行管理(监控)

## DNS Domain Name System域名系统

![第6讲：会话、表示、应用层_页面_45](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第6讲：会话、表示、应用层_页面_45.jpg)

域名系统(DNS)是网络上的服务，该服务管理域名并响应客户端将域名转换为关联IP地址的请求。

1. 早期是用IP地址以及Host文件来进行访问

### Domain Name 域名

![第6讲：会话、表示、应用层_页面_46](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第6讲：会话、表示、应用层_页面_46.jpg)

### TLD Top Level Domain 顶级域

![第6讲：会话、表示、应用层_页面_47](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第6讲：会话、表示、应用层_页面_47.jpg)

国家TLD(nTLD)

1. .cn(CHINA) 中国
2. .us (United States) 美国
3. .uk (United kingdom), etc. 英国等等

通用TLD(gTLD)，最早的域包括：

1. .com Enterprises and companies 企业和公司
2. .net Network services providers 网络服务提供者
3. .org Nonprofit organizations 非盈利组织
4. .edu Educational facilities 教育机构
5. .gov Governments (only for U.S.A) 政府(美国)
6. .mil Military facilities (only for U.S.A) 军方(美军)
7. .int International organizations 国际组织

![第6讲：会话、表示、应用层_页面_48](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第6讲：会话、表示、应用层_页面_48.jpg)

Infrastructure domain 基础设施领域

1. Only one: arpa, for resolving domain names reversely 仅一个：arpa，用于反向解析域名

Recently, new TLD domain added:

1. .aero(航空运输企业)
2. .biz (公司和企业)
3. .cat (加泰隆人的语言和文化团体)
4. .coop(合作团体)
5. .info(各种资讯)
6. .jobs(人力资源管理者)
7. .mobi(移动产品与服务的用户和提供者)
8. .museum (博物馆)
9. .name   (个人)
10. .pro (经过认证的专业人员)
11. .travel  (旅游业)

### 域名服务器

![第6讲：会话、表示、应用层_页面_49](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第6讲：会话、表示、应用层_页面_49.jpg)

- 顶级域名底下的域名就是由顶级域名下面进行管理
- 根域名服务器存储位置，所以子服务器知道根服务器的地址即可

### 结合域名服务器查找IP地址

![第6讲：会话、表示、应用层_页面_50](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第6讲：会话、表示、应用层_页面_50.jpg)

1. DNS系统以层次(hierarchy)结构设置，该层次结构创建不同级别的DNS服务器。
2. 此级别的DNS服务器判断其自身是否能够将域名转换为关联的IP地址：
   1. 如果可以，则将结果返回给客户端
   2. 如果没有，它将请求发送到更高级别。(向上级请求)

![第6讲：会话、表示、应用层_页面_51](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第6讲：会话、表示、应用层_页面_51.jpg)

- 请求分为两种:
  - 能够应答
  - 不能够应答
- 递归地进行查找:具体过程在上图
- 下面递归，上面迭代

### 应用层：通讯方式

![第6讲：会话、表示、应用层_页面_52](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第6讲：会话、表示、应用层_页面_52.jpg)

1. 通信处理发生的一种方式：(无上下文，请求后就断开)
   1. 当浏览器打开时，它将连接到默认页面，并且该页面的文件将传输到客户端。
   2. 处理完成后，连接断开
2. 第二种方式：(有上下文)
   1. 作为Telnet和FTP，建立与服务器的连接并保持该连接，直到执行所有处理。
   2. 当用户确定他/她已完成时，客户端将终止连接。
3. 所有的交流活动都属于这两类之一。



## DHCP Dynamic Host Configuration Protocol 动态主机配置协议 

## DHCP工作原理

![第6.1讲：dhcp_页面_03](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第6.1讲：dhcp_页面_03.jpg)

### DHCP过程

![第6.1讲：dhcp_页面_04](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第6.1讲：dhcp_页面_04.jpg)

AB是两个Server

Client先Discover去搜索

Server返回一个Offer报文

Client选择优先返回的Offer来优先服务

Client进行广播，告知到底服务了谁

然后B返回一个Ack报文

到了时间之后，选择release或者续租

### 发现阶段

![第6.1讲：dhcp_页面_05](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第6.1讲：dhcp_页面_05.jpg)

### 响应阶段

![第6.1讲：dhcp_页面_06](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第6.1讲：dhcp_页面_06.jpg)

### 选择问题

![第6.1讲：dhcp_页面_07](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第6.1讲：dhcp_页面_07.jpg)

### 租约确认问题

![第6.1讲：dhcp_页面_08](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第6.1讲：dhcp_页面_08.jpg)

### 租期续约

![第6.1讲：dhcp_页面_09](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第6.1讲：dhcp_页面_09.jpg)

### 租期释放

![第6.1讲：dhcp_页面_10](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第6.1讲：dhcp_页面_10.jpg)

### DHCP报文结构

![第6.1讲：dhcp_页面_11](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第6.1讲：dhcp_页面_11.jpg)

1. op:报文类型，1请求，2应答
2. HTYPE:硬件地址类型，1表示10M以太网地址
3. HLEN:以太网地址长度，10M为6
4. Hops:是否使用代理服务器进行处理

### 报文类型

![第6.1讲：dhcp_页面_12](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第6.1讲：dhcp_页面_12.jpg)

1. DHCP Discover：发现
2. DHCP Offer：提供
3. DHCP Request：告知决定
4. DHCP ACK：租约确认
5. DHCP NAK：租约不确认
6. DHCP Release：释放租约
7. DHCP Decline:收到Ack后，Client告诉服务器不接受
8. DHCP Inform:客户端向服务器端请求详细信息

## DHCP 欺骗与防范

![第6.1讲：dhcp_页面_14](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第6.1讲：dhcp_页面_14.jpg)

![第6.1讲：dhcp_页面_15](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第6.1讲：dhcp_页面_15.jpg)

![第6.1讲：dhcp_页面_16](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第6.1讲：dhcp_页面_16.jpg)

![第6.1讲：dhcp_页面_17](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第6.1讲：dhcp_页面_17.jpg)

![第6.1讲：dhcp_页面_18](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/第6.1讲：dhcp_页面_18.jpg)



ARP ip->mac

RARP mac->ip