- **一、****yum** **简介**

- yum，是Yellow dog Updater, Modified 的简称，是杜克大学为了提高RPM  软件包安装性而开发的一种软件包管理器。起初是由yellow dog 这一发行版的开发者Terra Soft 研发，用python 写成，那时还叫做yup(yellow dog updater)，后经杜克大学的Linux@Duke  开发团队进行改进，遂有此名。yum 的宗旨是自动化地升级，安装/移除rpm 包，收集rpm 包的相关信息，检查依赖性并自动提示用户解决。yum 的关键之处是要有可靠的repository，顾名思义，这是软件的仓库，它可以是http 或ftp 站点，也可以是本地软件池，但必须包含rpm 的header，header 包括了rpm 包的各种信息，包括描述，功能，提供的文件，依赖性等。正是收集了这些header 并加以分析，才能自动化地完成余下的任务。

- yum 的理念是使用一个中心仓库(repository)管理一部分甚至一个distribution 的应用程序相互关系，根据计算出来的软件依赖关系进行相关的升级、安装、删除等等操作，减少了Linux 用户一直头痛的dependencies 的问题。这一点上，yum 和apt 相同。apt 原为debian 的deb 类型软件管理所使用，但是现在也能用到RedHat 门下的rpm 了。

- yum 主要功能是更方便的添加/删除/更新RPM 包，自动解决包的倚赖性问题，便于管理大量系统的更新问题。

- yum 可以同时配置多个资源库(Repository)，简洁的配置文件（/etc/yum.conf），自动解决增加或删除rpm 包时遇到的依赖性问题，保持与RPM 数据库的一致性。

- **三、****yum** **配置**

- yum 的配置文件分为两部分：main 和repository

- - main 部分定义了全局配置选项，整个yum 配置文件应该只有一个main。常位于/etc/yum.conf 中。
  - repository 部分定义了每个源/服务器的具体配置，可以有一到多个。常位于/etc/yum.repo.d 目录下的各文件中。

- yum.conf 文件一般位于/etc目录下，一般其中只包含main部分的配置选项。

- \# cat /etc/yum.conf

- [-------------------------------------------------------------------------------------------

- [main]
     cachedir=/var/cache/yum

- //yum 缓存的目录，yum 在此存储下载的rpm 包和数据库，默认设置为/var/cache/yum
     keepcache=0

- //安装完成后是否保留软件包，0为不保留（默认为0），1为保留
     debuglevel=2

- //Debug 信息输出等级，范围为0-10，缺省为2
     logfile=/var/log/yum.log

- //yum 日志文件位置。用户可以到/var/log/yum.log 文件去查询过去所做的更新。
     pkgpolicy=newest

- //包的策略。一共有两个选项，newest 和last，这个作用是如果你设置了多个repository，而同一软件在不同的repository 中同时存在，yum 应该安装哪一个，如果是newest，则yum 会安装最新的那个版本。如果是last，则yum 会将服务器id 以字母表排序，并选择最后的那个服务器上的软件安装。一般都是选newest。

- distroverpkg=redhat-release

- //指定一个软件包，yum 会根据这个包判断你的发行版本，默认是redhat-release，也可以是安装的任何针对自己发行版的rpm 包。
     tolerant=1

- //有1和0两个选项，表示yum 是否容忍命令行发生与软件包有关的错误，比如你要安装1,2,3三个包，而其中3此前已经安装了，如果你设为1,则yum 不会出现错误信息。默认是0。
     exactarch=1

- //有1和0两个选项，设置为1，则yum 只会安装和系统架构匹配的软件包，例如，yum 不会将i686的软件包安装在适合i386的系统中。默认为1。
     retries=6

- //网络连接发生错误后的重试次数，如果设为0，则会无限重试。默认值为6.

- obsoletes=1

- //这是一个update 的参数，具体请参阅yum(8)，简单的说就是相当于upgrade，允许更新陈旧的RPM包。
     plugins=1

- //是否启用插件，默认1为允许，0表示不允许。我们一般会用yum-fastestmirror这个插件。
     bugtracker_url=http://bugs.centos.org/set_project.php?project_id=16&ref=http://bugs.centos.org/bug_report_page.php?category=yum

- \# Note: yum-RHN-plugin doesn't honor this.
     metadata_expire=1h

- installonly_limit = 5

- \# PUT YOUR REPOS HERE OR IN separate files named  file.repo
     \# in /etc/yum.repos.d

- -----------------------------------------------------------------------------------]

