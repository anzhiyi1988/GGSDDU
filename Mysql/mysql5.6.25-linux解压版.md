查看本机是否安装mysql

[root@anzhy mysql]# rpm -qa |grep mysql

[root@anzhy mysql]# whereis mysql

[root@anzhy mysql]# which mysql

如果有，删除

[root@anzhy mysql]# rpm -e xxxxx --nodeps

 

建立mysql组和用户

[root@anzhy mysql]# groupadd mysql

[root@anzhy mysql]# useradd mysql -g mysql

[root@anzhy mysql]# passwd mysql

 

复制 xxx.tar.gz 的包到mysql目录中/home/mysql，并解压

[root@anzhy mysql]# tar zxvf xxx.tar.gz

[root@anzhy mysql]# pwd

/home/mysql/mysql

[root@anzhy mysql]# ll

total 160

drwxr-xr-x.  2 mysql mysql   4096 Jul  3 16:00 bin

drwxr-xr-x.  5 mysql mysql   4096 Jul  6 23:12 data

drwxr-xr-x.  2 mysql mysql   4096 Jul  3 16:00 docs

drwxr-xr-x.  3 mysql mysql   4096 Jul  3 16:00 include

-rw-r--r--.  1 mysql mysql 103920 Jul  3 16:00 INSTALL-BINARY

drwxr-xr-x.  3 mysql mysql   4096 Jul  3 16:00 lib

-rw-r--r--.  1 mysql mysql   2729 Jul  3 16:00 LICENSE.mysql

drwxr-xr-x.  4 mysql mysql   4096 Jul  3 16:00 man

-rw-r--r--.  1 mysql mysql    965 Jul  3 17:43 my.cnf

drwxr-xr-x. 10 mysql mysql   4096 Jul  3 16:00 mysql-test

-rw-r--r--.  1 mysql mysql   1449 Jul  3 16:00 README

drwxr-xr-x.  2 mysql mysql   4096 Jul  3 16:00 scripts

drwxr-xr-x. 28 mysql mysql   4096 Jul  3 16:00 share

drwxr-xr-x.  4 mysql mysql   4096 Jul  3 16:00 sql-bench

drwxr-xr-x.  2 mysql mysql   4096 Jul  3 16:00 support-files

[root@anzhy mysql]# 

 

初始化数据目录

[root@anzhy mysql]#scripts/mysql_install_db --user=mysql --basedir=   --datadir=

找到my.cnf文件修改配置

[root@anzhy mysql]# more my.cnf 

\# For advice on how to change settings please see

\# <http://dev.mysql.com/doc/refman/5.6/en/server-configuration-defaults.html>

 

[mysqld]

 

\# Remove leading # and set to the amount of RAM for the most important data

\# cache in MySQL. Start at 70% of total RAM for dedicated server, else 10%.

\# innodb_buffer_pool_size = 128M

 

\# Remove leading # to turn on a very important data integrity option: logging

\# changes to the binary log between backups.

\# log_bin

 

\# These are commonly set, remove the # and set as required.

basedir = /home/mysql/mysql

datadir = /home/mysql/mysql/data

port = 3306

\# server_id = .....

\# socket = .....

 

\# Remove leading # to set options mainly useful for reporting servers.

\# The server defaults are faster for transactions and fast SELECTs.

\# Adjust sizes as needed, experiment to find the optimal values.

\# join_buffer_size = 128M

\# sort_buffer_size = 2M

\# read_rnd_buffer_size = 2M 

 

sql_mode=NO_ENGINE_SUBSTITUTION,STRICT_TRANS_TABLES 

[root@anzhy mysql]# 

启动，必须在mysql安装目录下启动，进入bin启动会报错

[root@anzhy mysql]# ./bin/mysqld_safe --user=mysql

150707 02:12:54 mysqld_safe Logging to '/home/mysql/mysql/data/anzhy.centOS.err'.

150707 02:12:54 mysqld_safe Starting mysqld daemon with databases from /home/mysql/mysql/data

 

修改密码

[mysql@anzhy mysql]$ ./bin/mysql -u root mysql

mysql> select User,Host,Password  from mysql.user;

+------+--------------+----------+

| User | Host         | Password |

+------+--------------+----------+

| root | localhost    |          |

| root | anzhy.centos |          |

| root | 127.0.0.1    |          |

| root | ::1          |          |

|      | localhost    |          |

|      | anzhy.centos |          |

+------+--------------+----------+

6 rows in set (0.00 sec)

 

mysql> update mysql.user set password = password('root') where user='root';

Query OK, 4 rows affected (0.02 sec)

Rows matched: 4  Changed: 4  Warnings: 0

 

mysql> flush privileges;

Query OK, 0 rows affected (0.00 sec)

 

mysql> 

mysql> \q

Bye

[mysql@anzhy mysql]$ 

 

增加局域网访问权限

[mysql@anzhy mysql]$ ./bin/mysql -u root -p

mysql> select user,host,password from mysql.user;

+------+--------------+-------------------------------------------+

| user | host         | password                                  |

+------+--------------+-------------------------------------------+

| root | localhost    | *81F5E21E35407D884A6CD4A731AEBFB6AF209E1B |

| root | anzhy.centos | *81F5E21E35407D884A6CD4A731AEBFB6AF209E1B |

| root | 127.0.0.1    | *81F5E21E35407D884A6CD4A731AEBFB6AF209E1B |

| root | ::1          | *81F5E21E35407D884A6CD4A731AEBFB6AF209E1B |

|      | localhost    |                                           |

|      | anzhy.centos |                                           |

+------+--------------+-------------------------------------------+

6 rows in set (0.00 sec)

 

mysql> grant all privileges on *.* to root@"%" identified by 'root' with grant option;

Query OK, 0 rows affected (0.01 sec)

 

mysql> select user ,host,password from mysql.user;

+------+--------------+-------------------------------------------+

| user | host         | password                                  |

+------+--------------+-------------------------------------------+

| root | localhost    | *81F5E21E35407D884A6CD4A731AEBFB6AF209E1B |

| root | anzhy.centos | *81F5E21E35407D884A6CD4A731AEBFB6AF209E1B |

| root | 127.0.0.1    | *81F5E21E35407D884A6CD4A731AEBFB6AF209E1B |

| root | ::1          | *81F5E21E35407D884A6CD4A731AEBFB6AF209E1B |

|      | localhost    |                                           |

|      | anzhy.centos |                                           |

| root | %            | *81F5E21E35407D884A6CD4A731AEBFB6AF209E1B |

+------+--------------+-------------------------------------------+

7 rows in set (0.00 sec)

 

mysql> 

mysql> flush privileges;

Query OK, 0 rows affected (0.00 sec)

 

mysql> 

mysql> \q

Bye

[mysql@anzhy mysql]$ 

 

增加防火墙例外

[root@anzhy ~]# cat /etc/sysconfig/iptables

\# Firewall configuration written by system-config-firewall

\# Manual customization of this file is not recommended.

*filter

:INPUT ACCEPT [0:0]

:FORWARD ACCEPT [0:0]

:OUTPUT ACCEPT [0:0]

-A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT

-A INPUT -p icmp -j ACCEPT

-A INPUT -i lo -j ACCEPT

-A INPUT -m state --state NEW -m tcp -p tcp --dport 22 -j ACCEPT

-A INPUT -m state --state NEW -m tcp -p tcp --dport 3306 -j ACCEPT

-A INPUT -j REJECT --reject-with icmp-host-prohibited

-A FORWARD -j REJECT --reject-with icmp-host-prohibited

COMMIT

[root@anzhy ~]# 

 

外部可以访问