 # if-else
- 实现：直接使用逐个项cmp比较
![image.png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/imgae/2023/02/08/046b9e1ca1d0c4174f7568bd732cfec4_20230208112955.png)
![image.png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/imgae/2023/02/08/fcafeb3b0c3bdd07c55b4ed0387719d3_20230208113000.png)


# switch实现
- 与if相比实现更快速跳转
- **空间换时间**

## 实现1:Jump Table
![image.png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/imgae/2023/02/08/126e9357a553a285646f11733a809051_20230208105957.png)
### 汇编实现
![image.png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/imgae/2023/02/08/5e05114181945881a1f521c7322c260b_20230208110019.png)
### 解释
L1:用寄存器eax保存参数vt的值 
L2-L4:vt先减去最小的枚举值（节省跳转表空间1），再与最大跳转值和最小跳转值的差比较，若vt大于此值就返回nullptr
L5:访问跳转表，0x56557034为基址，括号中为偏移量。
	N(,%eax,4)等价于N+%eax * 4
L6:跳转到对应位置执行

### Jump Table
![9c_VoIPSCreencastCoverWnd.png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/imgae/2023/02/08/68bea662609e2435c903d80c442de738_9c_VoIPSCreencastCoverWnd.png)

#### 具体映射关系
![9e_VoIPSCreencastCoverWnd.png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/imgae/2023/02/08/fab74c9f44863e8dcaa72ab77ee02ad2_9e_VoIPSCreencastCoverWnd.png)

![9f_VoIPSCreencastCoverWnd.png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/imgae/2023/02/08/affab5dc0ad0c5533de62039e8646a0c_9f_VoIPSCreencastCoverWnd.png)
注：字符串以\\0结尾


## 实现2：Tree
### rang问题，如果范围很大？
![image.png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/imgae/2023/02/08/1b9f195727cc6df01f69879eab3d6a2a_20230208111854.png)
如果按照之前的方案实现，这样的jump table需要约38MB空间，**代价过大**。

### 新的实现:类似平衡二叉树
![a0_VoIPSCreencastCoverWnd.png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/imgae/2023/02/08/95bbe1cb8460861390da4ecbcba19022_a0_VoIPSCreencastCoverWnd.png)
![a1_VoIPSCreencastCoverWnd.png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/imgae/2023/02/08/86b67f6f58fdfb13c169521ebd31046c_a1_VoIPSCreencastCoverWnd.png)


## 编译器
- 根据case和rang选择实现方式
![a2_VoIPSCreencastCoverWnd.png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/imgae/2023/02/08/c395472327a1f43920718dad5d4e6564_a2_VoIPSCreencastCoverWnd.png)
# 表驱动

**表驱动法**就是一种编程模式(scheme)——从表里面查找信息而不使用逻辑语句(if 和case)

## 应用场景
- 错误/异常处理Error/exception handle
- 消息驱动Message-driven
- 函数指针Function pointer

## 实现
- 数组Array
- Map


