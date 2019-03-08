验证是否安装

```
[root@anzhy Packages]# rpm -qa |grep ssh
openssh-clients-5.3p1-94.el6.x86_64
libssh2-1.4.2-1.el6.x86_64
openssh-5.3p1-94.el6.x86_64
openssh-server-5.3p1-94.el6.x86_64
[root@anzhy Packages]# 
```

 

如果没有以上内容就需要安装 

安装的时候如果碰到如下错误

```
[root@anzhy Packages]# rpm -ivh openssh-clients-5.3p1-94.el6.x86_64.rpm 
warning: openssh-clients-5.3p1-94.el6.x86_64.rpm: Header V3 RSA/SHA1 Signature, key ID c105b9de: NOKEY
error: Failed dependencies:
    libedit.so.0()(64bit) is needed by openssh-clients-5.3p1-94.el6.x86_64
[root@anzhy Packages]# 
则需要安装lebedit
[root@anzhy Packages]# rpm -ivh libedit-2.11-4.20080712cvs.1.el6.x86_64.rpm 
warning: libedit-2.11-4.20080712cvs.1.el6.x86_64.rpm: Header V3 RSA/SHA256 Signature, key ID c105b9de: NOKEY
Preparing...                ########################################### [100%]
  1:libedit                ########################################### [100%]
[root@anzhy Packages]# 
```

 

验证是否启动

service sshd status

 

测试登录本机

```
[root@anzhy Packages]# ssh localhost
root@localhost's password: 
Last login: Mon Aug  3 19:22:40 2015 from localhost
[root@anzhy ~]# 
```

 

以上就安装完毕了

注意如果机器是联网的，可以使用yum命令安装。

