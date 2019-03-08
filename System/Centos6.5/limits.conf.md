  

linux 资源限制 limits.conf ulimit2009-09-05 12:06limits.conf 配置

=============================================================================

limits.conf 文件实际是 Linux PAM（插入式认证模块，Pluggable Authentication Modules）中 pam_limits.so 的配置文件，而且只针对于单个会话。

limits.conf的格式如下：

username|@groupname type resource limit

username|@groupname：设置需要被限制的用户名，组名前面加@和用户名区别。也可以用通配符*来做所有用户的限制。

type：有 soft，hard 和 -，soft 指的是当前系统生效的设置值。hard 表明系统中所能设定的最大值。soft 的限制不能比har 限制高。用 - 就表明同时设置了 soft 和 hard 的值。

resource：

core - 限制内核文件的大小

date - 最大数据大小

fsize - 最大文件大小

memlock - 最大锁定内存地址空间

nofile - 打开文件的最大数目

rss - 最大持久设置大小

stack - 最大栈大小

cpu - 以分钟为单位的最多 CPU 时间

noproc - 进程的最大数目

as - 地址空间限制

maxlogins - 此用户允许登录的最大数目

要使 limits.conf 文件配置生效，必须要确保 pam_limits.so 文件被加入到启动文件中。查看 /etc/pam.d/login 文件中有：

session required /lib/security/pam_limits.so

例：

oracle soft nproc 2047 

oracle hard nproc 16384 

oracle soft nofile 1024 

oracle hard nofile 65536

===========================================================================

 LIMITS.CONF的工作原理：

==================================================================================

LIMITS.CONF的后端是这样工作的：LIMITS.CONF是PAM_LIMITS.SO的配置文件，然后/ETC/PAM.D/下的应用程序调 用PAM_***.SO模块。譬如说，当用户拜访服务器，服务程序将请求发送到PAM模块，PAM模块根据服务名称在/ETC/PAM.D目录下选择一个 对应的服务文件，然后根据服务文件的内容选择具体的PAM模块进行处理。

 

​    例：限制ADMIN用户登录到SSHD的服务不能超过2个

 

 

​    在/ETC/PAM.D/SSHD 中添加 SESSION REQUIRED PAM_LIMITS.SO

 

 

​    在/ETC/SECURITY/LIMITS.CONF中添加 ADMIN - MAXLOGINS 2

 

 

​    查看应用程序能否被PAM支持，用LDD

 

============================================================================

 

ulimit 命令用法

 

============================================================================================

 

 

Bash

Bash内建了一个限制器"ulimit"。注意任何硬限制都不能设置得太高，因此如果你在/etc/profile或用户的 .bash_profile （用户不能编辑或

 

删除这些文件）中定义了限制规则，你就能对用户的Bash shell实施限制。这对于缺少PAM支持的LINUX旧发行版本是很有用的。你还必须确保

 

用户不能改变他们的登录shell。限制的设置与PAM相似。例如：

 

ulimit –Sc 0

ulimit –Su 100

ulimit –Hu 150

 

 

<http://www.ringkee.com/jims/read_folder/books/LinuxHackingExposed>

Ulimit命令

设置限制     可以把命令加到profile文件里，也可以在/etc/security/limits.conf文件中定义

限制。

命令参数

-a      显示所有限制

-c      core文件大小的上限

-d      进程数据段大小的上限

-f      shell所能创建的文件大小的上限

-m     驻留内存大小的上限

-s      堆栈大小的上限

-t      每秒可占用的CPU时间上限

-p     管道大小

-n     打开文件数的上限

-u     进程数的上限

-v     虚拟内存的上限

 