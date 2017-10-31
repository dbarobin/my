启动的入口，位于public/index.php代码中。  

```php

$app = require_once __DIR__.'/../bootstrap/app.php';

//始初化的一个核心方法。
```

```php
$app = new Illuminate\Foundation\Application(
    //data/httpd/laravel
    realpath(__DIR__.'/../')
);
//这里直接文件作return了，奇怪的用法
```

```php
//new Illuminate\Foundation\Application的构造方法，具体做了什么事情？
public function __construct($basePath = null)
{
    //这个时候我的basePath是：/data/httpd/laravel
    if ($basePath) {//所以开始设置
        $this->setBasePath($basePath);
    }
    $this->registerBaseBindings();
    $this->registerBaseServiceProviders();
    $this->registerCoreContainerAliases();
}
```

```php
public function setBasePath($basePath)
{
    $this->basePath = rtrim($basePath, '\/');
    $this->bindPathsInContainer();//注意此方法

    return $this;
}
```

```php
//内容非常多,应该是所有的配置文件了。
protected function bindPathsInContainer()
{
    $this->instance('path', $this->path());///data/httpd/laravel/app
    $this->instance('path.base', $this->basePath());///data/httpd/laravel
    $this->instance('path.lang', $this->langPath());///data/httpd/laravel/resources/lang
    $this->instance('path.config', $this->configPath());///data/httpd/laravel/config
    $this->instance('path.public', $this->publicPath());///data/httpd/laravel/public
    $this->instance('path.storage', $this->storagePath());///data/httpd/laravel/storage
    $this->instance('path.database', $this->databasePath());///data/httpd/laravel/database
    $this->instance('path.resources', $this->resourcePath());///data/httpd/laravel/resources
    $this->instance('path.bootstrap', $this->bootstrapPath());///data/httpd/laravel/bootstrap
}
```
`$this->instance`的含义  

```php
public function instance($abstract, $instance)
{
    $this->removeAbstractAlias($abstract);//先移除实例，这个没什么好说的。注意这里是移除$this->abstractAliases[]里面的数据

    $isBound = $this->bound($abstract);//检测是否被绑定？上面不是移除了么。

    unset($this->aliases[$abstract]);

    // We'll check to determine if this type has been bound before, and if it has
    // we will fire the rebound callbacks registered with the container and it
    // can be updated with consuming classes that have gotten resolved here.
    $this->instances[$abstract] = $instance;//并成实例数组

    if ($isBound) {//如果绑定存在的话进行重绑定？
        $this->rebound($abstract);
    }
}
```
里面有两个比较重要的数组：`$this->instances`和`$this->aliases`  
代码中有些意义不明确，比如检查到有绑定就去重新绑定？  

```php
protected function rebound($abstract)
{
    //第一次执行的时候查到了request的参数。
    $instance = $this->make($abstract);
    //这里的callback是一个对象。
    foreach ($this->getReboundCallbacks($abstract) as $callback) {
        call_user_func($callback, $this, $instance);
    }
}
```
其中的`$this->reboundCallbacks`参数还不明确。  

```php
$kernel = $app->make(Illuminate\Contracts\Http\Kernel::class);
```

调用`$app`中的方法：  
```php

```
