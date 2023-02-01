# 物理层

![image-20221222151520606](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/image-20221222151520606.png)

## 网络连接类型

![image-20221222151627580](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/image-20221222151627580.png)

多路复用和点对点通信

![image-20221222151648105](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/image-20221222151648105.png)

1. 多路复用共享介质
   - 多个主机可以访问同一介质
   - 这意味着它们都共享相同的介质—即使"wire"可能是UTP，它有四对线
2. 点对点(Point To Point)网络
   1. 一个设备通过链路连接到另一个设备
   2. 最广泛地应用于拨号网络连接，也是你最熟悉的一种。使用电信号来完成传输。

## 局域网介质

![image-20221222153405955](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/image-20221222153405955.png)

第一类在铜线电缆传输电信号，第二类传输光脉冲光线信号光信号，第三类传输无线电波。

1. 功能是传输数据，物理层负责将01信号转化成以上三种形式进行传播

2. 数字信号转化成光信号、无线信号等传输过程称为**编码**

3. 电缆类型包括STP(有屏蔽双绞线)、UTP(无屏蔽双绞线)、coaxial同轴电缆、fiber-optics光纤、空气（无线电波）

   **STP**是内每对线都包裹一层铝箔（共有4个单独的铝箔包裹），外编织网的双层屏蔽网线。UTP是非屏蔽双绞线；

可通过调节频率（调频）、电压（调幅）、相位（调相）等方式来实现不同01编码，编码好坏影响传输效果



### UTP (无屏蔽双绞线 Unshielded Twisted Pair)

![image-20221222154419647](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/image-20221222154419647.png)

由八根子线组成，两个线组合成一组，共四组，每一组被称为Twisted Pair可以保证每一组电流抵消电磁波干扰(抗干扰能力有限)

1. 依赖于消除效应，由双绞线对产生，用来限制由EMI和RFI引起的信号退化
2. 有四对铜线，阻抗(impedance)为100欧姆，频率低、接口小、布线更加方便、最便宜。
3. 一般认为有效范围为100m，有电流损耗

有千兆传输能力，可以作为主流方案



#### 优点

![image-20221222154849950](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/image-20221222154849950.png)

1. 易于安装且成本较低，线薄，接口小
2. 每米成本低于任何其他类型的局域网布线
3. 较小的外径不能像其他类型的电缆那样迅速地填满布线管道(duct)
4. 使用RJ连接器安装，因此可以大大减少潜在的网络噪声源，并确保良好的可靠连接

#### 缺点

![image-20221222155018737](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/image-20221222155018737.png)

1. 与其他类型的网络媒体相比，UTP只通过双绞的形式屏蔽噪声，更容易产生**电噪声和干扰**
2. 双绞线的信号增强距离比同轴电缆(Coaxial)和光纤(Fiber-Optic)**短**，只有100米

### Coaxial Cable同轴电缆

![image-20221222155256807](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/image-20221222155256807.png)

结构：铜导线传输电信号，塑料绝缘膜，铜金属网防止外部干扰，塑胶套

1. 细缆/粗缆，细缆电阻大，传递距离近，粗缆成本高，传输距离远
2. 与双绞线相比，不使用中继器的网络运行时间更长
3. 比光纤便宜但比双绞线贵

传输距离500m左右，比双绞线传输更加远，成本也要高一点，带宽可以达到百兆-千兆

### Fiber-Optic光缆 

![image-20221222155745491](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/image-20221222155745491.png)

光不受外界电磁波干扰，较为可靠

结构：光导体（二氧化硅），塑料套，一般有两个接口，一个接收一个发送

1. 传导调制(modulated)光传输
2. 不易受到电磁干扰或射频干扰，并且能够比其他网络媒体更高的数据速率
3. 电磁波(electromagnetic wave)通过光纤被引导

成本比较高，传输距离达到几公里，宽带可达千兆

#### 分类

![image-20221222160120535](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/image-20221222160120535.png)

