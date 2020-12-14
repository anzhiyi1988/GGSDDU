CDC Connectors 是Flink的一组连接器，是使用CDC（数据更新捕获）来获取不同数据库中的数据改变的。CDC Connector集成了Debezium用来做捕获数据跟新的引擎，所以他可以充分的利用Debezium，查看 [Debezium](https://github.com/debezium/debezium)更多信息。

本节简单介绍CDC的核心功能，详情请看[Documentation](https://github.com/ververica/flink-cdc-connectors/wiki)。

## Supported Connectors

| Database   | Version                                        |
| ---------- | ---------------------------------------------- |
| MySQL      | Database: 5.7, 8.0.x JDBC Driver: 8.0.16       |
| PostgreSQL | Database: 9.6, 10, 11, 12 JDBC Driver: 42.2.12 |

| Format                                                       | Supported Connector                                          | Flink Version |
| :----------------------------------------------------------- | ------------------------------------------------------------ | ------------- |
| [Changelog Json](https://github.com/ververica/flink-cdc-connectors/wiki/Changelog-JSON-Format) | [Apache Kafka](https://ci.apache.org/projects/flink/flink-docs-release-1.11/dev/table/connectors/kafka.html) | 1.11+         |

## 特点 Features

1. Supports reading database snapshot and continues to read binlogs with **exactly-once processing** even failures happen.

2. 如果在DataStream API中使用CDC，用户在一个job中就可以获取多个数据库和表上的数据更新，并且不用部署 Debezium 和 Kafka。

3. 如果在Table/SQL API中使用CDC，用户可以使用SQL DDL创建CDC source，用以监控一个表的更新。

## Downloads

   You can download the releases connector jars from [here](https://github.com/ververica/flink-cdc-connectors/wiki/Downloads).

## 在Table/SQL API中使用

如果要用提供的connector设置flink集群需要如下步骤：

1. Setup a Flink cluster with version 1.11+ and Java 8+ installed.
2. Download the connector SQL jars from the [Download](https://github.com/ververica/flink-cdc-connectors/wiki/Downloads) page (or [build yourself](https://github.com/ververica/flink-cdc-connectors#building-from-source).
3. Put the downloaded jars under `FLINK_HOME/lib/`.
4. Restart the Flink cluster.

以下示例说明在 [Flink SQL Client](https://ci.apache.org/projects/flink/flink-docs-release-1.11/dev/table/sqlClient.html) 中如何创建mysql CDC，并执行查询：

```sql
-- creates a mysql cdc table source
CREATE TABLE mysql_binlog (
 id INT NOT NULL,
 name STRING,
 description STRING,
 weight DECIMAL(10,3)
) WITH (
 'connector' = 'mysql-cdc',
 'hostname' = 'localhost',
 'port' = '3306',
 'username' = 'flinkuser',
 'password' = 'flinkpw',
 'database-name' = 'inventory',
 'table-name' = 'products'
);

-- read snapshot and binlog data from mysql, and do some transformation, and show on the client
SELECT id, UPPER(name), description, weight FROM mysql_binlog;
```

## 在DataStream API中使用

需要加入如下依赖：

```xml
<dependency>
  <groupId>com.alibaba.ververica</groupId>
  <!-- add the dependency matching your database -->
  <artifactId>flink-connector-mysql-cdc</artifactId>
  <version>1.0.0</version>
</dependency>
```



```java
import org.apache.flink.streaming.api.environment.StreamExecutionEnvironment;
import org.apache.flink.streaming.api.functions.source.SourceFunction;
import com.alibaba.ververica.cdc.debezium.StringDebeziumDeserializationSchema;
import com.alibaba.ververica.cdc.connectors.mysql.MySQLSource;

public class MySqlBinlogSourceExample {
  public static void main(String[] args) throws Exception {
    SourceFunction<String> sourceFunction = MySQLSource.<String>builder()
      .hostname("localhost")
      .port(3306)
      .databaseList("inventory") // monitor all tables under inventory database
      .username("flinkuser")
      .password("flinkpw")
      .deserializer(new StringDebeziumDeserializationSchema()) // converts SourceRecord to String
      .build();

    StreamExecutionEnvironment env = StreamExecutionEnvironment.getExecutionEnvironment();

    env
      .addSource(sourceFunction)
      .print().setParallelism(1); // use parallelism 1 for sink to keep message ordering

    env.execute();
  }
}
```

## 