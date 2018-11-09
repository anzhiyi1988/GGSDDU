Minio 是一个基于Apache License v2.0标准的对象存储服务。可对接Amazon S3 云服务，他比较适合存储非结构性文档，如图谱、视频、日志文件、文件、备份和虚拟机镜像。对象的大小可以是几KB到最大5TB不等。

  

Minio很轻量，能和很多应用集成，类似NodeJS、Redis和MySQL。



# Linux 安装 

| Platform  | Architecture | URL                                                          |
| --------- | ------------ | ------------------------------------------------------------ |
| GNU/Linux | 64-bit Intel | <https://dl.minio.io/server/minio/release/linux-amd64/minio> |

  

```
wget https://dl.minio.io/server/minio/release/linux-amd64/minio
chmod +x minio
./minio server /data
```

## 打开防火墙  

Minio默认用9000端口监听访问。

### iptables

用 `iptables` (RHEL, CentOS, etc) 命令开通端口  

```
iptables -A INPUT -p tcp --dport 9000 -j ACCEPT
service iptables restart
```

9000-9010范围开通命令

```
iptables -A INPUT -p tcp --dport 9000:9010 -j ACCEPT
service iptables restart
```

### firewall-cmd  

用 `firewall-cmd` (CentOS) 命令开通端口访问。 

```
firewall-cmd --get-active-zones
```

获取有效的zone(s)然后用下面命令开通端口，例如获取的zone 是 `public` 则  

```
firewall-cmd --zone=public --add-port=9000/tcp --permanent
```

 `permanent` 参数指无论start, restart or reload都有效，最后reload the firewall是生效。  

```
firewall-cmd --reload
```

## 测试Minio

访问 http://127.0.0.1:9000/

