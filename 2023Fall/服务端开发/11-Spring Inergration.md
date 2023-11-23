做不同系统集成

# 集成流 Intergration FLow

![image.png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/image%2F2023%2F11%2F23%2F19-00-32-18ab4ec2735aac2ccea5b42e91078cdf-20231123190031-d3597c.png)

gateway：流的入口
transformer：消息处理、转换

## Gateway


只需要定义接口，类似JPA

定义数据从哪来
## Transformer

`@Transformer`注解


## Adapter

把message放到另外一个系统里去


# 集成流配置


1. XML配置
2. Java配置
3. 使用DSL的Java配置

类似依赖注入

## XML

1. 定义一个GateWay接口：获取消息数据
2. 定义一个集成流xml：定义有哪些Channel、Transformer


