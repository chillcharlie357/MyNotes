# t1
## 命令
- bximage:创建虚拟软盘映像
- nasm: 汇编boot.asm生产操作系统(boot.bin)的二进制代码
- dd: 把boot.bin写入软盘
	- `dd if=boot.bin of=a.img bs=512 count=1 conv=notrunc`
		- if：代表输入文件
		- of：代表输出设备
		- bs：代表一个扇区大小
		- count：代表扇区数
		- conv：代表不作其它处理
- `bochs -f bochsrc` : 启动bochs
	- -f : configfile

## boot.asm
![image-code](assets/image-code.png)


`$`:当前地址
`$$`:当前扇区的起始地址
[x86 - What is the difference between Times and Dup in Assembly Language? ](https://softwareengineering.stackexchange.com/questions/165025/what-is-the-difference-between-times-and-dup-in-assembly-language?newreg=f1626a136f5d4aa098cb03bbb585c112)

## bochsrc
bochs的配置文件
```
megs:32
display_library: sdl2 
floppya: 1_44=a.img, status=inserted
boot: floppy
```

- display_library：Bochs使用的GUI库，一般情况下用sdl2就可以了，如果不支持，尝试x等库
- megs：虚拟机内存大小 (MB)
- floppya：虚拟机外设，软盘为a.img文件
- boot：虚拟机启动方式，从软盘启动

# t2


# t3

[[整数除法]]

使⽤NASM汇编语⾔实现整数除法。 
输⼊：被除数x和除数y ，其中$0\le x,y \le 10^{100}$ 
输出：计算$x \div y$的结果，包括商和余数。若出错则输出错误信息。

nasm -felf32 div.asm && ld  -melf_i386 div.o && ./a.out

## 寄存器



