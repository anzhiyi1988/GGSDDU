常用参数：

文件/目录判断：

[ -a FILE ] 如果 FILE 存在则为真。

[ -b FILE ] 如果 FILE 存在且是一个块文件则返回为真。

[ -c FILE ] 如果 FILE 存在且是一个字符文件则返回为真。

[ -d FILE ] 如果 FILE 存在且是一个目录则返回为真。

[ -e FILE ] 如果 指定的文件或目录存在时返回为真。

[ -f FILE ] 如果 FILE 存在且是一个普通文件则返回为真。

[ -g FILE ] 如果 FILE 存在且设置了SGID则返回为真。

[ -h FILE ] 如果 FILE 存在且是一个符号符号链接文件则返回为真。（该选项在一些老系统上无效）

[ -k FILE ] 如果 FILE 存在且已经设置了冒险位则返回为真。

[ -p FILE ] 如果 FILE 存并且是命令管道时返回为真。

[ -r FILE ] 如果 FILE 存在且是可读的则返回为真。

[ -s FILE ] 如果 FILE 存在且大小非0时为真则返回为真。

[ -u FILE ] 如果 FILE 存在且设置了SUID位时返回为真。

[ -w FILE ] 如果 FILE 存在且是可写的则返回为真。（一个目录为了它的内容被访问必然是可执行的）

[ -x FILE ] 如果 FILE 存在且是可执行的则返回为真。

[ -O FILE ] 如果 FILE 存在且属有效用户ID则返回为真。

[ -G FILE ] 如果 FILE 存在且默认组为当前组则返回为真。（只检查系统默认组）

[ -L FILE ] 如果 FILE 存在且是一个符号连接则返回为真。

[ -N FILE ] 如果 FILE 存在 and has been mod如果ied since it was last read则返回为真。

[ -S FILE ] 如果 FILE 存在且是一个套接字则返回为真。

[ FILE1 -nt FILE2 ] 如果 FILE1 比 FILE2 新, 或者 FILE1 存在但是 FILE2 不存在则返回为真。

[ FILE1 -ot FILE2 ] 如果 FILE1 比 FILE2 老, 或者 FILE2 存在但是 FILE1 不存在则返回为真。

[ FILE1 -ef FILE2 ] 如果 FILE1 和 FILE2 指向相同的设备和节点号则返回为真。

 

 

字符串判断

[ -z STRING ] 如果STRING的长度为零则返回为真，即空是真

[ -n STRING ] 如果STRING的长度非零则返回为真，即非空是真

[ STRING1 ]　 如果字符串不为空则返回为真,与-n类似

[ STRING1 == STRING2 ] 如果两个字符串相同则返回为真

[ STRING1 != STRING2 ] 如果字符串不相同则返回为真

[ STRING1 < STRING2 ] 如果 “STRING1”字典排序在“STRING2”前面则返回为真。

[ STRING1 > STRING2 ] 如果 “STRING1”字典排序在“STRING2”后面则返回为真。

 

 

数值判断

[ INT1 -eq INT2 ] INT1和INT2两数相等返回为真 ,=

[ INT1 -ne INT2 ] INT1和INT2两数不等返回为真 ,<>

[ INT1 -gt INT2 ] INT1大于INT2返回为真 ,>

[ INT1 -ge INT2 ] INT1大于等于INT2返回为真,>=

[ INT1 -lt INT2 ] INT1小于INT2返回为真 ,<

[ INT1 -le INT2 ] INT1小于等于INT2返回为真,<=

 

 

逻辑判断

[ ! EXPR ] 逻辑非，如果 EXPR 是false则返回为真。

[ EXPR1 -a EXPR2 ] 逻辑与，如果 EXPR1 and EXPR2 全真则返回为真。

[ EXPR1 -o EXPR2 ] 逻辑或，如果 EXPR1 或者 EXPR2 为真则返回为真。

[ ] || [ ] 用OR来合并两个条件

[ ] && [ ] 用AND来合并两个条件

 

 

其他判断

[ -t FD ] 如果文件描述符 FD （默认值为1）打开且指向一个终端则返回为真

[ -o optionname ] 如果shell选项optionname开启则返回为真

 

 

IF高级特性：

双圆括号(( ))：表示数学表达式

在判断命令中只允许在比较中进行简单的算术操作，而双圆括号提供更多的数学符号，而且在双圆括号里面的'>','<'号不需要转意。

 

双方括号[[ ]]：表示高级字符串处理函数

双方括号中判断命令使用标准的字符串比较，还可以使用匹配模式，从而定义与字符串相匹配的正则表达式。

 

双括号的作用：

在shell中，[ $a != 1 || $b = 2 ]是不允许出，要用[ $a != 1 ] || [ $b = 2 ]，而双括号就可以解决这个问题的，[[ $a != 1 || $b = 2 ]]。又比如这个[ "$a" -lt "$b" ]，也可以改成双括号的形式(("$a" < "$b"))

 

 

 

bash shell 脚本的方法有多种，现在作个小结。假设我们编写好的shell脚本的文件名为hello.sh，文件位置在/data/shell目录中并已有执行权限。

方法一：切换到shell脚本所在的目录（此时，称为工作目录）执行shell脚本：

复制代码 代码如下:

cd /data/shell

./hello.sh

 

./的意思是说在当前的工作目录下执行hello.sh。如果不加上./，bash可能会响应找到不到hello.sh的错误信息。因为目前的工作目录（/data/shell）可能不在执行程序默认的搜索路径之列,也就是说，不在环境变量PASH的内容之中。查看PATH的内容可用 echo $PASH 命令。现在的/data/shell就不在环境变量PASH中的，所以必须加上./才可执行。

方法二：以绝对路径的方式去执行bash shell脚本：

复制代码 代码如下:

/data/shell/hello.sh

 

方法三：直接使用bash 或sh 来执行bash shell脚本：

复制代码 代码如下:

cd /data/shell

bash hello.sh

 

或

复制代码 代码如下:

cd /data/shell

sh hello.sh

注意，若是以方法三的方式来执行，那么，可以不必事先设定shell的执行权限，甚至都不用写shell文件中的第一行（指定bash路径）。因为方法三是将hello.sh作为参数传给sh(bash)命令来执行的。这时不是hello.sh自己来执行，而是被人家调用执行，所以不要执行权限。那么不用指定bash路径自然也好理解了啊，呵呵……。

方法四：在当前的shell环境中执行bash shell脚本：

复制代码 代码如下:

cd /data/shell

. hello.sh

 

或

复制代码 代码如下:

cd /data/shell

source hello.sh

前三种方法执行shell脚本时都是在当前shell（称为父shell）开启一个子shell环境，此shell脚本就在这个子shell环境中执行。shell脚本执行完后子shell环境随即关闭，然后又回到父shell中。而方法四则是在当前shell中执行的

 

来自 <<http://www.jb51.net/article/53924.htm>> 

 

 