# Notice
if in **WEB server** and want to do something  **async**
 pls consider **fastcgi_finish_request()**



#



# Async
fork from  ```muyizixiu/Async```  , (fix small bugs)


```
php 异步任务库
一个快速响应的web应用必然有很多繁重的异步任务去做，Async利用pnctl_fork创建异步进程，同时管理异步任务。
```
detai: https://github.com/muyizixiu/Async



## Install
```
composer install sanderswang/async
```



## Usage 

#### common async task: 
```
use Async/Async;
$async = new Async($redis_host,$redis_port,$redis_password,'/tmp/async);
$async->task(function($data){
    echo 'common Async' . "$data\n";
    doSomething();
}, 'common task');
```
do once, and then exit


####

#### domain and queue task: 

 domain.php
```
use Async/Async;
$a = new Async($redis_host,$redis_port,$redis_password,'/tmp/async');
//当任务不存在时创建任务
if(!$a->isTaskExists($task_name)){
    $a->task(function($data){
        echo 'hello Async'."$data\n";
        doSomething();
    },$task_name,true,true,'123');
}
```


a.php: 
```
$a = new Async($redis_host,$redis_port,$redis_password,'/tmp/async');
for($i = 0;$i < 10;$i ++){
    sleep(2);
    $a->sendData($task_name, "这是我第$i个数据");
}
```
以上代码模拟定时任务。
该异步进程一旦启动则常驻，不会每隔10秒启动一个异步进程，而是在同一个进程里面，不停的接受投递过来的参数。

## 依赖
本库依赖redis实现进程管理,需安装redis扩展，并提供账号和密码。
