# Java项目模板

[TOC]



# 构建工具

flink提供两种构建工具

maven

gradle

# Maven

## 环境要求

Maven 3.0.4 或更高

Java 8.x

## 创建项目

linux 命令行（本命令创建基本项目）

```
    $ mvn archetype:generate                               \
      -DarchetypeGroupId=org.apache.flink              \
      -DarchetypeArtifactId=flink-quickstart-java      \
      -DarchetypeVersion=1.10.0
```

只有上面的内容，构建真的很慢，需要加入如下catalog和interactive参数（本命令创建tableapi）

```
    -DgroupId=orz.an ^
    -DartifactId=flinktableapitest ^
    -Dversion=0.1 ^
    -Dpackage=orz.an.flinktableapitest ^
    -P aliyun-p,aliyun-s ^
    -DarchetypeCatalog=internal
    -DinteractiveMode=false ^
```

下两个参数很重要



## 构建项目

mvn命令打包