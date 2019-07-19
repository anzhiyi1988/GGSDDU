下载

[root@master home]# wget http://www.python.org/ftp/python/2.7.14/Python-2.7.14.tar.xz

 

解压

[root@master home]# unxz Python-2.7.14.tar.xz 

解压

[root@master home]# tar -vxf Python-2.7.14.tar 

 

设置

[root@master Python-2.7.14]# ./configure --enable-shared --enable-loadable-sqlite-extensions --with-zlib

 

 

[root@master Python-2.7.14]# cd Modules/

[root@master Modules]# vi Setup

\#zlib zlibmodule.c -I$(prefix)/include -L$(exec_prefix)/lib -lz  把这个打开

 

编译

[root@master Python-2.7.14]# make

make: *** [Modules/zlibmodule.o] Error 1

编译报错了，这个时候就安装zlib组件，要安装两个 zlib 和zlib-devel   （可以换yum源安装，也可以下载安装）

 

 

再次编译

[root@master Python-2.7.14]# make

Python build finished, but the necessary bits to build these modules were not found:

_curses            _curses_panel      _sqlite3        

_ssl               _tkinter           bsddb185        

bz2                dl                 imageop         

readline           sunaudiodev                        

To find the necessary bits, look in setup.py in detect_modules() for the module's name.

上面的错误就是说少依赖，那就安装就行了，一般要安装如下依赖

readline-devel，sqlite-devel，bzip2-devel.i686，openssl-devel.i686，gdbm-devel.i686，libdbi-devel.i686，ncurses-libs，zlib-devel.i686

安装后再make到剩下这几个的时候就可以了

Python build finished, but the necessary bits to build these modules were not found:

_tkinter           bsddb185           dl              

imageop            sunaudiodev     

 

 

安装

[root@master Python-2.7.14]# make install

 

没有变，卧槽，让他变

[root@master Python-2.7.14]# python -V

Python 2.6.6

编辑

[root@master ~]# vi /etc/ld.so.conf

include ld.so.conf.d/*.conf

/usr/local/lib 增加这行

 

[root@master ~]# /sbin/ldconfig

[root@master ~]# /sbin/ldconfig -V

ldconfig (GNU libc) 2.12

Copyright (C) 2010 Free Software Foundation, Inc.

This is free software; see the source for copying conditions.  There is NO

warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.

Written by Andreas Jaeger.

[root@master ~]# python -V        

Python 2.7.14

 

安装pip

[root@master home]# wget https://bootstrap.pypa.io/get-pip.py --no-check-certificate

[root@master home]# python get-pip.py 

[root@master home]# ln -s /usr/local/bin/pip2.7 /usr/bin/pip

ln: creating symbolic link `/usr/bin/pip': File exists

[root@master bin]# rm pip

rm: remove regular file `pip'? yes

[root@master bin]# cd /home

[root@master home]# ln -s /usr/local/bin/pip2.7 /usr/bin/pip

 

安装必要

[root@master home]# pip install scrapy

 

 

安装faker

[root@master datasource]# pip install fake-factory==0.7.18

Collecting fake-factory==0.7.18

  Could not find a version that satisfies the requirement fake-factory==0.7.18 (from versions: 0.5.6-proper, 0.2, 0.3, 0.3.1, 0.3.2, 0.4.0, 0.4.2, 0.5.0, 0.5.1, 0.5.2, 0.5.3, 0.5.4, 0.5.5, 0.5.6, 0.5.7, 0.5.8, 0.5.9, 0.5.10, 0.5.11, 0.6.0, 0.7.2, 0.7.4, 9999.9.9)

No matching distribution found for fake-factory==0.7.18

[root@master datasource]# pip install fake-factory==0.7.4

 

 

 