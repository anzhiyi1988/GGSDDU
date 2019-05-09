# 一定要编辑自己的cnf文件

[mysql@dbs mysql-5.7.19]$ more my.cnf 

\# mysqld mysqld_safe 都读取server下的参数配置

[server]

basedir = /home/hadoop/mysql-5.7.19  

datadir = /home/hadoop/mysql-5.7.19/data

socket = /home/hadoop/mysql-5.7.19/data/mysql.sock 

pid_file = /home/hadoop/mysql-5.7.19/data/mysql.pid

log_error = /home/hadoop/mysql-5.7.19/data/log.err

port = 3306

# 初始化数据库

[mysql@dbs mysql-5.7.19]$ ./bin/mysqld --defaults-file=my.cnf --initialize --user=mysql 

# 启动数据库

[mysql@dbs mysql-5.7.19]$ ./bin/mysqld_safe --defaults-file=my.cnf  --user=mysql &      

[mysql@dbs mysql-5.7.19]$ ./bin/mysql --socket=/home/mysql/mysql-5.7.19/data/mysql.sock   -uroot -p

