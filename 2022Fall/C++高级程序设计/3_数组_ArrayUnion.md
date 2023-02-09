# 1. 数组
- 特征
	- 相同类型
	- 连续存储
	- 面向实现，不做越界检查（C++允许越界）
## 1.1. 一维数组
1. 类型定义：`Type name[number]`
2. 元素个数：`sizeof(a)/sizeof(a[0])`
3. **函数接口**
- 元素个数需要通过参数**显示**给出，不能用`sizeof`得到(此时`sizeof(a)`转变)
- `void f(int a[],int n)`传参时作为表达式的右值类型自动转换成`int * const &a[0]`,实际上只传了**数组的地址**
- 传参后`a`已经无法得知有多少元素
4. 字符串
	- 字符串都以`\0`结尾
```cpp
char s1[]="abc";  
cout << s1;//实际上是{'a','b','c','\0'}  
char s2[]={'a','b','c'};  
cout << s2;//错误
```
## 1.2. 一维数组初始化
1. 整数数组初始化
```cpp
//默认初始化  
int a[5] = {}; //[0, 0, 0, 0, 0]  
//全部初始化为0  
int a[5] = {0}; //[0, 0, 0, 0, 0]  
//c++11新写法  
int a[5]{}; //[0, 0, 0, 0, 0]  
  
//注意，想要整型数组 全部初始化为1的时候不能粗暴的设置为  
int a[5] = {1}; //[1, 0, 0, 0, 0]  
// 因为 数组初始化列表中的元素个数小于指定的数组长度时， 不足的元素以默认值填补。  
//可以分别赋值  
int a[5] = {1,1,1,1,1}; //[1,1,1,1,1]
```
2. 字符串初始化/栈初始化
```cpp
string *str = string[5]; //调用5次默认构造函数  
string *str1 = string[5]{"aaa"}; //数组中的第一个元素调用 string::string(const char *) 进行初始化。后面四个调用 默认构造函数
```
3. **数组的默认初始化**:如果不明确指出初始化列表，那么基本类型**不会被初始化**(全局变量和静态变量除外)，所有内存都是脏数据；
4. **自定义的类型数组**会为每个元素调用默认构造函数进行初始化
# 2. 一维数组与指针
![6_NVIDIA_GeForce_Overlay_DT.png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/imgae/2023/01/16/67afba1e6b5e29e46b2bab7c587b70ec_6_NVIDIA_GeForce_Overlay_DT.png)
```cpp
int a[12];
int *p = &a[0];
int *p = a;//a为int *const
//v1
for(int i=0;i<12;i++){
	a[i]=0;
}
//v2
for(int i=0;i<12;i++){
	*(p+i)=0;//等价于*(p++)=0;
}
//v3
for(int i=0;i<12;i++){
	*(a+i)=0;//*(a++)=0错误，因为a的类型是int *const
}
```
# 3. 多维数组
1. 定义
```cpp
//1
T A[c1][c2];
//2
typedef T T1[c2]
T1 A[c1]
```

2. 存储形式：数组套数组
3. 参数传递
	- 缺省第1维`void f(int a[][2],int n);`
![7_NVIDIA_GeForce_Overlay_DT.png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/imgae/2023/01/16/a350293593aa880f67f76360a13c2c15_7_NVIDIA_GeForce_Overlay_DT.png)

# 4. 动态变量

## 4.1. 申请
- `new <类型名>`
- `new <类型名> [<整形表达式>]`
- new 操作符返回指针

### 4.1.1. new 和 malloc区别
- 语法：new强制类型转换
- 语义：调用构造函数
- 包含有效性判定，异常处理
### 4.1.2. 申请内存出现异常/错误时
三点必须做到一点
- release memory 释放内存
- throw std::bad_alloc 抛出异常交给上层处理
- terminate 终止

## 4.2. 归还
- new->delete
	- delete逐个调用析构函数
- delete[]
	- 归还对象数组，首地址上面的4Byte会记录元素个数
- malloc->free
	- 不调用析构函数
- 例子：类似文件的open close
```cpp
	int *L= new int[8];
	delete[] L;

	int *LL = new int;
	delete LL;
```


### RAII
- Resource Acquisition Is Initialization 资源获取即初始化
- 意义：利用构造函数和析构函数强制保障内存安全
- 例子：封装一个指针，**生命周期结束时强制调用析构函数**

```cpp
class auro_ptr{
	int *_ptr;
public:
	auto_ptr(int *p):_ptr(p){}
	~auto_ptr(){delete _ptr_;}
}
```


### 动态数组
```cpp
int *p = new int[10]; //一维数组
int (*p2)[5] = (int (*)[5])p;//强制类型转换成[2][5]
for (int i=0;i<10;i++) 
	p[i] = i+1;
for (int j=0;j<2;j++) { 
	for (int k=0;k<5;k++) 
		cout << p2[j][k] << " " ; 
	cout << endl
}
```


```cpp
typedef int i5Array [5];
void main() { 
i5Array *p = new i5Array [2]; 
for (int j=0;j<2;j++) 
	for (int k=0;k<5;k++) 
		p[j][k] = (j*5)+(k+1); 
}
```


# Struct
- 特征
	- 赋值同一类型
	- 对齐(alignment)，可在编译选项关闭
	- 参数传递
	- 默认public

## 占用空间
```cpp
stuct B
{
char b; //1 B
int a;// 4 B
short c;// 2 B
}
cout << sizeof(B); //4*3 = 12 对齐
```


# Union
- 特征
	- 共享空间

```cpp
 union A
    {
      char b;
      int a;
      short c;
    };
   cout << sizeof(A); //
```

## 例: matrix

```cpp
union Matrix 
{
struct {
	double _a11,_a12,_a13;
	double _a21,_a22,_a23;
	double _a31,_a32,_a33;
}
double _element[3][3];
}


```


```cpp
int i,j;
for(i=0;i<3;i++){
	for(j=0;j<3;j++){
		_element[i][j] = (i+1)*(j+1);
	}
}
for(i=0;i<3;i++){
	for(j=0;j<3;j++){
		cout<<_element[i][j] << " ";
	}
	cout << endl;
}

```

```cpp
Matrix m;

int i,j;
for(i=0;i<3;i++){
	for(j=0;j<3;j++){
		m._element[i][j] = (i+1)*(j+1);
	}
}
for(i=0;i<3;i++){
	for(j=0;j<3;j++){
		cout<<_m.element[i][j] << " ";
	}
	cout << endl;
}
```

- 同时提供struct和数组**两种访问方式**
![99_VoIPSCreencastCoverWnd.png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/imgae/2023/02/08/3c03e755748f960d85928ed407de6092_3c03e755748f960d85928ed407de6092_99_VoIPSCreencastCoverWnd.png)

## 例：定义数组，存储100个图形(直线、矩形、圆)

![image.png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/imgae/2023/02/08/e785b0c229b365b9310bed6d5bb057ec_e785b0c229b365b9310bed6d5bb057ec_20230208103752.png)
![image.png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/imgae/2023/02/08/80368652242e9c3762b977d64ec9d028_20230208103858.png)
![9a_VoIPSCreencastCoverWnd.png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/imgae/2023/02/08/57027db7b0bc5d541dbea7f5a993780e_9a_VoIPSCreencastCoverWnd.png)
- 不同union中的t都重叠

### 多态性
- 多态不是OO独有的，而是语言特征
![9b_VoIPSCreencastCoverWnd.png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/imgae/2023/02/08/3e05b8ec99e4d39ec59623ec7dd2754f_9b_VoIPSCreencastCoverWnd.png)
