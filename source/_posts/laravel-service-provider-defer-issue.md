---
title: Laravel Service Provider 开发时设置延迟加载时遇到的问题
date: 2018-10-18 08:04:51
tags: [laravel, ServiceProvider]
categories: [Laravel-tips]
---

### 发现问题

因实际项目需求，近日在开发 [laravel-database-logger][1] 包的时候，发现设置 ServiceProvider `defer` 属性设置为 `true` 时，会导致在 `register` 方法中注册的 `middleware` 无效。

```php
class ServiceProvider extends \Illuminate\Support\ServiceProvider
{
     protected $defer = true;
     
     public function register()
     {
        $this->mergeConfigFrom(
            __DIR__ . '/../config/config.php', 'ibrand.dblogger'
        );


        $this->app->singleton(DbLogger::class, function ($app) {
            return new DbLogger();
        });

        //当 $defer 设置为 true 时，在路由中引用 databaselogger middleware 会报错，提示 databaselogger class not found.
        $this->app[\Illuminate\Routing\Router::class]->middleware('databaselogger', Middleware::class);

    }
    
    public function provides()
    {
        return [DbLogger::class];
    }
}

```

当问题出现的时候就怀疑是因为设置了 `defer` 属性设置为 `true` 导致的，立刻就修改源码把 `protected $defer = true;` 的代码注释掉，结果仍然是提示 `databaselogger class not found.`，说明 Laravel 并没有注册此 `ServiceProvder`

接下来就是想如何解决此问题，尝试了下面的方法：

**1. 验证本身代码是否存在问题**

在正常注册的 `AppServiceProvider` 中注册自己的 `ServiceProvider`

```
public function register()
    {
        //
        $this->app->register(\Ibrand\DatabaseLogger\ServiceProvider::class);
    }
```
注册后结果一切正常。

**2. 研究源码**
在 `config/app.php` 中 `providers` 注册无效，但是在其他 `ServiceProvider` 中注册有效，说明是其他问题。

通过研究 `Illuminate\Foundation\Application` 源码找到 `registerConfiguredProviders` 方法：

Laravel 是在此方法中去读取 `config/app.php` 中的 `providers` 内容并`load`到 `ProviderRepository` 中。

```php
(new ProviderRepository($this, new Filesystem, $this->getCachedServicesPath()))
                    ->load($providers->collapse()->toArray());
```

重点在 `$this->getCachedServicesPath()` ,通过源码发现 Laravel 是根据 `bootstrap/cache/services.php` 文件去决定如何注册 `ServiceProvider`。

此时想到了为什么之前注释了 `//protected $defer = true;` 代码后仍然无效的原因。

所以为了让注释后的 `//protected $defer = true;` 代码有效需要执行
```
php artisan clear-compiled 
php artisan optimize
```
之后问题就解决了，也更加深入理解了 ServiceProvider 的原理。

> 所以切记：如果准备采用延迟加载`ServiceProvider`时，严禁进行注册 middleware, route 等系列操作。同时，更改 `defer` 属性值后，需要执行 `php artisan clear-compiled ` 和 `php artisan optimize` 以更新 ServiceProvider 缓存。
 
**3. 为什么 AppServiceProvider 中注册有效？**

愿意很简单，因为 `AppServiceProvider` 并没有延迟加载，因此在执行 `AppServiceProvider` 中 `register` 方法去注册新的 `ServiceProvider` 也是不会延迟加载的。

### 总结

1. 谨慎使用延迟加载 `ServiceProvider`
2. 更改 `defer` 属性值后，需要执行 `php artisan clear-compiled ` 和 `php artisan optimize` 以更新 ServiceProvider 缓存。
3. 严禁在延迟加载的 `ServiceProvider` 注册 `middleware` 和 `route` 。

  [1]: https://github.com/guojiangclub/laravel-database-logger/