# 1. 继承
## 1.1. 继承机制
- 基于目标代码的复用
- 对事物进行分类
- 增量开发

## 1.2. 单继承
- `class A:public B`
- 继承方式
	- public
	- private(不写继承方式时默认)
	- protected
- 注意
	- 父类数据会包含在子类中
	- 重载基类会隐藏基类的服务方式
	- 静态编译时会先匹配名称，子类中有就不会去基类找
	- 类 => 名空间

```cpp
class Student
{       int id; 
    public:
         char nickname[16];
         void set_ID(int x)  { id = x; }
         void SetNickName(char *s) { strcpy(nickname,s);} 
         void showInfo() 
         { cout << nickname << “ : “ << id <<endl; }
};
class Undergraduate_Student : public Student
{       int dept_no; 
    public:
         void setDeptNo(int x) { dept_no = x; }
         void showInfo(int x) 
 { cout << dept_no << “: “<< nickname <<endl; }

	private:
		Student::nickname
		void SetNickName
};

us.showinfo(0)//运行成功
us.showinfo()//失败，子类里有就不回去基类里找，没有无参版本
```

## 1.3. 初始化和析构
**构造析构函数都不能继承**

- 派生类对象的初始化
	- 由基类和派生类共同完成
- 构造函数的**执行次序**
	- 基类的构造函数
	- 派生类对**象成员类**的构造函数
	- 派生类的构造函数
- 析构函数的执行次序
	- 与构造函数相反

### 1.3.1. 基类构造函数的调用
- 缺省执行基类默认构造函数
- 如果要执行基类的非默认构造函数，则必须在**派生类构造函数的成员初始化表**中指出
```cpp
class A
{    int x;
  public:
     A() { x = 0; }
     A(int i) { x = i; }
};
class B: public A
{   int y;
public:
   B() { y = 0; }
   B(int i) { y = i; }
   B(int i, int j):A(i) 
   {   y = j;  }
};

B b1;//执行A::A()和B::B()
B b2(1); //执行A::A()和B::B(int)	   
B b3(0,1); //执行A::A(int)和B::B(int,int)
```

- 拷贝构造时
	- 写了拷贝构造函数，A默认构造函数，B拷贝构造函数。只拷贝了y
	- 不写，A默认拷贝构造函数，B默认拷贝构造
```cpp
B(cosnt B&b){}
B b1;
B b2(b1);
```
## 1.4. 友元和protected
- 继承和组合关系都不能传递友元
- 只能访问自己这个类的protected

```cpp
class Base (
protected :
    int prot_mem; //protected 
} ;
class Sneaky : public Base {
    friend void clobber(Sneaky&);//能访问Sneaky::prot_mem
    friend void clobber(Base&);//不能访问Base::prot_mem
    int j;//默认private
}
Sneaky::void clobber(Sneaky &s) { s.j = s.prot_mem = 0; )//正确：clobber 能访问Sneaky对象的 private和protected成员

Sneaky::void clobber(Base &b) { b.prot_mem = 0; }//错误： clobber 不能访问Base的 protected 成员

```



# 2. 虚函数

## 2.1. 类型相容

- 类，类型
- 类型相容，**赋值相容**

### 2.1.1. 对象赋值可能的问题

#### 2.1.1.1. 直接赋值

- 对象身份发生变化
- 属于派生类的属性不存在

```cpp
class A{
	public:
	int x;
	int y;
}
class B:public A{
	public:
	int b;
}
A a;
B b;
a = b//b扩展出的部分丢失
```

**对象切片：** b扩展出的部分会丢失。
![NVIDIA_Share_315_184.png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/imgae/2023/02/21/b3a44a23fb633dafd9144d85d4773108_NVIDIA_Share_315_184.png)

**原因**：`A a = b`会调用`A`的拷贝构造器。

#### 2.1.1.2.  使用指针/引用赋值

- 对象身份未发生变化

```cpp
B* pb;
A* pa=pb;

B b;
A &a = b;
```

调用对象类型由**声明**类型决定
![NVIDIA_Share_1112_739.png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/imgae/2023/02/21/d87397d886576bf47d2897ab34ce7389_NVIDIA_Share_1112_739.png)


## 2.2. 静态绑定/动态绑定

### 2.2.1. 静态/前期绑定（Early Binding）

- 编译时刻决定
- 依据对象的**静态类型**
- 效率高、灵活性差

### 2.2.2. 动态/后期绑定（Late Binding）

- 运行时刻决定
- 依据对象的**实际类型**（动态），根据实际引用和指向对象的类型
- 灵活性高、效率低

出于**效率**考虑，**默认静态绑定**；动态绑定需要用`virtual`关键字显示指出。

## 2.3. 虚函数实现

- 如**基类**中被定义为虚成员函数，则**派生类**中对其重定义的成员函数均为虚函数

### 2.3.1. 限制

1. 类的成员函数**才可以**是虚函数
2. 静态成员函数**不能**是虚函数——在编译期间就确定
3. 内联成员函数**不能**是虚函数——inline在编译时展开
4. 构造函数**不能**是虚函数——构造函数完成才能正常使用虚函数
5. 析构函数**可以（往往）** 是虚函数

