获取到proxy的tar包之后就可以开始进行配置了。

```

//解压文件

tar zxvf mysql-proxy-0.8.5-linux-el6-x86-64bit.tar.gz 

//将解压得到的文件夹移动到某个位置，这个位置需要自己创建

mv mysql-proxy-0.8.5-linux-el6-x86-64bit /usr/local/mysql-proxy 

```

将解压得到的文件复制到对应的位置

```

cd /usr/local/mysql-proxy

mkdir lua #创建脚本存放目录

mkdir logs #创建日志目录

cp share/doc/mysql-proxy/rw-splitting.lua ./lua #复制读写分离配置文件

cp share/doc/mysql-proxy/admin-sql.lua ./lua #复制管理脚本

vi /etc/mysql-proxy.cnf #创建配置文件

```

将以下复制到配置文件中，并按照自己的需要修改

重点注意的是：#后面的文字只是为了读者明白各个参数的意义，在实际的文件中最好删除或者将注释和参数放在不同的行，同时，每个参数后面不能后空格，一个空格都不行（重点！重点！重点！），这关系到最后启动服务的问题，如果留有空格，将启动失败，笔者曾经就踩了这个坑！

```

[mysql-proxy]

user=root #运行mysql-proxy用户

admin-username=root #主从mysql共有的用户

admin-password=123456 #用户的密码

proxy-address=192.168.189.130:4040 #mysql-proxy运行ip和端口，不加端口，默认4040

proxy-read-only-backend-addresses=192.168.189.128 #指定后端从slave读取数据

proxy-backend-addresses=192.168.189.129 #指定后端主master写入数据

proxy-lua-script=/usr/local/mysql-proxy/lua/rw-splitting.lua #指定读写分离配置文件位置

admin-lua-script=/usr/local/mysql-proxy/lua/admin-sql.lua #指定管理脚本

log-file=/usr/local/mysql-proxy/logs/mysql-proxy.log #日志位置

log-level=info #定义log日志级别，由高到低分别有(error|warning|info|message|debug)

daemon=true #以守护进程方式运行

keepalive=true #mysql-proxy崩溃时，尝试重启

```

:wq保存后退出，然后修改文件的权限

```

chmod 660 /etc/mysql-proxy.cnf

```

修改读写分离配置文件

```

# vi /usr/local/mysql-proxy/lua/rw-splitting.lua

if not proxy.global.config.rwsplit then

 proxy.global.config.rwsplit = {

 min_idle_connections = 1, #默认超过4个连接数时，才开始读写分离，改为1

 max_idle_connections = 1, #默认8，改为1

 is_debug = false

 }

end

```

配置一下环境变量，不然将找不到mysql-proxy这个命令

```

# export PATH=$PATH:/usr/local/mysql-proxy/bin/

# echo "PATH=$PATH:/usr/local/mysql-proxy/bin" >> /etc/profile

```

配置完环境变量之后，可以使用该命令来测试一下是否配置成功

```

mysql-proxy --help

```

顺便，这里介绍一下各个参数的意义：

```

--help-all ：获取全部帮助信息；

--proxy-address=host:port ：代理服务监听的地址和端口；

--admin-address=host:port ：管理模块监听的地址和端口；

--proxy-backend-addresses=host:port ：后端mysql服务器的地址和端口；

--proxy-read-only-backend-addresses=host:port ：后端只读mysql服务器的地址和端口；

--proxy-lua-script=file_name ：完成mysql代理功能的Lua脚本；

--daemon ：以守护进程模式启动mysql-proxy；

--keepalive ：在mysql-proxy崩溃时尝试重启之；

--log-file=/path/to/log_file_name ：日志文件名称；

--log-level=level ：日志级别；

--log-use-syslog ：基于syslog记录日志；

--plugins=plugin：在mysql-proxy启动时加载的插件；

--user=user_name ：运行mysql-proxy进程的用户；

--defaults-file=/path/to/conf_file_name ： 默认使用的配置文件路径；其配置段使用[mysql-proxy]标识；

--proxy-skip-profiling ： 禁用profile；

--pid-file=/path/to/pid_file_name ：进程文件名；

```

下面遍开始启动mysql-proxy了

```

# mysql-proxy --defaults-file=/etc/mysql-proxy.cnf #开启服务

# netstat -tupln | grep 4040 #已经启动

# tcp 0 0 192.168.189.130:4040 0.0.0.0:* LISTEN 1264/mysql-proxy

```

如果执行开启服务的命令后，后面没有跟着一堆东西，说明配置成功了