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
- `-e username="ritchie"`: 设置环境变量
- `--env-file=c:/templ /tl .txt`: 从指定文件读入环境变量
- `--expose = 2000-2002`: 开放（暴露）一个端口或一组端口，用于指出容器内可能对外暴露的端如果加上-P则会建立外部端口映射
- `--link my-mysql:server`：添加链接到另一个容器
- `-v c:/templ :/data`: 绑定一个卷
- `--rm`: 退出时自动删除容器


## inspect

查看详细信息

`docker image`和`docker container`都有`inspect`子命令