### 2.3.2. 后期绑定的实现

- 对象的内存空间中含有指针，指向其虚函数表
- 函数表加在对象前面
```cpp
class A {     
    int x,y;
public:
    virtual f();
    virtual g();
    h();
};
class B: public A
{      
    int z;
public:
      void f();//重写仍然是virtual
      void h();
};
A a; B b;
A *p; 
```



![MessageCenterUI_Ql6TXxyqbm](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/imgae/2023/02/21/b6eabfab3f28fb95ea8a183327651849_MessageCenterUI_Ql6TXxyqbm.png)
### 非虚接口

- 一般调用对象类型由**声明**类型决定
- 非虚接口
	- 调用非虚函数 => 呈现非虚函数特性
	- 调用虚函数 =>呈现虚函数
```cpp
class A
{    public:
	        A() { f();}
		virtual  void f();
	        void g();
		void h() { f(); g(); }//非虚接口
};
class B: public A
{   public:
	    void f();
	    void g();
};	
…
B b; // A::A()，A::f, B::B(), 
A *p=&b; 
p->f();   //B::f()   
p->g();     //A::g()
p->h();	//A::h,B::f,A::g
```

- 虚函数调用非虚函数
```cpp
class A
{  public:
      virtual void f( ) ;
      void g() ;
};
 
class B: public A
{   public:
		void f() { g(); }
        <=>void f( B* const this) { this->g(); }//虚函数调用非虚函数
        
        void g() ;
};

B b;
A* p = &b;
p->f(); //B::f(),B::g()

```

- 规则
	-  当一个对象调用一个非虚函数时， 与对象的**声明类型**绑定的， 并不是与实际的对象的类型绑定的
	- 在虚函数中调用非虚函数的时候， 调用的非虚函数是与虚函数所在的类是一致的
	- 在非虚函数中调用虚函数的时候， 调用的虚函数是与对象的实际类一致的
### 2.3.3. final,override
```cpp
struct B {
    virtual void f1(int) const ;
    virtual void f2 ();
    void f3 () ;
    virtual void f5 (int) final;

};
struct D: B {
    void f1(int) const override ; //正确： f1与基类中的f1 匹配
    void f2(int) override ;//错误： B没有形如f2(int) 的函数。int f2()？
    void f3 () override ;//错误： f3不是虚函数
    void f4 () override ;//错误： B没有名为f4的函数
    void f5 (int) ;//错误： B已经将f5声明成final
}
```



## 2.4. 纯虚函数和抽象类
- 纯虚函数
	- 声明时在函数原型后面加上 = 0   (not there)
	- 往往只给出函数声明，不给出实现
- 抽象类
	- 至少包含一个纯虚函数
	- 不能用于创建对象
	- 为派生类提供框架，派生类提供抽象基类的所有成员函数 的实现
```cpp
virtual int f()=0;
class AbstractClass 
{      …
   public:
       virtual int f()=0; 
};

```

公有继承is_a
私有继承has_a

- 绝对不要重新定义继承而来的缺省参数值(无效)
![NVIDIA_Share_1247_919.png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/imgae/2023/02/22/9c7e1f28c8b7de30929ef604b22079e2_NVIDIA_Share_1247_919.png)
- 动态查找函数表，参数没地方存
- 默认参数都是静态绑定
- 编译：先确定函数版本，再绑定变量常量

```cpp
class A
{
public:
     virtual void f(int x=0) =0;
};

class B: public A
{
public:
     virtual void f(int x=1) 
     { cout << x << endl;}
};
class C: public A
{
public:
     virtual void f(int x) { cout<< x << endl;}
};


int main(){
    A *p_a;
B b;
p_a = &b; 
p_a->f();

A *p_a1;
C c;
p_a1 = &c; 
p_a1->f();

}
```
output
```cpp
0
0
```


## 虚函数应用场景
- 不鼓励非虚函数
- 最提倡纯虚函数
![NVIDIA_Share_760_641.png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/imgae/2023/02/21/2f89ebac986051767b6da081c47081eb_NVIDIA_Share_760_641.png)

# 多继承

```cpp
class A : public B,private C,...{}
```
- 继承方式及访问控制的规定同单继承，但各自独立
- 派生类拥有所有基类的所有成员

- 基类的声明次序决定：
	- 对基类构造函数/析构函数的调用次序
	- 对基类数据成员的存储安排
- 名冲突解决
	- <基类名>::<基类成员名>


# 虚基类
 

- 如果直接基类有公共的基类，则该公共基类中的成员变量 在多继承的派生类中有多个副本


![NVIDIA_Share_389_239.png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/imgae/2023/02/21/520ca0a7b163bca15f8d4357ab4d63af_520ca0a7b163bca15f8d4357ab4d63af_NVIDIA_Share_389_239.png)

- 类D拥有两个x成员：B::x和C::x
- 虚基类：合并
```cpp
class A;
class B: virtual public A;
class C: public virtual A;//Bc共享一个基类
class D: B, C;
```

**注意**
- 虚基类的构造函数由**最新派生出的类**的构造函数调用 //D先调用A的构造函数，再调用B、C
- 虚基类的构造函数**优先于非虚基类**的构造函数执行