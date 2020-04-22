#### Introduction负载均衡

跨多个应用实例

优化资源利用

最大化吞吐量

减少延迟

容错

#### Load balancing methods负载均衡方法

- round-robin — requests to the application servers are distributed in a round-robin fashion,轮询
- least-connected — next request is assigned to the server with the least number of active connections,谁活动少给谁
- ip-hash — a hash-function is used to determine what server should be selected for the next request (based on the client’s IP address).

#### Default load balancing configuration

默认是轮询方式

```
http {
    upstream myapp1 {
        server srv1.example.com;
        server srv2.example.com;
        server srv3.example.com;
    }

    server {
        listen 80;

        location / {
            proxy_pass http://myapp1;
        }
    }
}
```

#### Least connected load balancing

按忙闲分配

```
 upstream myapp1 {
        least_conn;
        server srv1.example.com;
        server srv2.example.com;
        server srv3.example.com;
    }
```

#### hash

如果要保证session是持续的，用hash比较好

```
upstream myapp1 {
    ip_hash;
    server srv1.example.com;
    server srv2.example.com;
    server srv3.example.com;
}
```

#### Weighted load balancing

可以配置权重

```
upstream myapp1 {
        server srv1.example.com weight=3;
        server srv2.example.com;
        server srv3.example.com;
    }
```

#### Health checks

健康检查，可以配置最大尝试次数，不成功则标记为无效服务器。