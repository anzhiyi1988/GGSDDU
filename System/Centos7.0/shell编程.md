#### $!

代表最后执行的后台命令的PID

#### $?

代表上一个命令执行是否成功的标志，如果执行成功则$? 为0，否则不为0





####  >/dev/null 2>&1

先说一下[**Linux**](http://lib.csdn.net/base/linux)重定向：

0、1和2分别表示标准输入、标准输出和标准错误信息输出，可以用来指定需要重定向的标准输入或输出。

在一般使用时，默认的是标准输出，既1.当我们需要特殊用途时，可以使用其他标号。例如，将某个程序的错误信息输出到log文件中：./program 2>log。这样标准输出还是在屏幕上，但是错误信息会输出到log文件中。

另外，也可以实现0，1，2之间的重定向。2>&1：将错误信息重定向到标准输出。

Linux下还有一个特殊的文件/dev/null，它就像一个无底洞，所有重定向到它的信息都会消失得无影无踪。这一点非常有用，当我们不需要回显程序的所有信息时，就可以将输出重定向到/dev/null。

如果想要正常输出和错误信息都不显示，则要把标准输出和标准错误都重定向到/dev/null， 例如：

\# ls 1>/dev/null 2>/dev/null

还有一种做法是将错误重定向到标准输出，然后再重定向到 /dev/null，例如：

\# ls >/dev/null 2>&1

注意：此处的顺序不能更改，否则达不到想要的效果，此时先将标准输出重定向到 /dev/null，然后将标准错误重定向到标准输出，由于标准输出已经重定向到了/dev/null，因此标准错误也会重定向到/dev/null，于是一切静悄悄:-)

由于使用nohup时，会自动将输出写入nohup.out文件中，如果文件很大的话，nohup.out就会不停的增大，这是我们不希望看到的，因此，可以利用/dev/null来解决这个问题。

nohup ./program >/dev/null 2>log &

如果错误信息也不想要的话：

nohup ./program >/dev/null 2>&1 &

 

来自 <<http://blog.csdn.net/geekster/article/details/6657620>> 





# 执行sql语句

```shell
#!/bin/sh
export PGPASSWORD=xyz123

cmd="psql -h172.16.34.81 -p 5432 -U gpadmin -d jcw_ztk_kf "
sql=" select c_sjkip||','||c_dkh||','||c_yhm||','||c_mm||','||c_sjk ||','|| c_sjkljjc as r  from db_ztk.t_sjkjcxx_pzb "
sql3="./home/gp5/gp428/bin/psql -h172.16.34.132 -p 5432 -U postgres -d db_dp_qy -c  'show max_connections'"
echo $rrr
dbs=$(  $cmd -c "$sql")
len=$(  $cmd -c "$sql" | wc -l )
let len-=1
i=1
for line in $dbs
do
    if [ $i -gt 2 ] && [ $i -lt $len ] ;then
        echo "$i line: $line"
        IFS=,
        arr=($line)
        sjkip=${arr[0]}
        dkh=${arr[1]}
        yhm=${arr[2]}
        mm=${arr[3]}
        sjk=${arr[4]}
        sjkjjc=${arr[5]}


        sql1=" show max_connections "
        export PGPASSWORD=$mm
        data=$( psql -h$sjkip -p $dkh -U $yhm -d $sjk -c $sql1 )
        echo $data
        echo $( $data | wc -l )
    else
        echo "Don't need ! "
    fi
    let i+=1
done
```





# 备份文件夹

```shell
#!/bin/sh

date=$(date +%Y_%m_%d)
sourceDir="/opt/thunisoft/jdgtyypt/data/wjcs/zip"
bakDir="/home/anzhy/jdgtyypt/backup"

if [ -d $sourceDir ];
then
    if [ -d $bakDir/$date ];
    then    
        echo "$(date) begin backup -------------"
        echo "$(date) cp -R $sourceDir $bakDir/$date"
        $(cp -R $sourceDir $bakDir/$date)
        echo "$(date) backup finished ----------"
    else
        echo "$(date) not found [$bakDir/$date], begin to create, command:[ mkdir -p $bakDir/$date] "
        $( mkdir -p $bakDir/$date )
        if [ -d $bakDir/$date ];
        then
            echo "$(date) [$bakDir/$date] was  created"
            echo "$(date) begin backup -------------"
            echo "$(date) cp -R $sourceDir $bakDir/$date"
            $(cp -R $sourceDir $bakDir/$date)
            echo "$(date) backup finished ----------"

        else
            echo "$(date) create [$bakDir/$date] was  failed, backup stop !!!"
        fi

    fi
else
    echo "$(date) dir [$sourceDir] not found! backup stop!" 
fi

```





```sh
0 0 * * * /home/anzhy/xxx.sh >> /home/anzhy/backup.log
```