1. 单模式——单路光传输

   光导体直径比较细，相对于多模式光缆要细一个数量级，认为光传输近似直射，没有反射，能量损耗少，多用于广域网

   - 也称为轴(axial)：光沿着电缆的轴传播
   - 由于多模中的色散(dispersion)，比多模(高达10 Gbps)更快
   - 通常用于广域网
   - 直径小于多模(色散较小)
   - 最常使用ILD，但也使用LED

2. 多模式——多路光不同角度传输

   光导体直径大一些，同时传输多路光信号，入射端通过不同入射角区别，接收端按照角度进行识别，实现在一个介质上面实现多路传输，能量损失大一些(反射)，多用于局域网

   - 光以不同的角度进入玻璃管并沿非轴方向传播，这意味着它从玻璃管壁上来回反射
   - 直径大于单光模式，最常用于局域网
   - 易受更大分散性的影响

单模式和多模式在两端都需要用注入式激光二极管或者发光二极管进行发射

### Wireless Communication无线通讯

![image-20221222160715234](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/image-20221222160715234.png)

区分不同电磁波的主要方法是通过其频率(频率多路复用)

把信号编码成为电磁波的方式，不同设备使用不同频段，可以互不干扰

#### 传输方式

![image-20221222160812958](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/image-20221222160812958.png)

1. 激光 Lasers

   约定好电磁波频率范围，使用确定**对射**方案进行传输，部署在中间没有障碍物的两端之间，激光波长很短不能衍射

   - 输出一个相干(coherent)的电磁场，其中所有的波都在同一频率上，并在同一相位上排列

2. 红外线 Infrared

   红外能量要比激光弱得多，成本低，不能衍射，不能跨障碍物传输

   - 通常是一种瞄准线(line-of-sight)技术，但可以反弹(bounced)或重定向
   - 无法通过不透明对象

3. 无线电波 Radio

   可以通过衍射，使得信号在比较远的距离并越过障碍物进行通信

   传输距离比较远，辐射能量小，容易受到干扰，比如雨天能量会损失，在功率较大的设备旁边容易被干扰

   - 携带可以通过墙壁的数据信号
   - 地面(terrestrial)和卫星无线电技术，地面有无线电台，卫星无线电技术有GPS



## UTP for Ethernet 以太网使用的双绞线

### 电缆规格和终端

![image-20221222162120069](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/image-20221222162120069.png)

网络媒体标准由下列团体制定和发布：

- 美国电气与电子工程师学会 制定硬件标准，指定一些新型的协议。
- 保险商实验室
- 电子工业联盟
- 美国电信工业协会
- 美国国家标准协会

![image-20221222162414590](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/image-20221222162414590.png)

解决商业、住宅布线规范，对应路线空间标准、接地接线要求规范

### 无屏蔽双绞线的分类

![image-20221222162554276](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/image-20221222162554276.png)

1. 一类线：主要用于语音传输，**不用于数据传输**，只有两根线做双绞线，常用作电话的语音通信
2. 二类线：传输频率1MHz，用于语音和最高4Mbps的**数据传输**，常见于**令牌网**，不是很常用
3. 三类线：EIA/TIA568标准指定电缆，传输频率16MHz，用于语音传输及最高传输速率为10Mbps的数据传输，主要用于**10BASE-T**，以太网从三类线开始出现
4. 四类线：传输频率为20MHz，用于语音传输和最高传输速率16Mbps的数据传输，主要用于令牌网和**10BASE-T/100BASE-T达到百兆**
5. 五类线：增加了绕线密度，外套高质量绝缘材料，用于语音和数据传输(主要为**100/1000BASE-T达到千兆**)，是最常用的以太网电缆
   - 和三类线相比，增加双绞，绞合度更高，抗干扰能力更强。
   - 从五类线开始进行了更加标准化的处理。
6. 超五类线（**现在主要使用的**）：衰减小，串扰少，具有更高的衰减/串扰比和信噪比、更小的时延误差，主要用于**1000BASE-T**
7. 六类线：传输频率为1MHz～250MHz，性能远高于超五类标准，适用于高于1Gbps的应用
8. 七类线：带宽为600MHz，可能用于今后的10G比特以太网。

1000BASE-T T表示Twisted



### 双绞线类型

1. 直通线
2. 反转线
3. 交叉电缆

**两个台式机直连使用交叉线，台式机和交换机相连使用直通线**

