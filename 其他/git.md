
# 配置文件
`~/.ssh/config`没有自己创建
不是`.config`

格式：
```shell
Host github.com  
    IdentityFile ~/.ssh/github_sshkey  
    User git  
    HostName github.com  
    PreferredAuthentications publickey  
  
Host git.nju.edu.cn  
    IdentityFile ~/.ssh/github_sshkey  
    User git  
    HostName git.nju.edu.cn  
    PreferredAuthentications publickey
```
- 如何验证git远程仓库是否添加ssh验证成功？
`ssh -T <git远程仓库>`
如果终端信息不是`permission denied`就说明成功