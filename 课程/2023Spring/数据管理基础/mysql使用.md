---
title: mysql使用
tags: 2023_Spring数据管理基础 课程 
categories: 2023_Spring数据管理基础
date:  2023-03-03 20:47
modified:  星期一 3日 四月 2023 15:31:52
sticky:
thumbnail:
cover: 
excerpt: false
mathjax: true
comment: true
---



# terminal
## 登陆
- 终端输入`mysql -u root -p`  ，然后输入密码即可
- `mysql -uroot -p<passwd>`无空格

## 选择数据引擎

```sql
   set storage_engine = InnoDB; 
-- set storage_engine = MyISAM;
-- set storage_engine = Falcon;
-- set storage_engine = PBXT;
-- set storage_engine = Maria;
```

## 加载数据库
假设数据库为employees.sql
先登陆mysql服务器
再`source employees.sql`

在命令行中导入数据比较快

# vscode中连接mysql
这个感觉比较方便
安装插件：
```
Name: MySQL
Id: formulahendry.vscode-mysql
Description: MySQL management tool
Version: 0.4.1
Publisher: Jun Han
VS Marketplace Link: https://marketplace.visualstudio.com/items?itemName=formulahendry.vscode-mysql
```

```
Name: SQLTools MySQL/MariaDB
Id: mtxr.sqltools-driver-mysql
Description: SQLTools MySQL/MariaDB
Version: 0.5.1
Publisher: Matheus Teixeira
VS Marketplace Link: https://marketplace.visualstudio.com/items?itemName=mtxr.sqltools-driver-mysql
```

```
Name: SQLTools
Id: mtxr.sqltools
Description: Connecting users to many of the most commonly used databases. Welcome to database management done right.
Version: 0.27.1
Publisher: Matheus Teixeira
VS Marketplace Link: https://marketplace.visualstudio.com/items?itemName=mtxr.sqltools
```

```
Name: MySQL Syntax
Id: jakebathman.mysql-syntax
Description: MySQL syntax highlighting support
Version: 1.3.1
Publisher: Jake Bathman
VS Marketplace Link: https://marketplace.visualstudio.com/items?itemName=jakebathman.mysql-syntax
```



反引号 \` 用于标识符，如表名、列名等,当没有歧义时，并且表/列名称没有特殊字符或空格时，可以省略 。
单引号 `´` 用于字符串常量