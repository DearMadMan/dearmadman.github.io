title: laravel 5.1 events
date: 2015-08-12 06:08:33
categories: laravel
tags: laravel
---
![laravel](/images/dj.jpeg)
今天研究了一下laravel的事件驱动，发现使用起来非常方便，来来~一起嗨起来~.
这里只介绍一些简单的使用方法~  队列驱动以及广播分发以后使用到了再研究
# Laravel Events

## 简介

laravel的events提供了一些简单的观察者类实践,允许我们注册一些事件，并且允许我们注册一些事件监听者去监听这些事件，
当我们触发某些事件时，相应的监听者将会触发handle方法。在这里我们可以处理一些相应的业务逻辑。

## 注册

laravel 提供了便利的生成`事件注册`与`事件监听者注册`的机制,允许我们通过`EventServiceProvider`类中以键值对的形式注册`事件`及`事件监听者`:
	```php
	    /**
	     * The event listener mappings for the application.
	     *
	     * @var array
	     */
	    protected $listen = [
	        'App\Events\UserLoginEvent' => [
	            'App\Listeners\UserLoginEventListener', 
	        ],
	        'App\Events\ManagerCreateEvent'=>[
	            'App\Listeners\ManagerCreateEventListener',
	            'App\Listeners\SomeOtherEventListener',
	        ]
	    ];	
	```
	
上面的代码注册了名为`UserLoginEvent`的事件及名为`UserLoginEventListener`的`UserLoginEvent`事件监听者,`ManagerCreateEvent`事件，监听`ManagerCreateEvent`事件的两个监听者：`ManagerCreateEventListener` And `SomeOtherEventListener`
ok，这样我们就已经完成了基本的事件及监听者的注册，但是还没有对类进行开发，让我们来通过强大的artisan 进行生成：
```bash
	php artisan event:generate
```
通过命令会生成相应已注册的类到相应的文件中，Event会生成在`App\Events`目录中，Listener会生成在`App\Listeners`中
这样会发现如下的目录结构信息：
```bash
	app\
		Events\
				UserLoginEvent.php
				ManagerCreateEvent.php
		Listeners\
				ManagerCreateEventListener.php
				SomeOtherEventListener.php  # 比如邮件推送
```


在生成的事件类中，我们可以在`__constrcut`构造函数中注入相应的依赖，也可以在其中做一些业务处理，当触发事件类时，即`__constrct`完成后，相应的监听者将会按顺序的触发handle函数 


## 触发

触发事件的方法很简单，在你需要触发相应事件的地方进行触发操作就行了
```php
	 Event::fire(new ManagerCreateEvent($manager));
	 # or
	 event(new ManagerCreateEvent($manager));
```


事件触发的顺序是这样的：
```bash
	ManagerCreateEvent: __construct
	ManagerCreateEventListener: handle
	SomeOtherEventListener: handle
```

这个过程中是可以终止监听的 比如说 在`managerCreateEventListener`的`handle`方法中 `return false ;` 下序的`SomeOtherEventListener`将不再触发监听操作。


### Subscribe 关注/订阅 事件

laravel的Subscribe机制允许我们在事件监听者自身类中监听多个事件，只要我们在监听者类中实现了subscribe方法,例如：
```php
class UserEventHandler {

    /**
     * 处理用户登录事件。
     */
    public function onUserLogin($event)
    {
        //
    }

    /**
     * 处理用户注销事件。
     */
    public function onUserLogout($event)
    {
        //
    }

    /**
     * 注册监听器给订阅者。
     *
     * @param  Illuminate\Events\Dispatcher  $events
     * @return array
     */
    public function subscribe($events)
    {
        $events->listen('App\Events\UserLoggedIn', 'UserEventHandler@onUserLogin');

        $events->listen('App\Events\UserLoggedOut', 'UserEventHandler@onUserLogout');
    }

}

```

上面的`UserEventHandle`类监听了两个不同的事件类`UserLoggedIn`和`UserLoggedOut`,当触发UserLoggendIn或者UserLoggedOut时，会自动触发相应的监听方法。

只定义类和监听方法还是不行的，我们需要告诉系统我们需要注册subscribe形式的监听,在`EventServiceProvider`中添加：

```php
protected $subscribe = [
        'App\Handlers\Events\UserEventHandler',
    ];
```


执行顺序，laravel会优先执行Listen中的事件监听者，而后执行subscribe方式的监听者



^_^ 