- 除了上述之外，还有一些可以添加的选项，如：

- exclude=selinux*　　// 排除某些软件在升级名单之外，可以用通配符，列表中各个项目要用空格隔开，这个对于安装了诸如美化包，中文补丁的朋友特别有用。

- gpgcheck=1　　// 有1和0两个选择，分别代表是否是否进行gpg(GNU Private Guard) 校验，以确定rpm 包的来源是有效和安全的。这个选项如果设置在[main]部分，则对每个repository 都有效。默认值为0。

- **四、配置本地****yum****源**

- 1、挂载系统安装光盘

- \# mount /dev/cdrom /mnt/cdrom/

- 2、配置本地yum源

- \# cd /etc/yum.repos.d/

- \# ls

- 会看到四个repo 文件

- ![img](file:///C:/Users/anzhy/AppData/Local/Temp/msohtmlclip1/02/clip_image001.jpg)

-  

- CentOS-Base.repo 是yum 网络源的配置文件

- CentOS-Media.repo 是yum 本地源的配置文件

- 修改CentOS-Media.repo

- \# cat CentOS-Media.repo

- [------------------------------------------------------------------------------

- ------------------------------------------------------------------------]

- 在baseurl  中修改第2个路径为/mnt/cdrom（即为光盘挂载点）

- 将enabled=0改为1

- 3、禁用默认的yum 网络源

- 将yum  网络源配置文件改名为CentOS-Base.repo.bak，否则会先在网络源中寻找适合的包，改名之后直接从本地源读取。

- 4、执行yum 命令

- \# yum install postgresql

- ![img](file:///C:/Users/anzhy/AppData/Local/Temp/msohtmlclip1/02/clip_image002.jpg)

-  

- 源文档 <<http://www.cnblogs.com/mchina/archive/2013/01/04/2842275.html>> 

-  

-  

-  

- ![img](file:///C:/Users/anzhy/AppData/Local/Temp/msohtmlclip1/02/clip_image003.jpg)

-  

- 源文档 <<http://www.cnblogs.com/mchina/archive/2013/01/04/2842275.html>> 

-  

-  

- **关于****repo** **文件的格式**

- 所有repository 服务器设置都应该遵循如下格式：

- [serverid]
     name=Some name for this server
     baseurl=url://path/to/repository/

- - serverid 是用于区别各个不同的repository，必须有一个独一无二的名称；
  - name 是对repository 的描述，支持像$releasever $basearch这样的变量；
  - baseurl 是服务器设置中最重要的部分，只有设置正确，才能从上面获取软件。它的格式是：

- baseurl=url://server1/path/to/repository/
     url://server2/path/to/repository/
     url://server3/path/to/repository/

- 其中url  支持的协议有 http://  ftp:// [file://](file:///) 三种。baseurl 后可以跟多个url，你可以自己改为速度比较快的镜像站，但baseurl 只能有一个，也就是说不能像如下格式：

- baseurl=url://server1/path/to/repository/
     baseurl=url://server2/path/to/repository/
     baseurl=url://server3/path/to/repository/

- 其中url  指向的目录必须是这个repository header 目录的上一级，它也支持$releasever  $basearch 这样的变量。

- url 之后可以加上多个选项，如gpgcheck、exclude、failovermethod 等，比如：

-  

- [updates-released]
     name=Fedora Core $releasever - $basearch - Released Updates
      baseurl=http://download.atrpms.net/mirrors/fedoracore/updates/$releasever/$basearch
     <http://redhat.linux.ee/pub/fedora/linux/core/updates/$releasever/$basearch>
     <http://fr2.rpmfind.net/linux/fedora/core/updates/$releasever/$basearch>
     gpgcheck=1
     exclude=gaim
     failovermethod=priority

-  

- 其中gpgcheck，exclude  的含义和[main] 部分相同，但只对此服务器起作用，failovermethode 有两个选项roundrobin 和priority，意思分别是有多个url可供选择时，yum 选择的次序，roundrobin 是随机选择，如果连接失败则使用下一个，依次循环，priority 则根据url 的次序从第一个开始。如果不指明，默认是roundrobin。

- **五、配置国内****yum****源**

- 系统默认的yum 源速度往往不尽人意，为了达到快速安装的目的，在这里修改yum源为国内源。

- 上海交通大学yum源

- a. 修改/etc/yum.repos.d/CentOS-Base.repo为：

-  

- [base]
     name=CentOS-$releasever - Base
      \#mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=os
     baseurl=http://ftp.sjtu.edu.cn/centos/$releasever/os/$basearch/
     gpgcheck=1
     gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-5

- \#released updates 
     [updates]
     name=CentOS-$releasever - Updates
      \#mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=updates
     baseurl=http://ftp.sjtu.edu.cn/centos/$releasever/updates/$basearch/
     gpgcheck=1
     gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-5

- \#additional packages that may be useful
     [extras]
     name=CentOS-$releasever - Extras
      \#mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=extras
     baseurl=http://ftp.sjtu.edu.cn/centos/$releasever/extras/$basearch/
     gpgcheck=1
     gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-5

- \#additional packages that extend functionality of  existing packages
     [centosplus]
     name=CentOS-$releasever - Plus
      \#mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=centosplus
     baseurl=http://ftp.sjtu.edu.cn/centos/$releasever/centosplus/$basearch/
     gpgcheck=1
     enabled=0
     gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-5

- \#contrib - packages by Centos Users
     [contrib]
     name=CentOS-$releasever - Contrib
      \#mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=contrib
     baseurl=http://ftp.sjtu.edu.cn/centos/$releasever/contrib/$basearch/
     gpgcheck=1
     enabled=0
     gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-5

-  

- 关于变量

- - $releasever：代表发行版的版本，从[main]部分的distroverpkg获取，如果没有，则根据redhat-release包进行判断。
  - $arch：cpu体系，如i686,athlon等
  - $basearch：cpu的基本体系组，如i686和athlon同属i386，alpha和alphaev6同属alpha。

- b. 导入GPG KEY

- yum 可以使用gpg 对包进行校验，确保下载包的完整性，所以我们先要到各个repository 站点找到gpg key，一般都会放在首页的醒目位置，一些名字诸如RPM-GPG-KEY-CentOS-5 之类的纯文本文件，把它们下载下来，然后用rpm --import RPM-GPG-KEY-CentOS-5 命令将key 导入。

- c. 执行yum 命令

- ![img](file:///C:/Users/anzhy/AppData/Local/Temp/msohtmlclip1/02/clip_image004.jpg)

-  

-  

- 其他国内yum源列表如下：

- \1. 企业贡献：

- 搜狐开源镜像站：<http://mirrors.sohu.com/>

- 网易开源镜像站：<http://mirrors.163.com/>

- \2. 大学教学：

- 北京理工大学：

- [http://mirror.bit.edu.cn](http://mirror.bit.edu.cn/) (IPv4 only)

- [http://mirror.bit6.edu.cn](http://mirror.bit6.edu.cn/) (IPv6 only)

- 北京交通大学：

- [http://mirror.bjtu.edu.cn](http://mirror.bjtu.edu.cn/) (IPv4 only)

- [http://mirror6.bjtu.edu.cn](http://mirror6.bjtu.edu.cn/) (IPv6 only)

- [http://debian.bjtu.edu.cn](http://debian.bjtu.edu.cn/) (IPv4+IPv6)

- 兰州大学：<http://mirror.lzu.edu.cn/>

- 厦门大学：<http://mirrors.xmu.edu.cn/>

- 清华大学：

- <http://mirrors.tuna.tsinghua.edu.cn/> (IPv4+IPv6)

- <http://mirrors.6.tuna.tsinghua.edu.cn/> (IPv6 only)

- <http://mirrors.4.tuna.tsinghua.edu.cn/> (IPv4 only)

- 天津大学：<http://mirror.tju.edu.cn/>

- 中国科学技术大学：

- <http://mirrors.ustc.edu.cn/> (IPv4+IPv6)

- <http://mirrors4.ustc.edu.cn/>

- <http://mirrors6.ustc.edu.cn/>

- 东北大学：

- <http://mirror.neu.edu.cn/> (IPv4 only)

- <http://mirror.neu6.edu.cn/> (IPv6 only)

- 电子科技大学：<http://ubuntu.uestc.edu.cn/>

-  

- 源文档 <<http://www.cnblogs.com/mchina/archive/2013/01/04/2842275.html>> 

-  