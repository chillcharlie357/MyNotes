
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


# 切换远程仓库 
`git remote set-url <remote-branch-name> <remote-url>`

查看远程仓库
`git remote`
`git remote -v`


# 取消跟踪文件

[git 取消文件追踪/撤销git commit暂存区文件/.gitignore文件 - 腾讯云开发者社区-腾讯云](https://cloud.tencent.com/developer/article/2220221#:~:text=1.%E5%8F%96%E6%B6%88%E6%96%87%E4%BB%B6%E8%BF%BD%E8%B8%AA%20%E5%AF%B9%E6%9F%90%E4%B8%AA%E6%96%87%E4%BB%B6%E5%8F%96%E6%B6%88%E8%BF%BD%E8%B8%AA%20git%20rm%20-r%20%E2%80%93cached%20a.txt%E3%80%80%2F%2F%E5%88%A0%E9%99%A4a.txt%E7%9A%84%E8%B7%9F%E8%B8%AA%EF%BC%8C%E5%B9%B6%E4%BF%9D%E7%95%99%E5%9C%A8%E6%9C%AC%E5%9C%B0%20git,%E3%80%80%2F%2F%E5%88%A0%E9%99%A4a.txt%E7%9A%84%E8%B7%9F%E8%B8%AA%EF%BC%8C%E5%B9%B6%E4%B8%94%E5%88%A0%E9%99%A4%E6%9C%AC%E5%9C%B0%E6%96%87%E4%BB%B6%20git%20rm%20-r%20-n%20%E2%80%93cached%20%E6%96%87%E4%BB%B6%2F%E7%9B%AE%E5%BD%95%E5%90%8D%20%2F%2F%E5%88%97%E5%87%BA%E9%9C%80%E8%A6%81%E5%8F%96%E6%B6%88%E8%B7%9F%E8%B8%AA%E7%9A%84%E6%96%87%E4%BB%B6%EF%BC%8C%E4%B8%8D%E4%BC%9A%E5%88%A0%E9%99%A4%E6%96%87%E4%BB%B6%EF%BC%9B-r%E8%A1%A8%E7%A4%BA%E9%80%92%E5%BD%92%EF%BC%8C-n%E8%A1%A8%E7%A4%BA%E5%88%97%E5%87%BA%E6%96%87%E4%BB%B6)

