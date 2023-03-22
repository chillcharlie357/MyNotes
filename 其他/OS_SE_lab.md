# NASM
## Tutorials
[NASM Tutorial](https://nasm-tutorial.akash.website/basic/assembly.html)

[NASM Tutorial lmu](https://cs.lmu.edu/~ray/notes/nasmtutorial/)

[NASM x64 Assembly](https://www.cs.uaf.edu/2017/fall/cs301/reference/x86_64.html)


## 寄存器

![](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/imgae/2023/03/22/d3288ae47b38ead4be94368d401c21af_202303221543341.png)


## 系统调用

[Linux System Call Table for x86 64 · Ryan A. Chapman](https://blog.rchapman.org/posts/Linux_System_Call_Table_for_x86_64/)

[用NASM开发Linux x64汇编程序：Hello World - 简书](https://www.jianshu.com/p/e7430b713186)


## x86指令集
[x86 and amd64 instruction reference](https://www.felixcloutier.com/x86/)

> ld: i386 architecture of input file `div.o' is incompatible with i386:x86-64 output

This error message typically occurs when you are trying to link object files or libraries built for a different architecture than the one you are currently targeting.

In this specific case, it seems that you are trying to link a 32-bit object file `div.o` with a 64-bit output architecture. The `i386` architecture refers to 32-bit Intel processors, while `x86-64` architecture refers to 64-bit Intel processors.

To fix this error, you will need to either recompile the `div.o` object file for the x86-64 architecture, or change the output architecture to i386. If you are using a compiler, you may need to pass the appropriate command-line option to set the output architecture to i386.

If you are unsure about the architecture of your system or the object files, you can use the `file` command to check. For example, running `file div.o` will show you the architecture of the `div.o` object file.

# BOCHS
## 安装
依赖库
[调试操作系统：bochs 使用入门 - 知乎](https://zhuanlan.zhihu.com/p/84984590)

## 调试
[bochs调试方法与指令详解\_guozuofeng的博客-CSDN博客](https://blog.csdn.net/guozuofeng/article/details/102646618)


[Bochs 内存查看 - 字节流](http://www.bytekits.com/bochs/bochs-show-mem-cmd.html#:~:text=Bochs%20%E5%86%85%E5%AD%98%E6%9F%A5%E7%9C%8B%201%20x%20%2Fnuf%20%5Baddr%5D%20%E6%98%BE%E7%A4%BA%E7%BA%BF%E6%80%A7%E5%9C%B0%E5%9D%80%EF%BC%88Linear,Address%EF%BC%89%E7%9A%84%E5%86%85%E5%AE%B9%202%20xp%20%2Fnuf%20%5Baddr%5D%20%E6%98%BE%E7%A4%BA%E7%89%A9%E7%90%86%E5%9C%B0%E5%9D%80%EF%BC%88Physical%20Address%EF%BC%89%E7%9A%84%E5%86%85%E5%AE%B9)

