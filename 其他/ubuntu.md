
# 安装软件
## tar.gz包

[software installation - How do I install a .tar.gz (or .tar.bz2) file? - Ask Ubuntu](https://askubuntu.com/questions/25961/how-do-i-install-a-tar-gz-or-tar-bz2-file)
```bash
tar -zxvf <包的文件名> -C <解压目录>
```
一般的放到`/opt/`下

- 快捷方式创建
[【包学包会】【Zotero篇】Linux安装、重装和备份，换新电脑再也不用麻烦啦！不同系统的配置文件是通用的，香。 - 知乎](https://zhuanlan.zhihu.com/p/436241013)

## deb包
`dpkg -i`
`apt install `


grub里可以` exec` 切换`lightdm` 和`gdm3`

## APPImage
文档
[[总结 - AppImage中文文档](https://doc.appimage.cn/docs/wiki/)]
### AppImage应用安放在哪里比较合适？
如果你不想把它们放在`$HOME/Downloads`中，那么 `$HOME/.local/bin 和 $HOME/bin `是很好的选择：

在 CentOS/RHEL 和 Fedora 上：脚本$HOME/.bash_profile在登录时运行，这个脚本将$HOME/.local/bin:$HOME/bin添加到路径中。
在Ubuntu上：脚本$HOME/.profile在登录时运行，这个脚本将PATH="$HOME/bin:$HOME/.local/bin"添加到路径中。
此外，存储任何其他位置也是可以，例如U盘，网络位置或光盘，但是这样AppImage应用的路径不在环境变量中，意味着不能简单地在终端输入应用名来运行，而必须使用完整的路径。

# 终端复用
[Tmux 使用教程 - 阮一峰的网络日志](https://www.ruanyifeng.com/blog/2019/10/tmux.html)


# docker
[Docker 教程——理解 Docker 镜像和容器的存储路径](https://www.freecodecamp.org/chinese/news/where-are-docker-images-stored-docker-container-paths-explained/)
[Install Docker Desktop on Linux](https://docs.docker.com/desktop/install/linux-install/)



# root权限
ubuntu默认无法通过su获得root权限，因为没有密码
可以:
`sudo bash` 
`sudo -s`

# vim
- 删除行首的空格？
	- `%s/^\s\+//`
[linux - vim技巧：删除行首、行末的空白字符，删除空白行 - 南木阁 - SegmentFault 思否](https://segmentfault.com/a/1190000021058245)