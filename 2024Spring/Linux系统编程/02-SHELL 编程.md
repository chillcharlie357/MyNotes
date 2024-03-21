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
title: 02-SHELL 编程
date:  2024-03-21 10:03
modified:  2024-03-21 11:03
---

# Shell

用户和操作系统之间的接口  
是核外程序

SHELL里的变量的值都是字符串

# Read

# 条件判断

SHELL脚本本身不支持条件判断

- test命令
	- 调用test或一个叫\[的程序，<font color="#c00000">本质是执行一个程序，需要有空格</font>
	- `test expression`
	- `[ expression ]`

- test命令支持的条件测试，**运算符之间都要都空格**
	- 字符串比较
	- 算术比较
	- 与文件有关的条件测试
	- 逻辑操作

## 条件运算符

### 字符串比较

str1 = str2 两个字符串相同则结果为真  
str1 != srt2  
-z str 字符串为空则结果为真  
-n str 字符串不为空则为真

### 算术比较

expr1 -eq expr2  
expr1 -ne expr2  
expr1 -gt expr2  
expr1 -ge expr2  
expr1 -lt expr2  
expr1 -le expr2

### 与文件相关的条件测试

-e file 文件存在  
-d file 文件是目录  
-f file 文件是普通文件  
-s file 文件长度不为零

-r file 文件可读  
-w file 文件可写  
-x file 文件可执行

### 逻辑操作

! expr NOT  
expr1 -a expr2 AND  
expr1 -o expr2 OR

## 条件语句

### if 语句

- if elif后面也要有空格，then else默认单独占一行
- 形式：

```shell
if [ expression ]
then
	statements
elif [ expression ]
then
	statements
elif …
else
	statements
fi
```

- 紧凑形式
	- `;`
	- 同一行上多个命令分隔符

### case语句

- 形式
	- 分支语句结尾是`;;`

```shell
case str in
	str1 | str2) statements;;
	str3 | str4) statements;;
	*) statements;;
esac
```

- 例子：

```shell
#!/bin/sh
echo “Is this morning? Please answer yes or no.”
read answer
case “$answer” in
	yes | y | Yes | YES) echo “Good morning!” ;;
	no | n | No | NO) echo “Good afternoon!” ;;
	*) echo “Sorry, answer not recognized.” ;;
esac
exit 0

```

## 循环语句

语句开始和结束都是do和done

### for语句

- 形式

```shell
for var in list
do
	statements
done
```

适用于对一系列字符串循环处理。

- 例子

```shell
for file in $(ls f*.sh);do
	lpr $file
done
exit 0
```

`$()`相当于反引号，执行里面的命令把结果字符串替换在这里

`

### while语句

- 形式

```shell
while condition
do 
	statements
done
```

- 例子

```shell
quit=n
while [ “$quit” != “y” ]; do
	read menu_choice
	case “$menu_choice” in
		a) do_something;;
		b) do_anotherthing;;
		…
		q|Q) quit=y;;
		*) echo “Sorry, choice not recognized.”;;
	esac
done
```

### until语句

不推荐使用

- 形式
	- 条件为假时执行循环

```shell
until condition
do
	statements
done
```

### select语句

- 形式

```shell
select item in itemlist
do
	statements
done
```

- 作用
	- 直接生成菜单列表

```shell
#!/bin/sh
clear
select item in Continue Finish
do
	case “$item” in
		Continue) ;;
		Finish) break ;;
		*) echo “Wrong choice! Please select again!” ;;
	esac
done
```

break跳出的是select循环

# 命令表和语句块

## 命令表/命令组合

- 分号串联
	- 把多个命令放在同一行
	- command1;command2;...
- 条件组合
	- AND命令表
		- 前面成功了，后面才会继续执行。
		- statement1 && statement2 && statement3 && …
	- OR命令表
		- 前面的失败了，采取执行后面的；前面成功了后面就不执行。只有一个命令会成功执行。
		- statement1 || statement2 || statement3 || …

## 语句块

- 形式

```shell
{
	statement1
	statement2
	...
}
```

## 函数

- 形式

```shell
func()
{
statements
}
```

- 局部变量
	- local关键字
- 默认全局变量
- 函数调用
	- func para1 para2 ...
- 返回值
	- return
- 参数
	- 没有形参
	- 在函数内用$1, $2,...调用

- 例子

```shell
yesno()
{
	msg=“$1”
	def=“$2”
	while true; do
		echo ” ”
		echo “$msg”
		read answer
		if [ -n “$answer” ]; then
			...
		else
			return $def
		fi
	done
}
```

# 杂项命令

1. break: 从for/while/until/select循环退出
2. continue: 跳到下一个循环继续执行
3. exit n: 以退出码”n”退出脚本运行
4. return: 函数返回
5. export: 将变量导出到shell，使之成为shell的环境变量
	- 该进程及其子进程都有效，否则只在脚本有效
6. set: 为shell设置参数变量
7. unset: 从环境中删除变量或函数
8. trap: 指定在收到操作系统信号后执行的动作
9. “:”(冒号命令): 空命令
10. “.”(句点命令)或source: 在当前shell中执行命令

# 捕获命令输出

${}和反引号

```shell
echo “The current directory is $PWD”
echo “The current directory is $(pwd)”
```

# 算术扩展

- $((...))

在Shell脚本里比较慢

```shell
#!/bin/sh
x=0
while [ “$x” –ne 10 ]; do
	echo $x
	x=$(($x+1))
done
exit 0

```

# 参数扩展

![image.png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/image%2F2024%2F03%2F21%2F11-53-56-80673f6b51040b22e1c0a9aa2dd72cc5-20240321115355-3b2b2d.png)

- 例子
	- 批处理1_tmp,2_tmp,...

```shell
#!/bin/sh
i=1
while [ “$i” –ne 10 ]; do
	touch “${i}_tmp”
	i=$(($i+1))
done
exit 0
```


# 即时文档

