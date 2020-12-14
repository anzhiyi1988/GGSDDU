The MySQL CDC connector allows for reading snapshot data and incremental data from MySQL database.

 This document describes how to setup the MySQL CDC connector to run SQL queries against MySQL databases.

## Dependencies

In order to setup the MySQL CDC connector, the following table provides dependency information for both projects using a build automation tool (such as Maven or SBT) and SQL Client with SQL JAR bundles.

### Maven dependency

```xml
<dependency>
  <groupId>com.alibaba.ververica</groupId>
  <artifactId>flink-connector-mysql-cdc</artifactId>
  <version>1.1.0</version>
</dependency>
```

### SQL Client JAR

Download [flink-sql-connector-mysql-cdc-1.1.0.jar](https://repo1.maven.org/maven2/com/alibaba/ververica/flink-sql-connector-mysql-cdc/1.1.0/flink-sql-connector-mysql-cdc-1.1.0.jar) and put it under `<FLINK_HOME>/lib/`.

## Setup MySQL server

You have to define a MySQL user with appropriate(适当) permissions on all databases that the Debezium MySQL connector monitors.

1. Create the MySQL user:

```
mysql> CREATE USER 'user'@'localhost' IDENTIFIED BY 'password';
```

1. Grant the required permissions to the user:

```
mysql> GRANT SELECT, RELOAD, SHOW DATABASES, REPLICATION SLAVE, REPLICATION CLIENT ON *.* TO 'user' IDENTIFIED BY 'password';
```

1. Finalize the user’s permissions:

```
mysql> FLUSH PRIVILEGES;
```

See more about the [permission explanation](https://debezium.io/documentation/reference/1.2/connectors/mysql.html#_permissions_explained).

## How to create a MySQL CDC table

The MySQL CDC table can be defined as following:

```sql
-- register a MySQL table 'orders' in Flink SQL
CREATE TABLE orders (
  order_id INT,
  order_date TIMESTAMP(0),
  customer_name STRING,
  price DECIMAL(10, 5),
  product_id INT,
  order_status BOOLEAN
) WITH (
  'connector' = 'mysql-cdc',
  'hostname' = 'localhost',
  'port' = '3306',
  'username' = 'root',
  'password' = '123456',
  'database-name' = 'mydb',
  'table-name' = 'orders'
);

-- read snapshot and binlogs from orders table
SELECT * FROM orders;
```

## Connector Options

| Option           | Required | Default | Type    | Description                                                  |
| ---------------- | -------- | ------- | ------- | ------------------------------------------------------------ |
| connector        | required | (none)  | String  | 指定使用什么connector，这里必须是 `'mysql-cdc'`.             |
| hostname         | required | (none)  | String  | MySQL database 所在服务器IP地址或主机名.                     |
| username         | required | (none)  | String  | Name of the MySQL database to use when connecting to the MySQL database server. |
| password         | required | (none)  | String  | Password to use when connecting to the MySQL database server. |
| database-name    | required | (none)  | String  | 监控的MySQL服务的数据库名，支持正则表达式以支持监控多个库。  |
| table-name       | required | (none)  | String  | 监控的表的名称，支持正则表达式以支持监控多个表。             |
| port             | optional | 3306    | Integer | Integer port number of the MySQL database server.            |
| server-id        | optional | (none)  | Integer | A numeric ID of this database client, which must be unique across all currently-running database processes in the MySQL cluster. This connector joins the MySQL database cluster as another server (with this unique ID) so it can read the binlog. By default, a random number is generated between 5400 and 6400, though we recommend setting an explicit value. |
| server-time-zone | optional | UTC     | String  | The session time zone in database server, e.g. "Asia/Shanghai". It controls how the TIMESTAMP type in MYSQL converted to STRING. See more [here](https://debezium.io/documentation/reference/1.2/connectors/mysql.html#_temporal_values). |
| debezium.*       | optional | (none)  | String  | Pass-through Debezium's properties to Debezium Embedded Engine which is used to capture data changes from MySQL server. For example: `'debezium.snapshot.mode' = 'never'`. See more about the [Debezium's MySQL Connector properties](https://debezium.io/documentation/reference/1.2/connectors/mysql.html#mysql-connector-configuration-properties_debezium) |

## Features

### Exactly-Once Processing 精确的一次处理

The MySQL CDC connector is a Flink Source connector which will read database snapshot first and then continues to read binlogs with **exactly-once processing** even failures happen. Please read [How the connector performs database snapshot](https://debezium.io/documentation/reference/1.2/connectors/mysql.html#how-the-mysql-connector-performs-database-snapshots_debezium).

### Single Thread Reading

The MySQL CDC source can't work in parallel reading, because there is only one task can receive binlog events.

> Mysql CDC source不能并行运行，因为只有一个任务接收binlog事件。

### DataStream Source

The MySQL CDC connector can also be a DataStream source. You can create a SourceFunction as the following shows:

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

## FAQ

### How to skip snapshot and only read from binlog?

This can be controlled by the option `debezium.snapshot.mode`, you can set to:

- `never`: Specifies that the connect should never use snapshots and that upon first startup with a logical server name the connector should read from the beginning of the binlog; this should be used with care, as it is only valid when the binlog is guaranteed to contain the entire history of the database.
- `schema_only`: If you don’t need a consistent snapshot of the data but only need them to have the changes since the connector was started, you can use the `schema_only` option, where the connector only snapshots the schemas (not the data).

### How to read a shared database that contains multiple tables, e.g. user_00, user_01, ..., user_99 ?

The `table-name` option supports regular expressions to monitor multiple tables matches the regular expression. So you can set `table-name` to `user_.*` to monitor all the `user_` prefix tables. The same to the `database-name` option. Note that the shared table should be in the same schema.