![image-20221222163134180](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/image-20221222163134180.png)

制作网线的过程



#### Straight Cable 直通线

![image-20221222163227840](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/image-20221222163227840.png)

1. 100欧姆平衡双绞线电信插座/连接器
2. 双绞线是八根不同子线，根据颜色进行划分,从左到右(底下):白绿色、绿色、白橙色、蓝色、白蓝色、橙色、白棕色、棕色
3. 左边是T568A端口，右边是T568B端口
4. 直通线——两端都是T568A或者都是T568B

**主机连到交换机，交换机连到路由器**



#### Rollover Cable 反转线

![image-20221222163631529](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/image-20221222163631529.png)

1. 别名：控制台线
2. 用于将工作站或终端连接到路由器/交换机的**控制台端口**以进行配置（别名的由来）
3. **两端是反着连接的** ：一端的插脚1连接到另一端的插脚8；然后插脚2连接到插脚7，插脚3连接到插脚6，依此类推，两端是插脚对应是反着的

![image-20221222195000045](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/image-20221222195000045.png)

使用控制台配置设备(超级终端，使用电脑进行交换机路由器的配置)

- 使用RJ-45-to-DB-9适配器连接计算机的串行端口(com) Connect the serial port (com) of computer by using RJ-45-to-DB-9 adapter
- 启动"超级终端" Start up “super terminal”
- 使用"默认配置" Use “default configurations”
- 注意，我们连接的是console端口，而不能是网口。



#### Crossover Cable 

![image-20221222195226127](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/image-20221222195226127.png)

1. 电缆一端的对2和对3将在另一端反转，**一端为T568-A的排序，另一端为T568-B的排序**【和直通线的主要区别】

2. 被认为是"垂直"布线/主干的一部分

3. 可以用来

   - 连接两个或多个集线器或开关【**堆叠技术**：用交叉线来两个交换机(将两个交换机合成为一个交换机进行使用)或者两个hubs，2个8口交换机，通过一根线连接，则有14个端口】

   - 连接两个独立的工作站以创建小型

4. 主要用来**连接相同的设备**，相同的PC之间的连接

## 介质和信号问题

### 信号和通讯问题

![image-20221222195518621](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/image-20221222195518621.png)

物理层的传播是有损耗的

1. 传播 Propagation【都可以广义地理解为电磁波】
   - 传播时间；速度取决于介质
   - 随着数据传输速率的增加，有时必须考虑信号传输所需的时间。
   - 不同介质传播时间是不同的
2. 衰减 Attenuation
   - 由于**周围环境(surroundings)**造成的远距离信号丢失
   - 会影响网络，因为它限制了可以通过其发送消息的网络布线的长度
   - 在有限长度下进行传输

![image-20221222195857436](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/image-20221222195857436.png)

3. 反射 Reflection

   - 由介质的不连续性引起，我们要保证介质稳定。

   - 发生在电信号中；可能是电缆扭结(kinks)或电缆端接不良的结果

   - 网络应具有特定的阻抗，以匹配NIC中的电气组件
   - 不匹配的阻抗的结果是反射能

![image-20221222200057445](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/image-20221222200057445.png)

4. 噪声(电子干扰) Noise ——都会影响传输的质量

   - 对光/电磁信号的不必要的附加

   - 电缆中其他电线的串扰电噪声（电磁波）

   - EMI(电磁干扰)可由电动机引起 （脉冲）

   - 可以通过扭转线对在网络介质中提供自屏蔽来避免信号的消除。

![image-20221222200306056](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/image-20221222200306056.png)

 5. 时间问题 Timing problem

    - 色散信号在时间上，可以通过适当的电缆设计、限制电缆长度和找到适当的阻抗来固定

    - 抖动源和目标不同步，可通过硬件和软件(包括协议)修复

    - 网络信号延时

### 冲突和冲突域

![image-20221222200722994](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/image-20221222200722994.png)

1. 当两个位元在同一网路上同时传播时，会发生碰撞。
2. 添加**中继器和集线器**会**扩大**冲突域，因为可以连接更多的设备
3. 可以通过添加智能设备(如网桥、交换机和路由器)来分割网络。

