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
`mysql -uroot -p<passwd> < employees.sql`

其中` -t`选项是一个命令行参数，用于在mysql客户端中以表格格式显示查询结果


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