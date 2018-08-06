#### 查询每个segment的物理路径
`select * from pg_filespace_entry`

#### 查询每个segment的物理机ip和port
`select * from gp_segment_configuration`

#### 查询每个表的占用空间
`select pg_size_pretty(pg_relation_size('schema.tname'));`