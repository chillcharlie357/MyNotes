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

## 2.1. 默认构造函数
- 无参数
- **不对变量初始化**
- 当类中未提供构造函数时，编译系统提供
- 可重载，一旦自己写，就不再提供默认构造方法
- 一般是public
	- 但可以定义为private，用于单例，在类的内部创建对象
- 函数签名：`A()`
**全局静态变量会初始化为0**

例子

```cpp
class A
	{   …
	  public:
		A();//默认构造函数
		A(int i);
		A(char *p);
	};
	A   a1=A(1);  <=> A a1(1);  <=> A a1=1; 	//调A(int i)
	A   a2=A(); <=> A a2; 	//调A()，注意：不能写成：A a2();
	A   a3=A(“abcd”); <=> A a3(“abcd”); <=> A a3=“abcd”;  //调A(char *)
	A   a[4]; 	//调用a[0]、a[1]、a[2]、a[3]的A()
	A   b[5]={ A(), A(1), A("abcd"), 2, "xyz“ };
```

## 2.2. 拷贝构造函数
- Copy Constructor
	- 创建对象时，用一同类的对象对其初始化
	- 自动调用
	- 函数签名：`A(cosnt A&a)`

### 2.2.1. 默认拷贝构造函数
- 逐个成员**初始化**(member-wise initialization)
- 对于对象成员，该定义是递归的
- 浅拷贝
```cpp
//创建对象
A a;
A b=a;
//作为函数参数,传参本身就会copy
f(A a){...} <=> A b; f(b);
//函数返回值
A f(){
A a;...
return a;
}
```

- 如果函数传参时不想copy
	- 用引用传参!
