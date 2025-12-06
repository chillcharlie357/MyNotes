## 第二次实验



## 题目描述



使用 American Fuzzy Lop (http://lcamtuf.coredump.cx/afl/) 工具挖掘 C/C++ 程序 漏洞。完成实验报告。



## 实验过程





### 1. 安装 American Fuzzy Lop



![image-20251130161253268](./hw2.assets/image-20251130161253268.png)

![image-20251130161430025](./hw2.assets/image-20251130161430025.png)

![image-20251130161533793](./hw2.assets/image-20251130161533793.png)





### 2. 编写测试用例



```c
#include <stdio.h>
#include <string.h>

int main() {
    char buf[16];
    char input[256];

    fgets(input, 256, stdin);
    strcpy(buf, input);  // 可能溢出
    printf("Received: %s\n", buf);

    return 0;
}

```



### 3. 编译插桩版本

![image-20251130161839787](./hw2.assets/image-20251130161839787.png)



### 4. 准备测试数据



![image-20251130162117423](./hw2.assets/image-20251130162117423.png)





### 5. 执行afl



- 出现报错，按照提示修改：

![image-20251130162304339](./hw2.assets/image-20251130162304339.png)



![image-20251130162405715](./hw2.assets/image-20251130162405715.png)



- 启动后进入afl ui:

![image-20251130162643973](./hw2.assets/image-20251130162643973.png)





- 查看引起crash的测试用例

![image-20251130162839199](./hw2.assets/image-20251130162839199.png)

