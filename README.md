# php-coroutine (Beta PHP7 Only)
php的coroutine(协程\用户态线程)扩展 支持多层函数调用中yield


##使用方法
```php

function a()
{
  echo 1;
  Coroutine::yield();
  echo 3;
}

$co = new Coroutine(function(){
  a();
});
$co->resume();
echo 2;
$co->resume();

result:123
```
##Methods
###Coroutine::__construct($callable)

1. 传入函数名:$co = new Coroutine("run");
2. 传入类方法:$co = new Coroutine([$object,"methodName"]);
3. 传入闭包(Closure):$co = new Coroutine(function(){});

###Coroutine::resume()

开始\恢复 Coroutine运行;

###Coroutine::yield()

中断Coroutine,程序返回到调用 resume的点

###Coroutine::running()

返回当前协程实例

###Coroutine::reset();

当协程执行结束后,调用reset可以使协程回到刚创建时候的状态

##Properties
###$status

当前协程状态
Access:Public

###Constants
* STATUS_SUSPEND = 0;
* STATUS_RUNNING = 1;
* STATUS_DEAD = 2;

#注意
1. 当前版本不支持在协程中对另一个协程resume操作
2. 协程中会对register_shutdown_function的函数进行拦截,并在协程结束后调用
3. 不能在内部函数回调后 Coroutine->yield,必须等内部函数全部执行完后才能Coroutine->yield

#to be continued
* 第二版正在开发中,将会解决无法从call_user_func中yield的问题
