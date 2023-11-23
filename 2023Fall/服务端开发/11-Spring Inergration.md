做不同系统集成

# 集成流 Intergration FLow

![image.png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/image%2F2023%2F11%2F23%2F19-00-32-18ab4ec2735aac2ccea5b42e91078cdf-20231123190031-d3597c.png)

gateway：流的入口
transformer：消息处理、转换

outbound adapter：用于输出
inbound adapter：用于输入
## Gateway


只需要定义接口，类似JPA

定义数据从哪来
## Transformer

`@Transformer`注解


## Adapter

把message放到另外一个系统里去，如输出文件到文件系统


# 集成流配置


1. XML配置
2. Java配置
3. 使用DSL的Java配置

类似依赖注入

## XML

1. 定义一个GateWay接口：获取消息数据
2. 定义一个集成流xml：定义有哪些Channel、Transformer


## Java



![image.png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/image%2F2023%2F11%2F23%2F19-37-20-5a24358a2e8ff6ef8db47e3bc36c8989-20231123193719-f55088.png)


## DSL

IntergrationFlow对象

![image.png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/image%2F2023%2F11%2F23%2F19-40-43-b0218d8cb35748ff7d09c2f2e8d81d41-20231123194042-b1beb5.png)


# 集成流组件

Channels：消息通道
Filters：基于条件判断要不要在流上继续流下去
Transformers
Routers：决定消息要放到接下来的哪个管道
Splitters：把单个消息切分成多个消息
Aggregators：多个消息聚合
Service activators：服务激活器，处理消息的方法
Channel adapters：外部系统的边界
Gateways：构建消息放到集成流上


## Channels

**DirectChannel：默认**
PublishSubscribeChannel：1对多，1个发布多个订阅
QueueChannel：FIFO
PriorityChannel：优先级队列，不按照FIFO出队
RendezvousChannel

![image.png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/image%2F2023%2F11%2F23%2F19-52-20-550033b89ec4a7f0e1abf90cf2d6decf-20231123195220-2e4b63.png)


## Filters

在方法上加`@Filter`注解，返回`boolean`决定消息要不要往下走

![image.png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/image%2F2023%2F11%2F23%2F19-53-49-31d3375a6337ac01c8edbbf4f9089194-20231123195348-52eb90.png)


## Transformers

方法加`@Transformer`注解
通过类型参数指定source type和to type
返回转换逻辑

![image.png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/image%2F2023%2F11%2F23%2F19-55-28-8a87e0604e93c520bf876d1a6a585d58-20231123195527-beae6b.png)

## Routers

`@Router`注解


![image.png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/image%2F2023%2F11%2F23%2F19-56-06-37f0c1b140d7b9dd709f1b71cc8c2a80-20231123195605-a9b7af.png)


## Splitters

切分消息

![image.png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/image%2F2023%2F11%2F23%2F19-59-15-e9e403edfab61e437342fa71836a8113-20231123195914-c81166.png)

