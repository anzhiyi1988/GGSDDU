https://blog.csdn.net/wangshuminjava/article/details/84983818

## **服务器性能监控工具软件Nmon和ServerAgent对比**

| 软件 | **Nmon+nmon_analyser**                                       | **ServerAgent（Jemeter）**                                   |
| ---- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 范围 | AIX与各种Linux操作系统                                       | Windows和Linux操作系统                                       |
| 安装 | 1、 下载Nmon+nmon_analyserhttp://download.csdn.net/download/zwliu6/101937422、 将Nmon部署到服务器上（拷贝过去）3、 运行Nmon文件（需要设置可执行）4、 将产生文件导出，用nmon_analyser打开 | 1、下载ServerAgent和Jmeterhttp://download.csdn.net/download/zwliu6/101938022、将ServerAgent部署到服务器上（拷贝过去）3、运行startAgent.sh文件（需要设置可执行）4、 安装匹配的Jmeter，设置运行 |
| 范围 | ● cpu占用率<br>●内存使用情况●磁盘I/O速度、传输和读写比率●文件系统的使用率●网络I/O速度、传输和读写比率、错误统计率与传输包的大小●消耗资源最多的进程●计算机详细信息和资源●页面空间和页面I/O速度●用户自定义的磁盘组●网络文件系统等等 | ● cpu占用率●Memory内存使用情况●Swap 交换分区<br>●Disk I/O磁盘I/O速度、传输和读写比率●Network I/O网络I/O速度、传输和读写比率、错误统●TCP●JMX Java管理扩展●ENEC●TALL |
| 进程 | 可以                                                         | 可以                                                         |
| 使用 | 使用命令在Linux上设置监控内容及方式http://blog.csdn.net/linabc123000/article/details/70833427 | 直接在Jmeter上设置监控内容及方式https://jingyan.baidu.com/article/7908e85cbaa401af481ad285.html |
| 分析 | 导出的静态图表分析和命令动态观察                             | 可以在Jmeter上动态观察及静态图分析                           |
| 结论 | 推荐：Linux：Nmon  Windows:ServerAgent                       |                                                              |





