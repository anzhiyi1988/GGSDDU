# 一、JDBC 驱动

clickhouse 有两种 JDBC 驱动实现。
**官方驱动：**

```xml
<dependency>
    <groupId>ru.yandex.clickhouse</groupId>
    <artifactId>clickhouse-jdbc</artifactId>
    <version>0.1.52</version>
</dependency>
12345
```

**三方提供的驱动：**

```xml
<dependency>
    <groupId>com.github.housepower</groupId>
    <artifactId>clickhouse-native-jdbc</artifactId>
    <version>1.6-stable</version>
</dependency>
12345
```

两者间的主要区别如下：

- 驱动类加载路径不同，分别为 `ru.yandex.clickhouse.ClickHouseDriver` 和 `com.github.housepower.jdbc.ClickHouseDriver`
- 默认连接端口不同，分别为 `8123` 和 `9000`
- 连接协议不同，官方驱动使用 HTTP 协议，而三方驱动使用 TCP 协议

需要注意的是，**两种驱动不可共用**，同个项目中只能选择其中一种驱动。

