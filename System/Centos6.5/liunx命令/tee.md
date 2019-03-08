功能说明：读取标准输入的数据，并将其内容输出成文件。

 

语　　法：tee [-ai][--help][--version][文件...]

 

补充说明：tee指令会从标准输入设备读取数据，将其内容输出到标准输出设备，同时保存成文件。

 

参　　数：

-a或--append 　附加到既有文件的后面，而非覆盖它．

-i-i或--ignore-interrupts 　忽略中断信号。

--help 　在线帮助。

--version 　显示版本信息。 

 

 

[root@localhost ~]# who | tee who.out

root     pts/0        2009-02-17 07:47 (123.123.123.123)

[root@localhost ~]# cat who.out

root     pts/0        2009-02-17 07:47 (123.123.123.123)

[root@localhost ~]# pwd | tee -a who.out

/root

[root@localhost ~]# cat who.out

root     pts/0        2009-02-17 07:47 (123.123.123.123)

/root

[root@localhost ~]# 

 

[root@anzhy home]# tee hello.txt <<-'EOF'

\> asdfjk

\> fasd

\> sadf

\> asdf

\> asdf

\> asdf

\> asdf

\> qwer

\> EOF

asdfjk

fasd

sadf

asdf

asdf

asdf

asdf

qwer

[root@anzhy home]# 

[root@anzhy home]# more hello.txt 

asdfjk

fasd

sadf

asdf

asdf

asdf

asdf

qwer

[root@anzhy home]# 