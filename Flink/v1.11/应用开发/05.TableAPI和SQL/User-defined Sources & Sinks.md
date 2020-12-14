Dynamic tables是flink的Table & SQL API中的核心概念，他统一的方式处理有界和无界数据。

因为dynamic tables是一个逻辑概念，flink本身并不拥有数据，dynamic table的内容被存储在外部系统中 (如数据库、键值存储、消息队列) 或文件中。

*Dynamic sources* 和 *dynamic sinks* 用于从外部系统读写数据，本文中，sources and sinks 在术语 *connector*中有所概述。

Flink 为 Kafka, Hive, and different file systems提供了预定义的connectors，可以通过 [connector section](https://ci.apache.org/projects/flink/flink-docs-release-1.11/zh/dev/table/connectors/) 章节获取更多 built-in table sources and sinks的信息

本节重点聚焦在如何开发定制用户自定义的connector。

**Attention** 新的table source 和 table sink 接口已经引入在 Flink 1.11 的 [FLIP-95](https://cwiki.apache.org/confluence/display/FLINK/FLIP-95%3A+New+TableSource+and+TableSink+interfaces)中， 而且 factory interfaces已经重新设计过。FLIP-95 is not fully implemented yet. Many ability interfaces are not supported yet (e.g. for filter or partition push down). If necessary, please also have a look at the [old table sources and sinks page](https://ci.apache.org/projects/flink/flink-docs-release-1.11/zh/dev/table/legacySourceSinks.html). Those interfaces are still supported for backwards compatibility.

# 概述

许多情况下，实现者不需要从头开始创建一个新的connector，而是希望 可以轻微的修改已存在的connectors或是 hook into the existing stack。其他情况，实现者希望创建特殊的 connectors。

本节对这两种用例都有帮助。它解释了table connectors的一般架构，从API中的纯声明到将在集群上执行的运行时代码。

箭头说明了从一个阶段的对象到另一个阶段的对象的转换过程。

![Translation of table connectors](../../../image/table_connectors.svg)

## Metadata

Table API 和 SQL 是声明式AIPs，他们包含表的声明，因此执行 `CREATE TABLE` 语句会更新目标catalog的元数据。

对于多数的 catalog实现， 外部系统中的物理数据不会针对此类操作进行修改。Connector-specific的依赖项也不必出现在类路径中。WITH子句中声明的选项既不会经过验证也不会被解释。

neither...nor...  既不......也不

dynamic tables的元数据（通过DDL创建或由目录提供）被认为是CatalogTable的实例。必要时，表名将在内部解析为CatalogTable。

## planning

在进入 planning和 optimization阶段时， `CatalogTable` 被解析为 `DynamicTableSource` 和`DynamicTableSink` ，`DynamicTableSource` 用于select查询，`DynamicTableSink` 用于insert写入。

`DynamicTableSourceFactory` 和`DynamicTableSinkFactory` 提供connector-specific的逻辑处理，他们把`CatalogTable` 的元数据翻译成 `DynamicTableSource` 和`DynamicTableSink`示例。大多数情况，factory的目的是验证选择、配置编码解码的格式、并创建table connector的参数化实例。

默认情况， `DynamicTableSourceFactory` 和 `DynamicTableSinkFactory` 实例通过 Java的 [Service Provider Interfaces (SPI)](https://docs.oracle.com/javase/tutorial/sound/SPI-intro.html)被发现。with中的connector选项和有效的factory标识必须一致。

尽管在类命名中可能不明显，但DynamicTableSource和DynamicTableSink也可以看作是有状态的工厂，它们最终生成具体的运行时实现，用于读/写实际数据

The planner uses the source and sink instances to perform connector-specific bidirectional comunication until an optimal logical plan could be found. Depending on the optionally declared ability interfaces (e.g. `SupportsProjectionPushDown` or `SupportsOverwrite`), the planner might apply changes to an instance and thus mutate the produced runtime implementation.

## Runtime

当逻辑规划结束后，planner从table connector中获取运行实现（ *runtime implementation*） 。运行的逻辑在flink的connector核心接口中实现，如 `InputFormat` or `SourceFunction`。

这些接口按另一个抽象级别分组为ScanRuntimeProvider、LookupRuntimeProvider和SinkRuntimeProvider的子类。

例如，OutputFormatProvider和SinkFunctionProvider都是SinkRuntimeProvider的具体实例，规划器可以处理他们。

 `OutputFormatProvider`提供  `org.apache.flink.api.common.io.OutputFormat`) 

 `SinkFunctionProvider` 提供 `org.apache.flink.streaming.api.functions.sink.SinkFunction`) 



# 扩展接口介绍

本节介绍扩展Flink表连接器的可用接口。

## Dynamic Table Factories

Dynamic table factories用于根据catalog and session information为外部存储系统配置dynamic table connector。

**这里的catalog and session 信息指的是谁的呢，尼玛我翻译不出来。**

`org.apache.flink.table.factories.DynamicTableSourceFactory` 被实现，用于创建 `DynamicTableSource`.

`org.apache.flink.table.factories.DynamicTableSinkFactory` 被实现，用于创建 `DynamicTableSink`.

By default, the factory is discovered using the value of the `connector` option as the factory identifier and Java’s Service Provider Interface.

In JAR files, references to new implementations can be added to the service file:

```
META-INF/services/org.apache.flink.table.factories.Factory
```

The framework will check for a single matching factory that is uniquely identified by factory identifier and requested base class (e.g. `DynamicTableSourceFactory`).

The factory discovery process can be bypassed by the catalog implementation if necessary. For this, a catalog needs to return an instance that implements the requested base class in `org.apache.flink.table.catalog.Catalog#getFactory`.

## Dynamic Table Source

根据定义，动态表可以随时间变化。

在读取动态表时，可以将内容视为：

- 一种变更日志（有限的或无限的），它的所有变更都被连续地使用，直到变更日志用完为止。这由ScanTableSource接口表示。
- 一种不断变化的或非常大的外部表，其内容通常不会被完全读取，而是在必要时查询单个值。这由LookupTableSource接口表示。

一个类可以同时实现这两个接口。计划者根据指定的查询来决定它们的用途。

### Scan Table Source

A `ScanTableSource` scans all rows from an external storage system during runtime.

被扫描的行不但包含insert，还包含update和delete，因此这种table source被用于读取有限或无限的changelog。The returned *changelog mode* indicates the set of changes that the planner can expect during runtime.

对常规的批处理方案，这种table source能发出只插入行的有界的流。

对常规的流处理方案，这种table source能发出只插入行的无界的流。

对于change data capture (CDC)方案,这种table source能发出带有insert, update, and delete行的有界或无界流。

A table source可以实现更进一步的功能接口，如 `SupportsProjectionPushDown`，他可以在计划期间更改实例。All abilities are listed in the `org.apache.flink.table.connector.source.abilities` package and in the documentation of `org.apache.flink.table.connector.source.ScanTableSource`.

The runtime implementation of a `ScanTableSource` must produce internal data structures. 因此记录必须用 `org.apache.flink.table.data.RowData`发送。 The framework provides runtime converters such that a source can still work on common data structures and perform a conversion at the end.

### Lookup Table Source

 `LookupTableSource` 在运行时，通过一个或多个键查找外部系统的行。

和 `ScanTableSource`相比，这种source 没必要读取整个表，而是在需要时可以从外部系统单独读取个别值。

和 `ScanTableSource`相比，`LookupTableSource` 目前只支持发送insert-only更改。

不支持更近一步的功能接口. See the documentation of `org.apache.flink.table.connector.source.LookupTableSource` for more information.

The runtime implementation of a `LookupTableSource` is a `TableFunction` or `AsyncTableFunction`. The function will be called with values for the given lookup keys during runtime.

## Dynamic Table Sink

根据定义，动态表可以随时间变化

When writing a dynamic table, the content can always be considered as a changelog (finite or infinite) for which all changes are written out continuously until the changelog is exhausted. The returned *changelog mode* indicates the set of changes that the sink accepts during runtime.

For regular batch scenarios, the sink can solely accept insert-only rows and write out bounded streams.

For regular streaming scenarios, the sink can solely accept insert-only rows and can write out unbounded streams.

For change data capture (CDC) scenarios, the sink can write out bounded or unbounded streams with insert, update, and delete rows.

A table sink can implement further abilitiy interfaces such as `SupportsOverwrite` that might mutate an instance during planning. All abilities are listed in the `org.apache.flink.table.connector.sink.abilities` package and in the documentation of `org.apache.flink.table.connector.sink.DynamicTableSink`.

The runtime implementation of a `DynamicTableSink` must consume internal data structures. Thus, records must be accepted as `org.apache.flink.table.data.RowData`. The framework provides runtime converters such that a sink can still work on common data structures and perform a conversion at the beginning.

## Encoding / Decoding Formats

Some table connectors accept different formats that encode and decode keys and/or values.

Formats work similar to the pattern `DynamicTableSourceFactory -> DynamicTableSource -> ScanRuntimeProvider`, where the factory is responsible for translating options and the source is responsible for creating runtime logic.

Because formats might be located in different modules, they are discovered using Java’s Service Provider Interface similar to [table factories](https://ci.apache.org/projects/flink/flink-docs-release-1.11/zh/dev/table/sourceSinks.html#dynamic-table-factories). In order to discover a format factory, the dynamic table factory searches for a factory that corresponds to a factory identifier and connector-specific base class.

For example, the Kafka table source requires a `DeserializationSchema` as runtime interface for a decoding format. Therefore, the Kafka table source factory uses the value of the `value.format` option to discover a `DeserializationFormatFactory`.

The following format factories are currently supported:

```
org.apache.flink.table.factories.DeserializationFormatFactory
org.apache.flink.table.factories.SerializationFormatFactory
```

The format factory translates the options into an `EncodingFormat` or a `DecodingFormat`. Those interfaces are another kind of factory that produce specialized format runtime logic for the given data type.

For example, for a Kafka table source factory, the `DeserializationFormatFactory` would return an `EncodingFormat` that can be passed into the Kafka table source.



# 举个栗子

本节将介绍如何实现一个具有解码格式的并且支持日志变更语义的scan table source。并说明提到的组件是如何在一起运行的。这个例子可以作为实现参考。

特别是例子展示了：

- create factories that parse and validate options,
- implement table connectors,
- implement and discover custom formats,
- and use provided utilities such as data structure converters and the `FactoryUtil`.

The table source uses a simple single-threaded `SourceFunction` to open a socket that listens for incoming bytes. The raw bytes are decoded into rows by a pluggable format. The format expects a changelog flag as the first column.

We will use most of the interfaces metioned above to enable the following DDL:

```
CREATE TABLE UserScores (name STRING, score INT)
WITH (
  'connector' = 'socket',
  'hostname' = 'localhost',
  'port' = '9999',
  'byte-delimiter' = '10',
  'format' = 'changelog-csv',
  'changelog-csv.column-delimiter' = '|'
);
```

Because the format supports changelog semantics, we are able to ingest updates during runtime and create an updating view that can continuously evaluate changing data:

```
SELECT name, SUM(score) FROM UserScores GROUP BY name;
```

Use the following command to ingest data in a terminal:

```
> nc -lk 9999
INSERT|Alice|12
INSERT|Bob|5
DELETE|Alice|12
INSERT|Alice|18
```



## Factories

本节说明如何将来自目录的元数据转换为具体的连接器

这两个工厂都已添加到META-INF/services目录中。

**`SocketDynamicTableFactory`**

The `SocketDynamicTableFactory` translates the catalog table to a table source. Because the table source requires a decoding format, we are discovering the format using the provided `FactoryUtil` for convenience.

代码在github上：flink\flink-examples\flink-examples-table\src\main\java\org\apache\flink\table\examples\java\connectors

**`ChangelogCsvFormatFactory`**

The `ChangelogCsvFormatFactory` translates format-specific options to a format. The `FactoryUtil` in `SocketDynamicTableFactory` takes care of adapting the option keys accordingly and handles the prefixing like `changelog-csv.column-delimiter`.

Because this factory implements `DeserializationFormatFactory`, it could also be used for other connectors that support deserialization formats such as the Kafka connector.

代码在github上：flink\flink-examples\flink-examples-table\src\main\java\org\apache\flink\table\examples\java\connectors

## Table Source and Decoding Format

**`SocketDynamicTableSource`**

The `SocketDynamicTableSource` is used during planning. In our example, we don’t implement any of the available ability interfaces. Therefore, the main logic can be found in `getScanRuntimeProvider(...)` where we instantiate the required `SourceFunction` and its `DeserializationSchema` for runtime. Both instances are parameterized to return internal data structures (i.e. `RowData`).

**`ChangelogCsvFormat`**

The `ChangelogCsvFormat` is a decoding format that uses a `DeserializationSchema` during runtime. It supports emitting `INSERT` and `DELETE` changes.

## Runtime

For completeness, this section illustrates the runtime logic for both `SourceFunction` and `DeserializationSchema`.

**ChangelogCsvDeserializer**

The `ChangelogCsvDeserializer` contains a simple parsing logic for converting bytes into `Row` of `Integer` and `String` with a row kind. The final conversion step converts those into internal data structures.

**SocketSourceFunction**

The `SocketSourceFunction` opens a socket and consumes bytes. It splits records by the given byte delimiter (`\n` by default) and delegates the decoding to a pluggable `DeserializationSchema`. The source function can only work with a parallelism of 1.







# 实现自定义数据源

自定义数据源需要实现Flink提供的SourceFunction接口。

SourceFunction接口的定义如下：

```java
@Public
public interface SourceFunction<T> extends Function, Serializable {
    void run(SourceContext<T> ctx) throws Exception;
    void cancel();
}
```

## run方法

run方法为数据源向下游发送数据的主要逻辑。编写套路为：

- 不断调用循环发送数据。
- 使用一个状态变量控制循环的执行。当`cancel`方法执行后必须能够跳出循环，停止发送数据。
- 使用SourceContext的collect等方法将元素发送至下游。
- 如果使用Checkpoint，在SourceContext collect数据的时候必须加锁。防止checkpoint操作和发送数据操作同时进行。

## cancel方法：

`cancel`方法在数据源停止的时候调用。`cancel`方法必须能够控制`run`方法中的循环，停止循环的运行。并做一些状态清理操作。

## SourceContext类

SourceContext在SourceFunction中使用，用于向下游发送数据，或者是发送watermark。
 SourceContext的方法包括：

- collect：向下游发送数据。有如下三种情况：
  - 如果使用ProcessingTime，该元素不携带timestamp。
  - 如果使用IngestionTime，元素使用系统当前时间作为timestamp。
  - 如果使用EventTime，元素不携带timestamp。需要在数据流后续为元素指定timestamp（assignTimestampAndWatermark）。
- collectWithTimestamp：向下游发送带有timestamp的数据。和collect方法一样也有如下三种情况：
  - 如果使用ProcessingTime，timestamp会被忽略
  - 如果使用IngestionTime，使用系统时间覆盖timestamp
  - 如果使用EventTime，使用指定的timestamp
- emitWatermark：向下游发送watermark。watermark也包含一个timestamp。向下游发送watermark意味着所有在watermark的timestamp之前的数据已经到齐。如果在watermark之后，收到了timestamp比该watermark的timestamp小的元素，该元素会被认为迟到，将会被系统忽略，或者进入到旁路输出（side output）。
- markAsTemporarilyIdle：标记此数据源暂时闲置。该数据源暂时不会发送任何数据和watermark。仅对IngestionTime和EventTime生效。下游任务前移watermark的时候将不会再等待被标记为闲置的数据源的watermark。

# CheckpointedFunction

如果数据源需要保存状态，那么就需要实现CheckpointedFunction中的相关方法。
 CheckpointedFunction包含如下方法：

- snapshotState：保存checkpoint的时候调用。需要在此方法中编写状态保存逻辑
- initializeState：在数据源创建或者是从checkpoint恢复的时候调用。此方法包含数据源的状态恢复逻辑。

# 样例

Flink官方给出的样板Source。这个数据源会发送0-999到下游系统。代码如下所示：

```java
public class ExampleCountSource implements SourceFunction<Long>, CheckpointedFunction {
    private long count = 0L;
    // 使用一个volatile类型变量控制run方法内循环的运行
    private volatile boolean isRunning = true;

    // 保存数据源状态的变量
    private transient ListState<Long> checkpointedCount;

    public void run(SourceContext<T> ctx) {
        while (isRunning && count < 1000) {
            // this synchronized block ensures that state checkpointing,
            // internal state updates and emission of elements are an atomic operation
            // 此处必须要加锁，防止在checkpoint过程中，仍然发送数据
            synchronized (ctx.getCheckpointLock()) {
                ctx.collect(count);
                count++;
            }
        }
    }

    public void cancel() {
        // 设置isRunning为false，终止run方法内循环的运行
        isRunning = false;
    }

    public void initializeState(FunctionInitializationContext context) {
        // 获取存储状态
        this.checkpointedCount = context
            .getOperatorStateStore()
            .getListState(new ListStateDescriptor<>("count", Long.class));

        // 如果数据源是从失败中恢复，则读取count的值，恢复数据源count状态
        if (context.isRestored()) {
            for (Long count : this.checkpointedCount.get()) {
                this.count = count;
            }
        }
    }

    public void snapshotState(FunctionSnapshotContext context) {
        // 保存数据到状态变量
        this.checkpointedCount.clear();
        this.checkpointedCount.add(count);
    }
}
```

