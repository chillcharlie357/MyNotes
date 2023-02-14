![NVIDIA_Share_fV7poo9P5t.png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/imgae/2023/01/14/8466e1250845ae12437ec7dbcb4e294d_NVIDIA_Share_fV7poo9P5t.png)
# 1.函数调用过程
1. 参数压栈
2. 保存调用函数运行状态
	a. 保存返回地址
	b. 保存调用者的base pointer
3. 执行函数
	a. 设置新的base pointer
	b. 分配内存（可选）
	c. 执行任务
	d. 释放内存（若分配）
4. 恢复调用函数运行状态
	a. 加载调用者base pointer
	b. 加载返回地址
5. 继续执行
# 2.参数传递
## 2.1传值(pass by value)
```cpp
int func(int a,int b){
	int r = a + b;
	return r;	
}
int main(){
	int r = 0;
	r = func(1,2);
	return 0;
}
```
## 2.2传引用(pass by reference)
```cpp
void func(int &a){
	a = 1;
}
int main(){
	int r = 0;
	func(r);
	return 0;
}
```
# 3.Runtime Environment
调用者：caller
被调用者：callee
- \_cdecl
	- 调用者还参数
	- 调用者知道参数/有可变参数（被调用者不确定参数个数）
	- 可能增加代码长度
	- 无类型检查
- \_stdcall
	- 被调用者还参数
	- 更通用
- \_fastcall
	- 使用寄存器传前2个参数，其它压栈
	- 2个以下参数更快
- \_thiscall
![1_NVIDIA_GeForce_Overlay_DT.png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/imgae/2023/01/15/68d5623f5c5c4184cfbd1ca604fc1ec8_1_NVIDIA_GeForce_Overlay_DT.png)

# 4.函数原型
- `void f(int,int);`不用函数体，只需要接口
- 遵循先定义后使用的原则
- 自由安排定义函数的位置
- 无需参数类型，无需参数名称
- 编译器检查
# 5.函数重载
不是面向对象特有的
- 原则
	- 名同，参数不同（个数、类型、顺序）
	- 返回值类型不作为区别重载函数的依据（**重载不能只改返回值类型**）
- 匹配原则
	- 严格匹配
	- 类型不存在时，内部转换
	- 用户定义的转换
- 例子
```cpp
void bar(int i) {  
	cout << "bar(1)" << endl;  
}  
void bar(const char c) {  
	cout << "bar(2)" << endl;  
}  
void func(int a) {  
	cout << "func(1)" << endl;  
}  
void func(char c) {  
	cout << "func(2)" << endl;  
}  
void func(long long ll) {  
	cout << "func(3)" << endl;  
}  
void hum(int i, ...) {  
	cout << "hum(1)" << endl;  
}  
void hum(int i, int j) {  
	cout << "hum(2)" << endl;  
}  
int main() {  
	char c = 'A';  
	bar(c);  
	short s=1;  
	func(s);  
	hum(12, 5);  
	hum(10, 12, 1);  
	system("pause");  
}  
//输出结果为  
//bar(2)  
//func(1)  
//hum(2)  
//hum(1)  
  
//下面这种是不被允许的，二义性
void f(long);  
void f(double);  
f(10);
```
# 6.函数默认参数💥
是重载的补充
- 默认参数的声明
	- 在<font color="#ff0000">函数原型中给出</font>，而不是定义时
	- 先定义的函数中给出
- 默认参数顺序
	- <font color="#ff0000">右→左</font> 编译器不检查
	- 不间断
- 默认参数与函数重载
	- 如果一组重载函数都允许相同实参个数的调用可能有**二义性**
```cpp
void f(int);
void f(int,int=1);
void f(int=3,int=4);

f(6);//无法确定
f(5,6);//无法确定
```


```cpp
//默认值的声明
void f(int=3,int=4) //在声明时给出默认值
void f(int x,int y){//定义中不允许再给出默认值
	cout << x << y;
}
//参数顺序
void f(int,int = 2,int = 3);//使用函数原型  
void g(){  
	f(1);//==f(1,2,3)  
	f(1,3);//==f(1,3,3)  
	f(1,5,5);//==f(1,5,5)  
}
void f(int a,int b,int c){}
f()//error:无默认值
f(1,3,5)//全部给出实参
f(1)//b,c默认
f(1,,4)//error:必须从右到左严格匹配
``` 
# 7.Inline
- 目的
	- 提高可读性
	- 提高效率
- 实现
	- 编译系统将为 inline 函数创建一段代码，在调用点，以相应的代码替换
- 限制
	- 递归
	- 函数指针
	- 不能含有复杂的结构控制语句（switch,while）
- 缺点
	- 增大目标代码（object code）
	- 病态的换页（thrashing）
	- 降低指令cache的命中率（instruction cache hit rate）
- 适用
	- 使用频率高、简单、小段代码（1~5行）
	- 例：class的构造器
- 内联函数其实并没有产生函数调用（func_call）
	- ![3_NVIDIA_GeForce_Overlay_DT.png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/imgae/2023/01/15/3d32157c486b2fa1b0d00df3e1781b56_3_NVIDIA_GeForce_Overlay_DT.png)
注意：inline只是向编译器发出**请求**，可能被拒绝。inline被拒绝后的调用过程与常规函数不同。
# 8.ROP
基于return的攻击
解决方法：写/执行正交
