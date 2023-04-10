如何查看torch是否正确调用GPU
```shell
import torch
torch.cuda.is_available()//cuda是否可用
torch.cuda.current_device()//查看cuda调用设备
torch.cuda.device(<设备号>)
torch.cuda.get_device_name(<设备号>)//设备名称
```
![3f1e763dc7fb74379db8b20d5748428.png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/imgae/2023/01/24/c647bb164bccbbf527abadf97563f809_3f1e763dc7fb74379db8b20d5748428.png)

[How to Check PyTorch CUDA Version Easily - VarHowto](https://varhowto.com/check-pytorch-cuda-version/#:~:text=3%20ways%20to%20check%20CUDA%20version%20for%20PyTorch,NVIDIA%20driver%20you%20have%20installed.%20Simply%20run%20nvidia-smi)

[docker docs](https://docs.docker.com.zh.xy2401.com/config/containers/resource_constraints/)
# Docker
## 权限问题Got permission denied while trying to connect to the Docker daemon socket
把普通用户加入docker用户组
```shell
sudo gpasswd -a <username> docker
newgrp docker //以docker群组登录
```
[pytorch docs](https://pytorch.org/docs/stable/index.html)

conda base环境自启动关闭````bash
conda config --set auto_activate_base false
