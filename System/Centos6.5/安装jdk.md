**查看已经安装的****jdk****路径**

\#whereis java

\#which java （java执行路径）

\#echo $JAVA_HOME

\#echo $PATH

 

 

**安装****jdk**

把下载的jdk-6u11-linux-i586.bin复制到要安装的路径下

需改为可执行文件 # chmod 755 jdk-6u11-linux-i586.bin

设置用户变量

[pgsql@anzhy home]$ cd ~         <<-- 切换到用户目录

[pgsql@anzhy ~]$ more .bash_profile

export JAVA_HOME=/home/jdk1.8.0_171

export PATH=$PATH:$JAVA_HOME/bin

export CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar

 

立即执行 .空格.bash_profile

 

 

 测试 java –version

 

 

**修改系统默认****java**

**update-alternatives**

 

 

[root@master ~]# alternatives --install /usr/bin/java java /usr/java/jdk1.8.0_60/bin/java 300
 [root@master ~]# alternatives --config java

There are 2 programs which provide 'java'.

Selection    Command
 \-----------------------------------------------
 *+ 1           /usr/lib/jvm/jre-1.5.0-gcj/bin/java
    2           /usr/java/jdk1.8.0_60/bin/java

Enter to keep the current selection[+], or type selection number: 2

 

 

 <<http://blog.csdn.net/u011364306/article/details/48375653>> 

 

 

 

 