### 分割冲突域

![image-20221007093055656](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/image-20221007093055656.png)

网桥、交换机、路由器可以分割冲突域。

到第二第三层(分段后)才能有效划分冲突域，第一层不能解决冲突问题。

划分成不同的段，不是分到了不同的局域网。

## 数据通信的基础信息

### 数据通信的理论基础

#### 基本术语

![image-20221222201323267](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/image-20221222201323267.png)

模拟信号连续

数字信号离散

码元,传输基本单位，并不一定只包含一位，比如我们可以根据波形分为8种，8种区分可以传输三位，2^3^ = 8.

#### 信号处理

![image-20221222201658682](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/image-20221222201658682.png)

可以通过编码和调幅等实现任意复合模拟信号

![image-20221222201826231](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/image-20221222201826231.png)

![image-20221222202038026](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/image-20221222202038026.png)

下面这种码元之间的界限模糊，发生 码间串扰

![image-20221222202136639](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/image-20221222202136639.png)

以上为理想情况

![image-20221222202317380](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/image-20221222202317380.png)

![image-20221222202358541](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/image-20221222202358541.png)

#### 波特率与比特率

![image-20221222202431044](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/image-20221222202431044.png)

波特率和比特率有正比关系

### 数据通信技术

#### 数据通信系统基本结构

![image-20221222202822364](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/image-20221222202822364.png)

源系统-传输系统-目的系统



#### 数据表示和传输方式

![image-20221222203013850](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/image-20221222203013850.png)

#### 信号的传输

![image-20221222203143212](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/image-20221222203143212.png)

![image-20221222203200015](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/image-20221222203200015.png)

1. 线路编码是指将二进制数据转换成可以在物理通信链路上传输的形式，例如电线上的电脉冲、光纤上的光脉冲或空间中的电磁波
2. 在基带传输时数据离散传输，线路编码是有必要的
3. 线路编码作用:在发送和接收双方进行协同操作，避免混淆理解，提高传输效率

#### 数字信号编码

![image-20221222203424958](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/image-20221222203424958.png)

##### 单极性编码

![image-20221222203504591](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/image-20221222203504591.png)



##### 极化编码

![image-20221222203523690](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/image-20221222203523690.png)

![image-20221222203556741](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/image-20221222203556741.png)

![image-20221222203705477](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/image-20221222203705477.png)

编码效率较低

![image-20221222203821717](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/image-20221222203821717.png)

原理：每一位中间都有一个跳变，从低跳到高表示"0"，从高跳到低表示"1"

编码效率大概只有50%

![image-20221222203917553](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/image-20221222203917553.png)

![image-20221222204307004](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/image-20221222204307004.png)

原理：

- 每一位中间跳变：表示时钟
- 每一位位前跳变：表示数据：有跳变表示"0"，无跳变表示"1"

##### 双极性编码

![image-20221222205010405](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/image-20221222205010405.png)

1. 与RZ相同的是: 采用三个电平：正、负与零
2. 与RZ不同的是: 零电平表示"0"，正负电平的跃迁表示 “1”，实现对"1"电平的交替反转。
3. 优点：
   1. 对每次出现的"1"交替反转，使直流分量为0
   2. 尽管连续"0"不能同步，但连续"1"可以同步
4. 这次是1是高点位，下一次就是低电位。

#### 多路复用

![image-20221222205113133](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/image-20221222205113133.png)

![image-20221222205335631](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/image-20221222205335631.png)

##### 时分复用TDM

![image-20221222205433172](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/image-20221222205433172.png)

![image-20221222210107910](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/image-20221222210107910.png)

##### 统计时分复用STDM

![image-20221222210133064](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/image-20221222210133064.png)

每一个子帧要携带用户信息

比较主流的复用方案

##### 频分复用FDM

![image-20221222210337608](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/image-20221222210337608.png)

##### 波分复用WDM

![image-20221222210421450](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/image-20221222210421450.png)

波长分类，八倍复用

##### 码分复用CDM

![image-20221222210430679](https://quasdo.oss-cn-hangzhou.aliyuncs.com/img/image-20221222210430679.png)

采用多个正交的码型