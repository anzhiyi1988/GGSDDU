GP 支持几个专业的存储模式和混合存储模式。

 

# Heap storage

Heap table storage works best with OLTP-type workloads where the data is often modified after it is initially loaded

# Append-Optimized Storage

Append-optimized table storage works best with denormalized fact tables in a data warehouse environment. 

Moving large fact tables to an append-optimized storage model eliminates the storage overhead of the per-row update visibility information, saving about 20 bytes per row. 

Single row INSERT statements are not recommended.

UPDATE and DELETE are not allowed on append-optimized tables in a serializable transaction and will cause the transaction to abort. CLUSTER, DECLARE...FOR UPDATE, and triggers are not supported with append-optimized tables.

 

Use the WITH clause of the CREATE TABLE command to declare the table storage options.

CREATE TABLE bar (a int, b text)

WITH (appendonly=true)

DISTRIBUTED BY (a);