```cpp
A(const A&a);
```
### 2.2.2. 深拷贝与浅拷贝
- 主要区别：成员中是否有指向堆内存的引用？
[[类的构造函数#1.拷贝构造函数（Copy Constructor）]]

### 2.2.3. 包含成员对象的类
- 默认拷贝构造函数
	- 调用成员对象的**拷贝构造**函数
- 自定义拷贝构造函数
	- 调用成员对象的**默认构造**函数，无论成员对象是否有拷贝构造函数
	- 原因：**程序员一旦接管，complier就完全不管**（C++设计原则中对程序员的信任）

例子
```cpp
class A
{      int x,y;
    public:
       A() { x = y = 0; }//默认构造函数
       void inc() { x++; y++; }
};
class B
{      int z;
       A a;
   public:
       B() { z = 0; }
       B(const B& b) { z = b.z; }//拷贝构造函数
       void inc() { z++; a.inc(); }
};
...
B b1;//b1.z = 0,b1.a.x=b1.a.y=0，调用成员对象的默认构造函数
b1.inc(); //b1.z=1,b1.a.x=b1.a.y=1;
B b2(b1); //b2.z = b1.z =1,b2.a.x = b2.a.y=0调用成员对象的默认构造函数
```
若`B(const B& b) { z = b.z; }`改为`B(const B& b):a(b.a) { z = b.z; }`则会得到`b2.a.x = 1,b2.a.y=1`，此时使用成员初始化表调用了A的默认拷贝构造函数


## 2.3. 移动构造函数
- `A(A&& a)` 参数为右值引用（临时变量）
- 作用：减少不必要的调用拷贝构造函数，提高内存利用效率
- 正常情况下右值无法绑定到非cosnt对象/左值引用
**具体语法**
```cpp
Object_name(Object_name&& obj) 
   : data{ obj.data }
{
   // Nulling out the pointer to the temporary data
   obj.data = nullptr;//很关键，记得注销临时指针
}
```
[Move Constructors in C++ with Examples - GeeksforGeeks](https://www.geeksforgeeks.org/move-constructors-in-c-with-examples/)

## 2.4. 移动构造与拷贝构造的区别
- The [copy constructors](https://www.geeksforgeeks.org/copy-constructor-in-cpp/) in [C++](https://www.geeksforgeeks.org/c-plus-plus/) work with the **l-value references** and copy *semantics字面上*(copy semantics means copying the actual data of the object to another object rather than making another object to point the already existing object in the **heap**). 
- While **move constructors** work on the **r-value references** and move semantics(move semantics involves pointing to the already existing object in the **memory**).

On declaring the new object and assigning it with the r-value, firstly a temporary object is created, and then that temporary object is used to assign the values to the object. Due to this the copy constructor is called several times and increases the overhead and decreases the computational power of the code. To avoid this overhead and make the code more efficient we use move constructors.

## 2.5. 左值和右值的区别
[l-values references and rvalues references in C++ with Examples - GeeksforGeeks](https://www.geeksforgeeks.org/lvalues-references-and-rvalues-references-in-c-with-examples/)
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

- 必须使用成员初始化表
	-  初始化引用成员
	- const成员
	- 调用基类构造函数，并且带参数
	- 调用成员类构造函数，带参数


# 4. 析构函数
- 析构函数
	- ~<类名>()
	- 对象消亡时，系统自动调用，**释放对象持有的非内存资源**
	- 可以重载


# 5. Const成员
- const成员变量
	- 初始化放在构造函数的**成员初始化表**中进行
	- static const在定义是初始化`static const int x =100;`
- const成员函数
	- const实例只能调用const成员函数
	- 不能改变成员变量的**值**

```cpp
class A
{    int x,y;
 public:
 A(int x1, int y1);
void f();
void show() const;
};
void A::f()
{  x = 1; y = 1; }
void A::show() const
{  cout <<x << y;}

const A a(0,0);
a.f();//error
a.show();
```
函数原来自带一个参数`A * const this`
```cpp
void f( A * const this);
```
函数后加`const`关键字等价于把参数强制类型转换为`const A * const this`
```cpp
void show(const A* const this);
```


## 5.1. However

```cpp
class A
{
    int a;
    int & indirect_int;
public:
     A():indirect_int(*new int){ ... }
    ~A() { delete &indirect_int; }
    void f() const { indirect_int++; }//不会报错
};
```

引用没有改变，变的是外部内存；
若引用指向`a`则即使声明`f`为const成员函数`a`仍然可以改变

`mutable`关键字，调用`this.x`时强制转换为`(A*)this.x`


# 6. 静态成员
- 类刻划了一组具有相同属性的对象，对象是类的实例
- 问题：如何让同一个类的不同对象如何共享变量？
	- 如果把这些共享变量定义为全局变量，则缺乏数据保护
	- 名空间
- 解决方法：静态成员
	- 类相当于名空间

## 6.1. 静态成员变量
- 特性
	- 类对象所共享
	- 唯一拷贝
	- 遵循类访问控制
- 初始化
	- 在类外初始化
```cpp
class A
	{    int   x,y;
	     static int shared;
        .....
	};
	
	int A::shared=0;
    
	A a, b;

```

- 静态成员的使用
	- 通过对象使用`A a;a.f();`,但不受类对象的影响
	- 通过类使用`A::f();`
## 6.2. 静态成员函数
- 特性
	- 只能存取静态成员变量，调用静态成员函数
	- 遵循类访问控制
```cpp
class A
	{    static int shared;
	     int x;
	 public:
	     static void f() { …shared…}
	     void q() { …x…shared…}
	};

```


### 6.2.1. 单例singleton
- private构造函数，无法在类外调用
- Resource Control原则：谁创建，谁归还

```cpp
class  singleton
{	protected:
		 singleton(){}
		 singleton(const singleton &);
	public:
		static singleton * instance() //只能通过static构造实例，实现单例模式
		{   return  m_instance == NULL? //懒初始化
				m_instance = new singleton: m_instance;
		}
		static void destroy()  { delete m_instance; m_instance = NULL; }
	private:
		static singleton * m_instance;
};
singleton * singleton ::m_instance= NULL;

```

# 7. 友元
- 背景
	- 类外部不能直接访问该类私有成员，但可以通过该类的public方法，会降低对private成员的访问效率，缺乏灵活性
- 作用
	- 会降低对private成员的访问效率，缺乏灵活性
	- 数据保护和对数据的存取效率之间的一个折中方案
	- **友元不具备传递性**，不能任意访问基类；必须显示声明
- 分类
	- 友元函数
	- 友元类
	- 友元类成员函数
例子
```cpp
void func() ;  
class B;//这种情况下B不是必须的  
class C{  
void f();  
};  
class A{  
friend void func();//友元函数  
friend class B; //友元类:B中的每一个函数都可以访问A的成员函数  
friend void C::f();//友元类成员函数  
};
```
## 7.1. 分类
### 7.1.1. 友元函数
一个全局函数是一个类的友元，如果在这之前函数没有声明也是可以进行声明友元函数
### 7.1.2. 友元类
1.  一个类是另一个类的友元
2. class关键字可以省略

- `friend class B`
	- 编译器会寻找有没有类B
	- 如果没有则会引入一个B
- `friend B`
	- 省略关键字的时候不会引入B，如果没有B会报错
	- 但是这种形式常用于模板类(T或者typedef的时候来写)
 ### 友元类成员函数
- 完整的类的声明完成之前是不能够被声明的

## 7.2. 例子
### 7.2.1. 例1
```cpp
class Matrix{
 	friend void multiply(Matrix &m, Vector &v, Vector &r);
};
class Vector{
 	friend void multiply(Matrix &m, Vector &v, Vector &r);
};
```
- 上面这段代码可以编译吗？(循环依赖)
	- 不可以编译的，要在前面先声明Vector
	- 使用变量前必须要先声明
	- Matrix里面如果去掉Vector中的引用？会出现内存分配问题(不知道如何拷贝，而引用大小是相同的)
- 解决方案:不完全声明:class vector;在class Matrix前添加前向声明
### 7.2.2. 例2 声明两个类互为友元
```cpp
class A{
    int a;
    public:
        friend class B;
    void show(B &b){
        std::cout << b.b;//这里可以吗？不行，不知道B中有b
    }
}
class B{
    int b;
    public:
        friend class A;
        void show(A &a){
            std::cout << a.a;//这里是可以的
        }
}
void A::show (B &b){//只能在这里面实现
    std::cout << b.b;
}
```
- 互为友元的两个类声明时是否需要**前置声明**
	- 如果A和B不在一个命名空间不能通过编译
	- 如果A和B在一个命名空间的话可以没有前置声明