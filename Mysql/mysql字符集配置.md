# 方法一：最简单直接的方法

修改mysql的配置文件

linux : /etc/my.cnf  或者  /usr/my.cnf  是在不行就用find找 

```properties
[client]
default-character-set = utf8
[mysqld]
character_set_server = utf8
```

# 方法二：使用mysql的命令

```
mysql> SET character_set_client = utf8 ;
mysql> SET character_set_connection = utf8 ;
mysql> SET character_set_database = utf8 ;
mysql> SET character_set_results = utf8 ;
mysql> SET character_set_server = utf8 ;
mysql> SET collation_connection = utf8 ;
mysql> SET collation_database = utf8 ;
mysql> SET collation_server = utf8 ;
```

重启后失效 

查看字符集

```
mysql> SHOW VARIABLES LIKE 'character%'
+--------------------------+---------------------------------+   
| Variable_name | Value |   
+--------------------------+---------------------------------+    
| character_set_client | utf8 |   
| character_set_connection | utf8 |   
| character_set_database | utf8 |  
| character_set_filesystem | binary |   
| character_set_results | utf8 |   
| character_set_server | utf8 |   
| character_set_system | utf8 |   
| character_sets_dir | D:"mysql-5.0.37"share"charsets" |   
+--------------------------+---------------------------------+  
```

