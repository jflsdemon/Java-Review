### Mysql Mac 安装
- [Mysql](https://dev.mysql.com/downloads/mysql/)一定要下载Community Server，Mac下下载DMG类型安装文件。
- 安装过程中，会生成一个临时密码，这个一定要记住。这个临时密码是之后登陆Mysql时，root用户的密码。（windows下一般root用户初始密码为空）
- Mysql默认mac下的安装路径为
    **/usr/local/mysql/**

- 若没有记住这个临时密码，之后登陆Mysql时，遇到如下问题是，
    > **access denied for user rootlocalhost**
- 需要重设密码，[操作如下](http://blog.csdn.net/u014410695/article/details/50630233)。

    - 这个时候，可以查看右侧通知栏，会有密码保存
    - 重设密码
        1. cd到mysql安装文件夹的bin目录下，关闭正在运行的mysql：mysqladmin -u root -p shutdown
        2. 在命令行输入以下命令后，命令行变成：sh-3.2#
            > sudo su

        3. 在命令行输入以下明令进入安全模式：
            > ./mysqld_safe --skip-grant-tables &
        
        4. 运行结束我们打开mac的系统偏好设置，选择msyql，我们会发现Mysql重新运行
        5. 回到终端点击Command ＋ N 重新打开一个终端输入以下命令，这时候我们不需要密码就能进入mysql
            > mysql -u -root

        6. 首先执行下面命令为了能够修改任意的密码
            > mysql> FLUSH PRIVILEGES;
        
        7. 执行修改密码的SQL语句,这里的qsd19001008可以替换你自己想要修改的密码
            > mysql> SET PASSWORD FOR root@'localhost' = PASSWORD('qsd19001008');

        8. 最后刷新：
            > FLUSH PRIVILEGES;
        
        9. Control＋D推出mysql，然后关闭安全模式数据库，这里要输入你刚才设置数据密码就好啦
            > /usr/local/mysql/bin/mysqladmin -u root -p shutdown

        10. 正常启动mysql数据库，输入刚才设置的密码,就可以正常进入mysql
            > mysql -u root -p