xmlhttp对象

 

var xmlhttp = new XMLHttpRequest();

 

xmlhttp.open(type,url,async)初始化对象

type：说明请求的类型，get或者post

url：目标url

async：是否异步模式，true时异步模式，请求发送后继续执行后续代码；false时同步模式，发送请求后，接到响应在继续执行后续代码。

例子：xmlhttp.open("get","info.txt",true)

 

 

属性readyState有以下5种状态

0   uninitialized 未初始化，对象已经创建，但是没有调用open方法

1   loading 载入中 open已经调用，但请求还没有发送

2   loaded 已载入 请求已经发送

3   interactive 交互中，已经接收到部分响应

4   complete 完成，所有数据已经收到，连接已经关闭

 

 

当状态发生变换时，会触发readystatechange事件，并调用onreadystatechange函数(回调函数，由用户自定义)。

xmlhttp.onreadystatechange=function(){

 

if(xmlhttp.readyState == 4){

alert("complete");

}

 

}

 

_blank：在新窗口中打开；

_self：默认。在相同的框架中打开；

_parent：在父框架集中打开；

_top：在整个窗口中打开；

 



http://jun1986.iteye.com/blog/1399242

 



http://blog.csdn.net/you23hai45/article/details/44629027