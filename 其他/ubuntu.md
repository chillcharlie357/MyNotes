
# 安装软件
## tar.gz包
```bash
tar -zxvf <包的文件名> -C <解压目录>
```

## deb包
`dpkg -i`
`apt install `

<<<<<<< HEAD

grub里可以` exec` 切换`lightdm` 和`gdm3`
=======
## APPImage
文档
[[总结 - AppImage中文文档](https://doc.appimage.cn/docs/wiki/)]
### AppImage应用安放在哪里比较合适？
如果你不想把它们放在`$HOME/Downloads`中，那么 `$HOME/.local/bin 和 $HOME/bin `是很好的选择：

在 CentOS/RHEL 和 Fedora 上：脚本$HOME/.bash_profile在登录时运行，这个脚本将$HOME/.local/bin:$HOME/bin添加到路径中。
在Ubuntu上：脚本$HOME/.profile在登录时运行，这个脚本将PATH="$HOME/bin:$HOME/.local/bin"添加到路径中。
此外，存储任何其他位置也是可以，例如U盘，网络位置或光盘，但是这样AppImage应用的路径不在环境变量中，意味着不能简单地在终端输入应用名来运行，而必须使用完整的路径。


>>>>>>> origin/main
