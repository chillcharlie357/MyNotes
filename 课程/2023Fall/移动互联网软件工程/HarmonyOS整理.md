---
aliases: 
tags: 
categories:
sticky:
thumbnail:
cover: 
excerpt: false
mathjax: true
comment: true
title: HarmonyOS整理
date:  2024-01-09 18:01
modified:  2024-01-09 20:01
---

# 数据管理

## 分类

![image.png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/image%2F2024%2F01%2F09%2F18-48-42-7cbaa4af3a5b58f05cfd992630b79bb6-20240109184841-c3871c.png)

## 用户首选项

用户首选项开发时，方法preferences.flush()作用：数据持久化

## 键值型数据库

设备协同数据库，针对每条记录，Key的⻓度≤896 Byte，Value的⻓度<4 MB。  
单版本数据库，针对每条记录，Key的⻓度≤1 KB，Value的⻓度<4 MB。

单个个应⽤程序最多⽀持同时打开16个键值型分布式数据库。

## 关系型数据库

关系型数据库没有显式的flush操作

## 数据安全

根据设备安全能力，将**设备安全等级**分为SL1、SL2、SL3、SL4、SL5五个等级。  
⼿表通常为低安全的SL1设备，⼿机、平板通常为⾼ 安全的SL4设备  
在设备组⽹时可以通过"hidumper -s 3511"查看设备安全等级。

**风险等级**：  
![image.png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/image%2F2024%2F01%2F09%2F19-04-18-29b240c7bfa7cf820c29c3a9f8d968b4-20240109190418-df20f0.png)

# 端云一体化

[HarmonyOS端云一体化开发简介](https://developer.huawei.com/consumer/cn/doc/harmonyos-guides-V2/agc-harmonyos-clouddev-overview-0000001489475661-V2)

- 相比于传统开发模式，云开发模式的优势有哪些：
	1. 开发成本低
	2. 开发门槛低
	3. 开发效率高

云开发应用发布时**不需要**进行单独的端侧与云侧发布。

- 云函数：在云端运行的函数
	1. 主要用于实现后端业务逻辑
	2. 可以调用第三方服务
	3. 可以部署在华为云平台上

## 文件管理

- 在文件管理模块中，按文件所有者的不同，可分为
	1. 应用文件
	2. 用户文件
	3. 系统文件

- 系统文件及其目录对于应用是只读的。

- **应用沙箱目录**只能看到自己的应用文件和必要的系统文件，限制应用可见的最小数据范围。

- 应用文件目录的一级目录：`data/`  
- 设备开机后即可访问的数据区：`el1/`。1. 应用文件目录的三级目录el1/、el2/分别代表不同文件加密类型，其中el1是设备级加密区，设备开机后即可访问的数据区。

- base目录是应用在本设备上存放持久化数据的目录，子目录包含files/、cache/、temp/和haps/；随应用卸载而清理。

- 文件URI的格式: `file://<bundleName>/<path>`

- 文件选择器（FilePicker）分别提供以下接口
	1. PhotoViewPicker适用于**图片或视频类**文件的选择与保存
	2. DocumentViewPicker适用于文档类文件的选择与保存
	3. AudioViewPicker适用于音频类文件的选择与保存

- BundleStats属性包括
	1. appSize
	2. cacheSize
	3. dataSize

- 分布式文件系统的数据等级默认为S3，应用可以主动设置文件的安全等级。

- hmdfs包括缓存管理、文件访问、元数据管理和冲突管理等功能

- 应用文件包
	1. 应用安装文件
	2. 应用资源文件
	3. 应用缓存文件

- 设备上应用所使用及存储的数据，以**文件、键值对、数据库**等形式保存在一个应用专属的目录内

# 传感器

- 不属于传感器六大类：声音类传感器
- 六大类：运动类传感器、环境类传感器、方向类传感器、光线类传感器、健康类传感器、其他类传感器（如霍尔传感器），每一大类传感器包含不同类型的传感器，某种类型的传感器可能是单一的物理传感器，也可能是由多个物理传感器复合而成。

传感器权限列表

|传感器|权限名|敏感级别|权限描述|
|---|---|---|---|
|加速度传感器、加速度未校准传感器、线性加速度传感器|ohos.permission.ACCELEROMETER|system_grant|允许订阅Motion组对应的加速度传感器的数据|
|陀螺仪传感器、陀螺仪未校准传感器|ohos.permission.GYROSCOPE|system_grant|允许订阅Motion组对应的陀螺仪传感器的数据|
|计步器|ohos.permission.ACTIVITY_MOTION|user_grant|允许订阅运动状态|
|心率|ohos.permission.READ_HEALTH_DATA|user_grant|
