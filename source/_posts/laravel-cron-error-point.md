---
title: Laravel Cron 定时任务“跳坑”点
date: 2018-10-15 12:36:39
tags: [laravel,crontab]
typora-copy-images-to: laravel-cron-error-point
---

Laravel 中执行定时任务是通过 cron 来实现，官网文档中就是简单一句 + 一行Cron 代码

```
* * * * * php /path-to-your-project/artisan schedule:run >> /dev/null 2>&1
```

但是在实际使用的过程中，如果对 Linux 和 Cron 不熟悉，会遇到一些小坑，我们整理并记录了分享出来希望能帮助到大家。

## 坑1：环境变量

当`Cron`无法生效时，可能是`Cron`执行环境变量不正确引起的。

#### 执行命令

```
env > /tmp/env.output
```

打开`/tmp/env.output`文件，将`PATH`字段整行添加至`corntab`文件顶部，`corntab`文件在`/var/spool/cron`目录下l

#### `crontab` 文件示例
``` 
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/opt/mysql/bin:/opt/php7/bin:/opt/memcached/bin:/root/bin
* * * * * php /path-to-your-project/artisan schedule:run >> /dev/null 2>&1
```

## 坑2：Cron 执行用户导致 Laravel log 不可写

通过 `crontab -e` 命令创建的  `Cron` 是属于 `root` 用户，如果定时任务在实行时主动写入日志或者遇到异常未捕捉，会创建 root 权限的日志文件，最终会导致 `php-fpm` 的  `www` 账号无法写入。

因此需要在创建 cron 的时候指定用户  
```
crontab -u www -e
```

> 个人管理的系统中 php-fpm 用户都是 www，请根据自己的实际情况调整。

## 坑3：cron 内容最后一行未回车

解决上述两点问题后，如果仍然发现 cron 不执行，请确认
```
* * * * * php /path-to-your-project/artisan schedule:run >> /dev/null 2>&1

```
代码最后有进行回车换行。

这个坑坑了工程师一个下午 : (


> 小广告一波：
> Laravel & VUE & 小程序 & 电商产品 交流群：674454674 暗号：segmentfault



