---
title: Laravel 开源电商体验与部署
date: 2018-12-14 06:02:21
tags: [laravel,开源电商]
categories: [开源电商]
---

## 体验

开源项目已经部署了体验环境，开源通过扫描下方小程序码进行体验：

![iBrand开源社交电商](https://iyoyo.oss-cn-hangzhou.aliyuncs.com/post/miniprogramcode/1.jpg)

  我们部署了 Laravel API demo 环境，访问地址：https://demo-open-admin.ibrand.cc/ , 访问默认是 Laravel 的欢迎页面，可通过 [API 文档](http://dev.ibrand.com/docs/api/v1/index)了解请求地址和相关参数说明。

我们提供了完整的 Postman 文件，可以通过百度网盘下载：

- Postman 软件下载  https://pan.baidu.com/s/1bqVD5MJ  密码：4lku
- Postman API 请求下载  https://pan.baidu.com/s/17EtkM1QCA9jVRzIQ6sdzGg 提取码: 9m54

## Laravel API 部署

要本地开发部署，需要先搭建好本地的开发环境，本文已经假设你已经会通过各类工具（homestead）等来开发 Laravel 项目

### 下载源码

```
git clone https://github.com/ibrandcc/ecommerce-open-api
```
或者

```
composer create-project ibrand/open-ecommerce
```

### Laravel 常规安装

以下步骤基本是 Laravel 项目安装需要执行的必须步骤

#### 安装依赖包

我们为了方便大家使用，在项目的 `composer.json` 中已经默认使用了国内的 `composer` 镜像源，感谢 [laravel-china](https://laravel-china.org)

下载好源码后，直接执行

```
composer install -vvv
```

#### 设置 .env

.env 文件中的数据库部分设置成自己开发的数据库配置

```
cp .env.example .env
```

####  应用密钥

通过以下命令来生成应用密钥，密钥值在 `.env` 文件 `APP_KEY`
```
php artisan key:generate
```

#### 发布相关资源

执行 `publish` 命令发布所有相关的资源，包含配置项，静态资源等。

```
php artisan vendor:publish --all
```

#### 设定公共磁盘软连接

Laravel 中上传文件通常是存储在 `storage/app/public` 目录下，该目录下的文件可以通过 `php artisan storage:link` 命令软连接到 `public` 目录下，以供外部访问。

更多细节请见：[文件系统](https://laravel-china.org/docs/laravel/5.5/filesystem/1319)

### 完成安装

执行内置命令完成数据库及其他配置和数据初始化等任务。

```
php artisan ibrand:store-install 
```

### 最后一步

请把 `.env` 文件中 APP_URL 值设置为你当前的域名，比如开源 demo 环境中
```
APP_URL=https://demo-open-admin.ibrand.cc
```
因为后续为了方便上 https ，所以此处 APP_URL 值必须指定当前项目所在域名。

> 欢迎提交问题，觉得项目不错，记得 star : )    项目传送门：[ibrand-ecommerce-open-source](https://github.com/ibrandcc/ecommerce-open-api)