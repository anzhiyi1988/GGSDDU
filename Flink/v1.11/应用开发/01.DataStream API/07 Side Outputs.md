# Side Outputs

除了 `DataStream` operations操作的主流之外，还可以产生任意数量的附加结果输出流，附加流的类型不一定与主流的类型一直，而且不同的附加流类型也可以不同。如果程序运行过程中需要分割一个流或者过滤一个流，这个操作非常有用。

如果使用side outputs，必须定义一个 `OutputTag` 标签用于标识side output：

```java
// this needs to be an anonymous inner class, so that we can analyze the type
OutputTag<String> outputTag = new OutputTag<String>("side-output") {};
```

Notice how the `OutputTag` is typed according to the type of elements that the side output stream contains.

通过一下function向side output 发送数据：

- [ProcessFunction](https://ci.apache.org/projects/flink/flink-docs-release-1.11/dev/stream/operators/process_function.html)
- [KeyedProcessFunction](https://ci.apache.org/projects/flink/flink-docs-release-1.11/dev/stream/operators/process_function.html#the-keyedprocessfunction)
- CoProcessFunction
- KeyedCoProcessFunction
- [ProcessWindowFunction](https://ci.apache.org/projects/flink/flink-docs-release-1.11/dev/stream/operators/windows.html#processwindowfunction)
- ProcessAllWindowFunction

使用上述函数中暴露的 `Context`参数，把数据发送给`OutputTag`定义的side output，一下是一个`ProcessFunction`例子：



```java
DataStream<Integer> input = ...;

final OutputTag<String> outputTag = new OutputTag<String>("side-output"){};

SingleOutputStreamOperator<Integer> mainDataStream = input
  .process(new ProcessFunction<Integer, Integer>() {

      @Override
      public void processElement(
          Integer value,
          Context ctx,
          Collector<Integer> out) throws Exception {
        // emit data to regular output
        out.collect(value);

        // emit data to side output
        ctx.output(outputTag, "sideout-" + String.valueOf(value));
      }
    });
```

 如果要遍历 side output流需要在`DataStream` 上使用 `getSideOutput(OutputTag)` 操作，得到的结果就是side output 流:

```java
final OutputTag<String> outputTag = new OutputTag<String>("side-output"){};

SingleOutputStreamOperator<Integer> mainDataStream = ...;

DataStream<String> sideOutputStream = mainDataStream.getSideOutput(outputTag);
```