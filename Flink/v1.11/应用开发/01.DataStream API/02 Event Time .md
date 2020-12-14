# Event Time

本节介绍如何编写有时间意识的flink程序，请学习 [Timely Stream Processing](https://ci.apache.org/projects/flink/flink-docs-release-1.11/zh/concepts/timely-stream-processing.html) 了解及时流处理的概念。

有关如何在flink中使用时间的方法请参考 [windowing](https://ci.apache.org/projects/flink/flink-docs-release-1.11/zh/dev/stream/operators/windows.html) 和[ProcessFunction](https://ci.apache.org/projects/flink/flink-docs-release-1.11/zh/dev/stream/operators/process_function.html).

## Setting a Time Characteristic

flink流式程序第一部分通常是设置基本时间特性*time characteristic*，该设置定义了source的行为方式，以及类似 `KeyedStream.timeWindow(Time.seconds(30))`的窗口操作用哪个时间概念。

以下例子，在每小时的时间窗口中聚合事件，窗口的行为与时间特性相适应：

```java
final StreamExecutionEnvironment env = StreamExecutionEnvironment.getExecutionEnvironment();

env.setStreamTimeCharacteristic(TimeCharacteristic.ProcessingTime);

// 其他选项 alternatively:
// env.setStreamTimeCharacteristic(TimeCharacteristic.IngestionTime);
// env.setStreamTimeCharacteristic(TimeCharacteristic.EventTime);

DataStream<MyEvent> stream = env.addSource(new FlinkKafkaConsumer010<MyEvent>(topic, schema, props));

stream
    .keyBy( (event) -> event.getUser() )
    .timeWindow(Time.hours(1))
    .reduce( (a, b) -> a.add(b) )
    .addSink(...);
```

为了在event time模式下运行此例，要么使用的source需要为数据定义event time并发送水印，要么程序需要在source后面注入一个时间戳赋值器Timestamp Assigner 和 水印生成器 Watermark Generator。

这些函数描述了如何访问事件的时间戳和事件展现的无序程度。

以下部分描述 *timestamps* 和*watermarks*背后的机制，如何使用时间戳赋值器 和 水印生成器请参考[Generating Timestamps / Watermarks](https://ci.apache.org/projects/flink/flink-docs-release-1.11/dev/event_timestamps_watermarks.html).

## Idling sources

Currently, with pure event time watermarks generators, watermarks can not progress if there are no elements to be processed. That means in case of gap in the incoming data, event time will not progress and for example the window operator will not be triggered and thus existing windows will not be able to produce any output data.

To circumvent this one can use periodic watermark assigners that don’t only assign based on element timestamps. An example solution could be an assigner that switches to using current processing time as the time basis after not observing new events for a while.

Sources can be marked as idle using `SourceFunction.SourceContext#markAsTemporarilyIdle`. For details please refer to the Javadoc of this method as well as `StreamStatus`.

## 调试Debugging Watermarks

Please refer to the [Debugging Windows & Event Time](https://ci.apache.org/projects/flink/flink-docs-release-1.11/monitoring/debugging_event_time.html) section for debugging watermarks at runtime.

## operators如何处理水印

一般来说，在将给定的水印转发到下游之前，operators 需要完全处理它，例如， `WindowOperator` 将先计算应该触发哪些窗口，只有在这个水印触发了所有产生的输出后，这个水印本身才会被发送到下游。 In other words, all elements produced due to occurrence of a watermark will be emitted before the watermark.

The same rule applies to `TwoInputStreamOperator`. However, in this case the current watermark of the operator is defined as the minimum of both of its inputs.

The details of this behavior are defined by the implementations of the `OneInputStreamOperator#processWatermark`, `TwoInputStreamOperator#processWatermark1` and `TwoInputStreamOperator#processWatermark2` methods.

