url : https://www.cnblogs.com/zhanzhan/p/7729981.html

---

CentOS install:

```

curl -sS https://downloads.mariadb.com/MariaDB/mariadb_repo_setup | sudo bash

yum -y install mariadb-server

// MariaDB config

systemctl start mariadb

systemctl enable mariadb

mysql_secure_installation

```

Servers:

```

CREATE SERVER vanthink FOREIGN DATA WRAPPER mysql

OPTIONS (

  HOST '172.17.0.200',

  DATABASE 'b_vanthink_core',

  USER 'developer',

  PASSWORD '8B#T&Bel',

  PORT 3306);

```

Table:

```

CREATE TABLE test.spider_example (

  id INT PRIMARY KEY AUTO_INCREMENT,

  name VARCHAR(50)

) ENGINE=Spider

COMMENT='wrapper "mysql", srv "vanthink", table "spider_example”';

```

----

Docker:

```

docker build -t mariadb:1 ./

docker run -d --name mariadb -p 13306:3306 -v /home/zjh/mariadb/data:/var/lib/mysql mariadb:1 /root/run.sh

docker exec -it mariadb /bin/bash

mysql -uroot

//Mysql

use mysql;

GRANT ALL PRIVILEGES ON *.* TO 'superman'@'%' IDENTIFIED BY 'secret' WITH GRANT OPTION;

flush privileges;

source /usr/share/mysql/install_spider.sql

```

Dockerfile

```

FROM centos:7.4.1708

RUN curl -sS https://downloads.mariadb.com/MariaDB/mariadb_repo_setup | bash

#安装mariadb数据库

RUN yum -y install mariadb-server

#让容器支持中文

ENV LC_ALL en_US.UTF-8

#初始化数据库

ADD db_init.sh /root/db_init.sh

RUN chmod 775 /root/db_init.sh

RUN /root/db_init.sh

#导出端口

EXPOSE 3306

#添加启动文件

ADD run.sh /root/run.sh

RUN chmod 775 /root/run.sh

#设置默认启动命令

CMD ["/root/run.sh"]

```

db_init.sh

```

#!/bin/bash

mysql_install_db --user=mysql

sleep 3

mysqld_safe &

```

run.sh

```

#!/bin/bash

mysqld_safe

```