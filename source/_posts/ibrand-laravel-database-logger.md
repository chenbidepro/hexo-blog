---
title: Laravel Database Logger SQL执行分析工具包
date: 2018-10-21 08:11:32
tags: [laravel,database,logger,laravel-package]
categories: [Laravel-packages]
---

iBrand 社交新零售电商产品从2016年9月启动至今，已经趋于稳定，而且已经初步得到市场的检验，特别能抗住电商中秒杀时高并发的交易场景。

接下来我们团队会逐步开源一些正在使用的工具和解决方案，并且会开源电商产品代码，欢迎大家关注我们 iBrand 产品。

今天介绍的是我们在实际应用场景中使用的一个小功能包 [Laravel database logger][1] ，求 Star : )

### Why

1. iBrand 是一个电商 + 新零售的交易类产品，所以对金额数据比较敏感。对于后台管理的操作需要进行操作日志，主要用于追踪操作记录。
2. iBrand 产品包含 H5微商城（VUE），小程序，导购小程序端，因此是前后端完全分离的，在这种情况下，没有一个跟踪分析 API SQL 执行效率的工具。特别是后期需求越来越复杂，使用 Laravel Eloquent ORM 是非常方便，但也容易造成性能问题。而 Laravel debugger 只适用于 web 应用。因此需要个工具来分析每个请求产生的 SQL 执行语句和执行效率。


### Feature

1. 日志文件区分匿名用户和 Guard.
2. 记录执行用户
3. 记录 request url
4. 支持记录指定 SQL 语句类型(SELECT,INSET INTO,UPDATE,DELETE,ALTER TABLE etc.)
5. 单独记录 slow sql.

### 安装

```
composer require ibrand/laravel-database-logger:~1.0 -vvv
```

**低于 Laravel5.5 版本**

在 `config/app.php` 文件中 'providers' 添加

```
iBrand\DatabaseLogger\ServiceProvider::class
```

`php artisan vendor:publish --provider="iBrand\DatabaseLogger\ServiceProvider" `


### 使用

1. add `databaselogger` middleware to route.
2. set `log_queries=>true` in `config/ibrand/dblogger.php` file. or set  `DB_LOG_QUERIES = true` in `.env` file.

### 效果

![9459](https://cdn.chenhow.com/ibrand-laravel-database-logger/1.png)

![9462](https://cdn.chenhow.com/ibrand-laravel-database-logger/2.png)

![9465](https://cdn.chenhow.com/ibrand-laravel-database-logger/3.png)

> 欢迎大家 star 和提交 issue   :)

[1]: https://github.com/ibrandcc/laravel-database-logger
