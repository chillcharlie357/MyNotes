# 1. 概念
**封装（Encapsulation）**:减少耦合，提高可扩展性可维护性
Cfront：把C++编译成C语言
**结构化编程**：面向过程，一组命令的集合
**OO**：一组对象的集合，互相传递信息


![1676293646.png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/imgae/2023/02/13/982ebc5e9c8dbdd7bed64a0101370468_1676293646.png)

- 分类
	- 面向对象
	- 基于对象
		- 没有继承


- 类
	- 成员函数
	- 成员变量
# 2. 构造函数

- 描述
	- 与类同名、无返回类型
	- 自动调用，但无法直接调用
	- 可以重载

- **默认构造函数**
	- 无参数，不对变量初始化
	- 当类中未提供构造函数时，编译系统提供
		- 一旦自己写，就不再提供默认构造方法
- 一般是public
	- 但可以定义为private，用于单例，在类的内部创建对象
**全局静态变量会初始化为0**



```cpp
class A
	{   …
	  public:
		A();
		A(int i);
		A(char *p);
	};
	A   a1=A(1);  <=> A a1(1);  <=> A a1=1; 	//调A(int i)
	A   a2=A(); <=> A a2; 	//调A()，注意：不能写成：A a2();
	A   a3=A(“abcd”); <=> A a3(“abcd”); <=> A a3=“abcd”;  //调A(char *)
	A   a[4]; 	//调用a[0]、a[1]、a[2]、a[3]的A()
	A   b[5]={ A(), A(1), A("abcd"), 2, "xyz“ };
```



# 3. 成员初始化表
- 作用
	- 构造函数补充
	- 减轻complier负担
## 3.1. 执行顺序
- 先于构造函数体
- 按类数据成员申明次序

```cpp
class A
{
 int x;
 const int y;
 int &z;
public:
	A():y(1),z(x),x(0){x=100;} 
	//顺序x->y->z;
}
```

```cpp
class Cstr
{
char *p;
int size;
public:
	Cstr(int x):size(x),p(new char[size]){}
	//error,先初始化P在初始化szie,但初始化P时时size还未初始化
}
```

![NVIDIA_Share_1038_645.png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/imgae/2023/02/18/af6db0badba9008bd0ae6ebb1193df7c_NVIDIA_Share_1038_645.png)


## 3.2. 应用场景
- 在构造函数中尽量使用成员初始化表取代赋值动作
	- const 成员/reference 成员/对象成员
	- 效率更高，减轻complier负担
- const 成员/reference 成员/对象成员
	- const 成员/reference 成员/对象成员



# 4. 析构函数
- 析构函数
	- ~<类名>()
	- 对象消亡时，系统自动调用，**释放对象持有的非内存资源**
	- 可以重载