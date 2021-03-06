# 两机文件复制

scp [可选参数] file_source file_target 

scp -r dirname/    ip:/dirname/

scp  filename ip:/dirname

scp  filename ip:/dirname/filename 

## 从 本地 复制到 远程 

\* 复制文件： 

​        \* 命令格式： 

```
scp local_file remote_username@remote_ip:remote_folder
scp local_file remote_username@remote_ip:remote_file 
scp local_file remote_ip:remote_folder 
scp local_file remote_ip:remote_file
```

 

​                第1,2个指定了用户名，命令执行后需要再输入密码，第1个仅指定了远程的目录，文件名字不变，第2个指定了文件名； 

​                第3,4个没有指定用户名，命令执行后需要输入用户名和密码，第3个仅指定了远程的目录，文件名字不变，第4个指定了文件名； 

​        \* 例子： 

​                scp /home/space/music/1.mp3 root@www.cumt.edu.cn:/home/root/others/music 

​                scp /home/space/music/1.mp3 root@www.cumt.edu.cn:/home/root/others/music/001.mp3 

​                scp /home/space/music/1.mp3 [www.cumt.edu.cn:/home/root/others/music](www.cumt.edu.cn://home/root/others/music) 

​                scp /home/space/music/1.mp3 [www.cumt.edu.cn:/home/root/others/music/001.mp3](www.cumt.edu.cn://home/root/others/music/001.mp3) 

 

\* 复制目录： 

​        \* 命令格式： 

​                scp -r local_folder remote_username@remote_ip:remote_folder 

​                或者 

​                scp -r local_folder remote_ip:remote_folder 

 

​                第1个指定了用户名，命令执行后需要再输入密码； 

​                第2个没有指定用户名，命令执行后需要输入用户名和密码； 

​        \* 例子： 

​                scp -r /home/space/music/ root@www.cumt.edu.cn:/home/root/others/ 

​                scp -r /home/space/music/ [www.cumt.edu.cn:/home/root/others/](www.cumt.edu.cn://home/root/others/) 

 

​                上面 命令 将 本地 music 目录 复制 到 远程 others 目录下，即复制后有 远程 有 ../others/music/ 目录 

 

 

## 从 远程 复制到 本地 

从 远程 复制到 本地，只要将 从 本地 复制到 远程 的命令 的 后2个参数 调换顺序 即可； 

 例如： 

```
scp root@www.cumt.edu.cn:/home/root/others/music /home/space/music/1.mp3 
scp -r [www.cumt.edu.cn:/home/root/others/](www.cumt.edu.cn://home/root/others/) /home/space/music/
```

最简单的应用如下 : 

 

scp 本地用户名 @IP 地址 : 文件名 1 远程用户名 @IP 地址 : 文件名 2 

 

[ 本地用户名 @IP 地址 :] 可以不输入 , 可能需要输入远程用户名所对应的密码 . 

 

可能有用的几个参数 : 

 

-v 和大多数 linux 命令中的 -v 意思一样 , 用来显示进度 . 可以用来查看连接 , 认证 , 或是配置错误 . 

 

-C 使能压缩选项 . 

 

-P 选择端口 . 注意 -p 已经被 rcp 使用 . 

 

-4 强行使用 IPV4 地址 . 

 

-6 强行使用 IPV6 地址 .

 

来自 <<http://www.cnblogs.com/hitwtx/archive/2011/11/16/2251254.html>> 