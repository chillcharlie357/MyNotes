# 1. torch
如何查看torch是否正确调用GPU
```shell
import torch
torch.cuda.is_avaliable()//cuda是否可用
torch.cuda.current_device()//查看cuda调用设备
torch.cuda.device(<设备号>)
torch.cuda.get_device_name(<设备号>)//设备名称
```
![3f1e763dc7fb74379db8b20d5748428.png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/imgae/2023/01/24/c647bb164bccbbf527abadf97563f809_3f1e763dc7fb74379db8b20d5748428.png)

[docker docs](https://docs.docker.com.zh.xy2401.com/config/containers/resource_constraints/)
# 2. Docker
## 2.1. 权限问题Got permission denied while trying to connect to the Docker daemon socket
把普通用户加入docker用户组
```shell
sudo gpasswd -a <username> docker
newgrp docker //以docker群组登录
```
[pytorch docs](https://pytorch.org/docs/stable/index.html)

conda base环境自启动关闭`bash`
`conda config --set auto_activate_base false`

## 2.2. 运行镜像
[Docker run 命令 | 菜鸟教程](https://www.runoob.com/docker/docker-run-command.html)
[Docker Dockerfile | 菜鸟教程](https://www.runoob.com/docker/docker-dockerfile.html)

`docker run [OPTIONS] IMAGE [COMMAND] [ARG...]`

- 如果你希望连接到docker image的终端：`docker run -it <image> /bin/bash`

## 2.3. 向容器中传文件
使用`docker cp`命令
### 2.3.1. 命令参数
`docker cp [OPTIONS] CONTAINER:SRC_PATH Host_DEST_PATH`
`docker cp [OPTIONS] Host_SRC_PATH CONTAINER:DEST_PATH`

### 2.3.2. 具体步骤：
1. 首先`docker ps`查看正在运行容器的`CONTAINER ID`
2. `docker cp <host_src_path> CONTAINER ID:dest_path`

## 2.4. 如何在docker中使用nvidia gpu
安装NVIDIA Container Toolkit
[Installation Guide — NVIDIA Cloud Native Technologies documentation](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/install-guide.html#docker)

Usage
[User Guide — NVIDIA Cloud Native Technologies documentation](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/user-guide.html)

### 2.4.1. 如何验证安装？
`docker run  --runtime=nvidia --gpus all <image:tag>  nvidia-smi`

如果终端返回这样的结果说明成功了:
![](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/Screenshot%20from%202023-03-07%2023-06-39.png)

## 2.5. docker容器端口映射

1. 查看目标容器的`CONTAINER ID`
![](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/Screenshot%20from%202023-03-10%2022-16-11.png)

2. 从目标容器创建新的镜像
![](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/Screenshot%20from%202023-03-10%2022-18-19.png)

![](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/Screenshot%20from%202023-03-10%2022-21-09.png)

- 使用`docker commit`命令
	- 语法：`docker commit [OPTIONS] CONTAINER_ID [REPOSITORY[:TAG]]`

3. 从步骤2的镜像运行(`docker run`)一个容器，并端口映射

- `docker run [OPTIONS] IMAGE [COMMAND] [ARG...]`
	- `-p`:指定端口映射，格式为：主机(宿主)端口:容器端口
	- `-P`:随机端口映射，容器内部端口**随机**映射到主机的端口
	- `-v`：把主机目录映射到容器目录

`docker run -P -d nginx:latest`：使用镜像nginx:latest以后台模式启动一个容器,并将容器的80端口映射到主机随机端口
`docker run -p 80:80 -v /data:/data -d nginx:latest`：使用镜像 nginx:latest，以后台模式启动一个容器,将容器的 80 端口映射到主机的 80 端口,主机的目录 /data 映射到容器的 /data
`docker run -p 127.0.0.1:80:8080/tcp ubuntu bash`：使用镜像nginx:latest以交互模式启动一个容器,在容器内执行/bin/bash命令

[Docker-端口映射 - 简书](https://www.jianshu.com/p/b92d4b845ed6)