# 1.程序结构
- 逻辑结构
- 物理结构
	- 多个源文件组成
	- main唯一
- 工程文件
	- 外部函数
	- 外部变量
![5_NVIDIA_GeForce_Overlay_DT.png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/imgae/2023/01/15/bfc50085c1f7c4035937a36e3f2157aa_5_NVIDIA_GeForce_Overlay_DT.png)
Complier:把cpp独立翻译成obj
Linker：组装
Lib：一堆obj的组合
# 2.namespace
- 两种形式
	- declaration
	- directive
- 作用
	- 在约束作用域方面代替static
	- 限制在本文件中使用，其他文件加extern也不行
	- 进一步解决了全局变量/函数的名冲突
- **staic和main中全局变量**：函数第一次调用时放入内存，程序结束时归还
## 2.1namespace两种形式
```cpp
namespace L{
	int k;
	void f();
	......
}
//using-declaration
using L::k;
using L::f;
k=0;
f(6);

//using-directive
using namespace L;
k=0;
f(6);
```

## 2.2细节特点
1. 别名:namespace本身名字也会冲突
```cpp
//别名
namespace American_Telephone_and_Telegraph {……}
namespace ATT = American_Telephone_and_Telegraph
```
2. 全局:无命名空间，只有`::`默认为全局变量
```cpp
//全局
int a;
namespace X{
	int a;
	void f(){
		int a=0;
		a++;
		X::a++;
		::a++;//全局变量
	}
}
```
3. 开放:可以多次定义，持续扩展
```cpp
//开放
namespace A{
	int a;
	...
}
namespace A{
	void f();
	...
}
```
4. 可嵌套
```cpp
//可嵌套
namespace L1{
	int a;
	...
	namespace L2{
		void f();
		...
	}
	...
}
L1:L2:f();
using namespace L1;
L2::f();
```
5. 重载
不建议在同一作用域下两次使用using-directive
```cpp
//重载
namespace B{
	void f(int);
	...
}
namespace A{
	void f(char);
	...
}
void f();
using namespace A;
void g(){
	f('1');//A::f()
}
//错误
using namespace B;
...
using namespace A;//B::f被A::f重载
...
void g(){
	f('1');//A::f()
	f(1)//error
}
```
6.向前兼容：新的语言成分不应该对以前的程序的影响
- 优先考虑使用using-declaration
- .h与非.h文件：如果使用stdio需要写`using namespace std`;
	```cpp
//stdio  
namespace std {  
	int printf( const char *, …);  
} 

//stdio.h  
namespace std {  
	int printf( const char *, …);  
}  
using namespace std;//向stdio形式兼容
```

```cpp
//使用stdio.h
#include <stdio.h>  
int main(){  
	printf("hello,world\n");  
}  
//使用stdio
#include <stdio>
using namespace std;
int main(){
	printf("hello,world\n");
}

```

# 3.编译预处理（Complie Pre Process）
- 与作用域、类型、接口等概念格格不入
	- 潜伏于环境
	- 穿透作用域
- 应用方式丰富,很难为其的找到具有更好的结构且高效的替代品
## 3.1 \#include
1. 把另一个源文件嵌入到当前源文件
2. 保证接口的定义在本文件中有效
3. 暴露源文本
## 3.2 \#define
### 3.2.1 建立常量(Symbolic constants)
c:
`#define pi 3.14`
c++ better solution:
`const pi = 3.14`
### 3.2.2 开放式子过程(Open subroutines)
c:
```cpp
#define Max(a,b) (a>b)?a:b
```
c++:
```cpp
inline int Max(int a,int b){
	return (a>b)?a:b; 
}
```
### 3.2.3 Generic subroutines
c++中有template
### 3.2.4 Generic “types”
c++中有template
### 3.2.5 重命名
c++中有namespace
### 3.2.6 字符串连接String concatenation
**保留**
```cpp
#define Conn(x,y) x##y
#define ToString(x) #x
#define ToChar(x) #@x
```
### 3.2.7 特殊目的用法
**保留**
```cpp
#if //如果  
  
#else //不然  
  
#elif //不然如果  
  
#endif //if结束  
  
#ifdef //如果定义了  
  
#ifndef //如果没有定义
```

例子:版本控制，条件编译
```cpp
#ifndef MY_PRINTF_VERSION //如果宏在之前没有被定义  
　 #define MY_PRINTF_VERSION 1 //那么将这个宏定义为1  
  
#endif  
//选择编译，选择不同情况下，使用哪个源代码  
#if MY_PRINTF_VERSION == 1  
void printf(*str) {}  
#elif MY_PRINTF_VERSION == 2  
int printf(char* fmt, char* args, ...){}  
#endif
```
### 3.2.8 General macro processing
**保留**
## 3.3 \#ifdef
1. 版本控制
2. 注释代码
## 3.4 \#pragma
1. 层间控制
2. 编译器选项
![image.png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/imgae/2023/01/15/c4fc5f79511d31544777d3364d4a0589_c4fc5f79511d31544777d3364d4a0589_20230115190718.png)
