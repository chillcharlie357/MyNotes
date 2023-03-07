# java环境配置

### 1.安装

查找所需jdk版本:`apt search [*]`

安装jdk:`sudo apt install [*]`

### 2.检查

`java --version`

`javac --version`

### 3.设置环境变量

在许多java应用中`JAVA_HOME`用来表示java安装位置。

在`/etc/profile`中添加

```shell
export JAVA_HOME="/usr/lib/jvm/java-1.x.x-openjdk"
export PATH=$JAVA_HOME/bin:$PATH
```

验证：`echo $JAVA_HOME`

### 4.卸载

`sudo apt remove [*]`

### 5.安装多个版本java

安装同1.

**修改默认版本**：`update-alternatives --config java`（也可以用于查看安装路径）

# C/C++环境配置

## 1.安装`build-essential`包

`sudo apt install build-essential`

## 2.检验

`gcc --verison`

或脚本

```shell
#!/bin/sh
gcc -v
if [ $? != 0 ]; then
       echo "GCC is not installed!"
fi
ld -v
if [ $? != 0 ]; then
        echo "Please install binutils!"
fi
```



## python环境配置

安装`miniconda`或`anaconda`

创建环境：`conda create -n [*]`

激活环境：`conda activate [*]`

# node.js环境

```sh
curl -fsSL https://deb.nodesource.com/setup_19.x | sudo -E bash - &&\
sudo apt-get install -y nodejs
```

# vim配置

## 主题更换(colorscheme)



