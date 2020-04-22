

参考官方网站的内容进行测试

https://ci.apache.org/projects/flink/flink-docs-release-1.10/zh/getting-started/walkthroughs/table_api.html



tableAPI统一批处理和流处理方式，使其使用相同的语义执行流式和批量数据集，并产生相同的结果。

tableAPI用于简化数据分析、ETL、数据流水线三类程序。

# 建立项目

快速构建tableAPI项目（[mvn命令详解](../../../Maven/命令.md)）

```
mvn archetype:generate ^
    -DarchetypeGroupId=org.apache.flink ^
    -DarchetypeArtifactId=flink-walkthrough-table-java ^
    -DarchetypeVersion=1.10.0 ^
    -DgroupId=orz.an ^
    -DartifactId=flinktableapitest ^
    -Dversion=0.1 ^
    -Dpackage=orz.an.flinktableapitest ^
    -DinteractiveMode=false ^
    -P aliyun-p,aliyun-s ^
    -DarchetypeCatalog=internal
```



# 编码

数据一般是读取到source中，目的地是sink，这两个类的子类需要注册到环境中使用。

source子类，完成数据的获取，数据可以从数据库、键值存储、消息队列或文件系统中获取。

sink子类，完成数据的输出，格式支持csv、json、avro、parquet，目的存储支持文件和数据库。



最基础的一个表到一个表，但是没有用到数据库。



参考mycode中的项目flinktableapitest

