配置一下，找到谁在什么时间执行了什么sql。

### 1. 设置init-connect

1.1 创建用于存放连接日志的数据库和表

```sql
create database accesslog;
CREATE TABLE accesslog.accesslog (
	`id` int(11) primary key auto_increment,
    `time` timestamp,
    `localname` varchar(30),
    `matchname` varchar(30)
)
```

1.2 创建用户权限
可用现成的root用户用于信息的读取

```sql
grant read on accesslog.* to root@localhost identified by ‘password’;
```

如果存在具有to  \*.\*  权限的用户需要进行限制。

1.3 设置init-connect

在[mysqld]下添加以下设置：

```
init-connect=’insert into accesslog.accesslog values(connection_id(),user(),current_user(),now());’
```

1.4 重启数据库生效

```
shell> service mysqld restart
```



### 2. 记录追踪

2.1 thread_id确认

假设想知道在2009年11月25日，上午9点多的时候，是谁吧test.dummy这个表给删了。可以用以下语句定位

```
mysqlbinlog –start-datetime=’2009-11-25 09:00:00′ –stop-datetime=’2009-11-25 09:00:00′  binlog.xxxx | grep ‘dummy’ -B 5
```

会得到如下结果(可见thread_id为5)：

```
at 300777
#091124 16:54:00 server id 10  end_log_pos 301396       Query   thread_id=5     exec_time=0     error_code=0
SET TIMESTAMP=1259052840;
drop table test.dummy;
```

2.2 用户确认
thread_id 确认以后，找到元凶就只是一条sql语句的问题了。

```
select * from accesslog.accesslog where conn_id=5 ;
```

就能发现是testuser2@localhost干的了。

| id   | time | localname | matchname |
| ---- | ---- | --------- | --------- |
| 5    | 2009-11-25 10:57:39 | testuser2@localhost | testuser2@% |

### 3. Q&A

Q：使用init-connect会影响服务器性能吗？

A：理论上，只会在用户每次连接时往数据库里插入一条记录，不会对数据库产生很大影响。除非连接频率非常高（当然，这个时候需要注意的就是如何进行连接复用和控制，而非是不是要用这种方法的问题了）



Q：access-log表如何维护?

 A: 由于是一个log系统，推荐使用archive存储引擎，有利于数据厄压缩存放。如果数据库连接数量很大的话，建议一定时间做一次数据导出，然后清表。



Q：表有其他用途么？

A：有！access-log表当然不只用于审计，当然也可以用于对于数据库连接的情况进行数据分析，例如每日连接数分布图等等，只有想不到没有做不到。



Q：会有遗漏的记录吗？

A：会的，init-connect 是不会在super用户登录时执行的。所以access-log里不会有数据库超级用户的记录，这也是为什么我们不主张多个超级用户，并且多人使用的原因。 



https://blog.csdn.net/coolpale/article/details/80308834