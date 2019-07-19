数据库总是不断地在执行删除，更新等操作。良好的空间管理非常重要，能够对性能带来大幅提高。在postgresql中用于维护数据库磁盘空间的工具是VACUUM，其重要的作用是删除那些已经标示为删除的数据并释放空间。

VACUUM语法结构：

 

VACUUM [ FULL ] [ FREEZE ] [ VERBOSE ] [ table ]

VACUUM [ FULL ] [ FREEZE ] [ VERBOSE ] ANALYZE [ table [ (column [, ...] ) ] ]

 

postgresql中执行delete操作后，表中的记录只是被标示为删除状态，并没有释放空间，在以后的update或insert操作中该部分的空间是不能够被重用的。经过vacuum清理后，空间才能得到释放。可惜的是vacuum工具不能够对相应的索引进行清理，唯一的办法就是手动去重建相应索引(令人非常不爽，而高兴的是在9.0之后有所改进)。

**Full Vacuum**

full vacuum与单纯的vacuum还是有很大的区别的。vacuum只是将删除状态的空间释放掉，转换到能够重新使用的状态，但是对于系统来说该数据块的空闲空间并没有反应到系统的元数据中。类似oracle中高水位标记并没有下降。Full vacuum将会使空间释放的信息表现在系统级别，其实质是将当前删除记录后面的数据进行移动，使得整体的记录连贯起来，降低了“高水位标记”。

**Vacuum analyze**

analyze的功能是更新统计信息，使得优化器能够选择更好的方案执行sql。oracle中同样也有analyze，作用也相同，目前更多的使用的是dbms_stats包。统计信息收集和更新对于系统性能来说非常重要，与oracle维护类似，通常可以通过采用手动或者定制任务的方式。也有不同，oracle在进行imp后自动的对相应数据对象进行统计信息的收集和更新，而postgresql的恢复过程还没有集成到里面，需要手动去执行。

**自动****vacuum****配置**

自动vacuum的执行直接由autovacuum参数值决定，默认值是on。

log_autovacuum_min_duration：默认值为-1,关闭vacuum的日志记录，配置为0表示记录autovacuum的所有log。参数设置为正整数表示对于在此时间内完成的vacuum操作不进行log记录，如果没能完成，则记录超出时间内的log。该参数对于了解对象执行vacuum操作的时间非常有用。

autovacuum_max_workers：最大的autovacuum进程的数量，默认值为3。参数大小的配置主要依据系统当前负载和资源。对于系统负载较重的情况，建议开启少量的进程为好，反之，空闲时间可以采用较大值的方式。

autovacuum_naptime：检查数据库的时间间隔。默认为1分钟。

autovacuum_vacuum_threshold：参数表示执行autovacuum操作之前，对单个表中记录执行DML操作的最少行数。达到该行数时自动激活autovacuum操作。该参数针对数据库中的所有表，还可以通过对单个表配置不同的值来改变相应表的autovacuum操作。默认值是50。

autovacuum_analyze_threshold：激活自动analyze操作的最小行数。默认值50。机制与上面相同。

autovacuum_vacuum_scale_factor：该参数采用百分比的方式设定阀值。默认值为20%，当DML涉及的数据量大于某个表的20%时，自动触发autovacuum操作。同样可以通过对单个表进行阀值设定。

autovacuum_analyze_scale_factor：机制与上面相同，到达阀值是自动激活analyze操作。同样可以通过对单个表进行阀值设定。

autovacuum_freeze_max_age：为防止事务ID的重置，在启用vacuum操作之前，表的pg_class .relfrozenxid字段的最大值，默认为200万。

autovacuum_vacuum_cost_delay：autovacuum进程的时间延迟限制，默认值是20ms。对于单个表同样适用。

autovacuum_vacuum_cost_limit：autovacuum进程的开销延迟限制，默认值是-1，表示不进行开销限制，系统将会直接依据vacuum_cost_limit参数管理vacuum的开销。对于单个表同样适用。 

 

通过了解vacuum，能够更清楚postgresql是如何进行系统维护的，其中最为重要的是索引的维护，因为8.3中还不能对索引同时进行维护，这在实际的使用中需要耗费较大的精力，还好在9.0版本中有所改进(具体情况没有测试)。同时大致了解postgresql进行自动vacuum操作的机制与过程。

 

源文档 <http://www.cnblogs.com/daduxiong/archive/2010/10/11/1847975.html> 

 