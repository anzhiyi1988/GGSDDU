1.通过创建InnodbMonitor表来打开Innodb的monitor功能：

mysql> create table innodb_monitor(a int) engine=innodb;

Query OK, **0** rows affected (**0.07** sec)

 

2.然后通过使用“SHOWINNODBSTATUS”查看细节信息（由于输出内容太多就不在此记录了）；

可能会有读者朋友问为什么要先创建一个叫innodb_monitor的表呢？因为创建该表实际上就是告诉Innodb我们开始要监控他的细节状态了，然后Innodb就会将比较详细的事务以及锁定信息记录进入MySQL的errorlog中，以便我们后面做进一步分析使用。

