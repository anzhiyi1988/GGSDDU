









# 环境变量

```
20200628-[postgre@localhost ~]$ vi .bash_profile 
20200628-# .bash_profile
20200628-
20200628-# Get the aliases and functions
20200628-if [ -f ~/.bashrc ]; then
20200628-        . ~/.bashrc
20200628-fi
20200628-
20200628-# User specific environment and startup programs
20200628-
20200628-PATH=$PATH:$HOME/.local/bin:$HOME/bin
20200628-
20200628-export PATH
```

# 建立数据库群

```
[postgre@localhost ~]$ initdb -D /home/pgsql/data
The files belonging to this database system will be owned by user "postgre".
This user must also own the server process.

The database cluster will be initialized with locale "zh_CN.UTF-8".
The default database encoding has accordingly been set to "UTF8".
initdb: could not find suitable text search configuration for locale "zh_CN.UTF-8"
The default text search configuration will be set to "simple".

Data page checksums are disabled.

creating directory /home/pgsql/data ... ok
creating subdirectories ... ok
selecting default max_connections ... 100
selecting default shared_buffers ... 128MB
selecting dynamic shared memory implementation ... posix
creating configuration files ... ok
running bootstrap script ... ok
performing post-bootstrap initialization ... ok
syncing data to disk ... ok

WARNING: enabling "trust" authentication for local connections
You can change this by editing pg_hba.conf or using the option -A, or
--auth-local and --auth-host, the next time you run initdb.

Success. You can now start the database server using:

    pg_ctl -D /home/pgsql/data -l logfile start

[postgre@localhost ~]$ 
```

# 修改系统配置

```
[root@localhost home]# vi  /etc/security/limits.conf   
* soft nofile 131072
* hard nofile 131072
* soft nproc 131072
* hard nproc 131072
* soft core unlimited
* hard core unlimited
* soft memlock 50000000
* hard memlock 50000000

[root@localhost home]# vi /etc/sysctl.conf  
kernel.sem = 250 512000 100 2048

```



# 修改数据库参数

```
alter system set listen_addresses = '*';
alter system set max_connections = 2000;
alter system set superuser_reserved_connections = 10;
alter system set shared_buffers = '1024MB';
alter system set wal_buffers = '16MB';
alter system set log_destination = 'csvlog';
alter system set logging_collector = on;
alter system set log_line_prefix = '%m - %a - %u - %d - %p: ';
-- alter system set log_lock_waits = on;
alter system set escape_string_warning = off;
-- alter system set standard_conforming_strings = off;
alter system set unix_socket_permissions = 448;
alter system set temp_buffers = '8MB';
```

# 增加密码

```
[root@localhost ~]# openssl req -new -text -out server.req
[root@localhost ~]# openssl rsa -in privkey.pem  -out server.key
[root@localhost ~]# openssl req -x509 -in server.req -text -key server.key -out server.crt
[root@localhost ~]# chmod -og-rwx server.key
```



# 防火墙例外

```
[root@localhost pgsql]# firewall-cmd --permanent --zone=public --add-port=5432/tcp
success
```



# 启动

```
20200628-[postgre@localhost pgsql]$ pg_ctl -D /home/pgsql/data -l logfile start
20200628-server starting
```

# 