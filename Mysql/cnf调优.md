```properties
# For advice on how to change settings please see
# http://dev.mysql.com/doc/refman/5.6/en/server-configuration-defaults.html

[mysqld]

# Remove leading 
# and set to the amount of RAM for the most important data
# cache in MySQL. Start at 70% of total RAM for dedicated server, else 10%.
# innodb_buffer_pool_size = 128M

# Remove leading # to turn on a very important data integrity option: logging
# changes to the binary log between backups.
# log_bin

# These are commonly set, remove the # and set as required.
# basedir = .....
# datadir = .....
# port = .....
# server_id = .....
# socket = .....

#tmpdir=/opt/mysql/temp

#Global
max_connections=1000
table_open_cache=2048
query_cache_size=1024M（数据库缓存很大）

#Global ,Session
tmp_table_size=4G(很夸张啊，默认16M)
max_heap_table_size=4G(很夸张啊，默认16M)
sort_buffer_size=512M（单个线程的，设置过大，默认是：2M）
join_buffer_size=512M（单个线程的，设置过大，默认是：1M）
query_cache_type=1（用到查询缓存）


# ***** innodb options *****
# globalinnodb_buffer_pool_size = 2G（最关键参数，越大越好）
innodb_additional_mem_pool_size = 16M（相应的增加到：128M）
innodb_buffer_pool_instances=16
innodb_thread_concurrency = 40
innodb_write_io_threads = 16
innodb_read_io_threads = 8
innodb_flush_log_at_trx_commit = 2（可以调到0，mysql如果崩溃的话，丢失最后一秒的事务）
innodb_log_buffer_size = 512M（不应该这么大，默认是1k，肯定要比innodb_log_file_size小，并且是倍数关系比较好，例如：100M）
innodb_log_file_size = 300M
innodb_log_files_in_group = 3
innodb_max_dirty_pages_pct = 30
innodb_io_capacity = 200
innodb_concurrency_tickets = 1000
innodb_autoextend_increment=512
innodb_lock_wait_timeout = 500
# global session


innodb_file_per_table = 1（增加：单独表空间模式）
thread_cache_size=100(增加：数据库连接缓存)



# Remove leading # to set options mainly useful for reporting servers.
# The server defaults are faster for transactions and fast SELECTs.
# Adjust sizes as needed, experiment to find the optimal values.
# join_buffer_size = 128M
# sort_buffer_size = 2M
# read_rnd_buffer_size = 2M
lower_case_table_names=1
sql_mode=NO_ENGINE_SUBSTITUTION,STRICT_TRANS_TABLES 

```

