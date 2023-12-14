![image.png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/image%2F2023%2F12%2F14%2F18-46-26-eb3906fceb86ed0e9c6f826e521b0ab0-20231214184623-83585b.png)

# docker的三部分

![image.png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/image%2F2023%2F12%2F14%2F18-48-51-551a7d3c33dd450f7f8dc60d1a6ae29c-20231214184848-94a261.png)

# docker子命令

## run



文件：写时复制，不修改就用底层linux的文件，修改复制一份就放到上一层

docker ru n hello-world
- `--rm`: 退出时删除容器
- `--it`: 进入命令行终端
- `-d`: 后台运行容器，并返回容器 ID
- `-i`: 交互模式运行容器，通常与 -t 同时使用
- `-t`: 为容器重新分配一个伪输入终端，通常与 -i 同时使用
- `-p`: 指定（发布）端口映射，格式为：主机（宿主）端口：容器端口
- `-P`: 随机端口映射，容器内部端口随机映射到主机的高端口
- `--name="nginx-lb"`: 为容器指定一个名称
- `-e username="ritchie"`: **设置环境变量**
	- 可以用环境变量给容器初始化/传入特定参数，在微服务中很常用
	- 例：给mysql容器传`MYSQL_ROOT_PASSWORD`
- `--env-file=c:/templ /tl .txt`: 从指定文件读入环境变量
- `--expose = 2000-2002`: 开放（暴露）一个端口或一组端口，用于指出容器内可能对外暴露的端(不一定映射)
	- 如果加上-P则会建立外部端口映射
- `--link my-mysql:server`：添加链接到另一个容器
	- The linking feature is a **legacy feature**. You should always prefer using **Docker network** drivers over linking.
- `-v c:/templ :/data`: 绑定一个卷
	- 左边时本机路径，右边是容器路径
- `--rm`: 退出时自动删除容器

## inspect

查看详细信息

`docker image`和`docker container`都有`inspect`子命令

# 镜像分层👍

- **写时复制 COW**(Copy-On-Write)
	- 需要修改底层文件时才会复制一份到上层，然后进行修改
	- 否则一直共享底层文件
- docker hostory \<image name>  查看镜像的层

![image.png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/image%2F2023%2F12%2F14%2F20-16-50-cef8d6cc91e64be46d5afa44e9e97583-20231214201648-b1c6b2.png)

# 数据的存储方式

- 数据卷 Volume
- bind mounts
- 临时文件系统 tmpfs(temporary file system)


# Vscode中基于docker容器开发

C/C++：底层开发，嵌入式，性能要求高
Java：
Python
JS：前端开发，服务端，基于JS的测试工具

![image.png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/image%2F2023%2F12%2F14%2F19-50-53-30b0928d69d9e80c0c19e469f102cdb6-20231214195051-9c2274.png)

Gadle：并发构建，性能优于maven

