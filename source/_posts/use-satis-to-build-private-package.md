---
title: 使用 Satis 搭建私有的 Composer 包仓库
date: 2018-10-20 14:04:18
tags: [laravel,composer,packages,laravel-package]
categories: [Laravel-tips]
---

## 简述

iBrand 产品立项时是商业性质的项目，但是在搭建架构时考虑后续的通用性，因此每个模块都设计成一个 Package，作为公司内部用，因此这些包并不能提交到 [packagist.org][1] 上去。 所以就想是否能够搭建私有的包仓库，实现一个私有的 packagist 。 

仔细翻阅 Composer 文档，发现官方有相应的解决方案：[Handling private packages][2] 

**这里推荐使用 Satis** ，也正是我们目前使用的方案，目前运行一切良好。

> Satis 是一个静态的 composer 资源库生成器。它像是一个超轻量级的、基于静态文件的 packagist 版本。

> 你给它一个包含 composer.json 的存储库，定义好 VCS 和 资源库。它会获取所有你列出的包，并打印 packages.json 文件，作为 composer 类型的资源库。

## 说明

**服务器环境**：
1. centos7.2
2. nginx
3. php7.1

代码管理平台：[码云][3]

**文章中尽量以一个真实的情况来撰写，但是文章的仓库地址，网页地址均是不可访问的，用虚拟信息替换了真实信息。**


## Create Private Git Project

既然为公司内部项目源码是不公开的，我们选择了[码云][4]，未选择 github，主要有两个原因：

1. github 因为是国外服务器，国内偶尔会抽风。
2. 国内也有比较优秀的 git 代码托管平台，免费支持 Private Project。比如：[码云][5]，[Coding][6]

github private project 是收费的，对于一个公司来说费用不高，但是加上以上两点原因后，所以未选择。

假设我们已经在码云上建立好了私有项目，并且已经编写好了所有的代码和单元测试。

**ssh 地址：** git@gitee.com:iBrand/test-private-composer.git

**composer.json**

```
{
  "name": "iBrand/test-private-composer",
  "type": "library",
  "description": "iBrand test private composer",
  "keywords": [
    "iBrand crop",
    "private composer",
  ],
  "license": "MIT",
  "authors": [
    {
      "name": "shjchen",
      "email": "ibrand.shjchen@foxmail.com"
    }
  ],
  "require": {
    "php": "^5.6|^7.0",
  },
  "autoload": {
    "psr-4": {
       "iBrand\\Prviate\\Composer\\": "src/"
    }
  },

  "minimum-stability": "dev",
  "prefer-stable": true
}
```

## Create Satis Server

### Install
```
$ cd /data/www/
$ composer create-project composer/satis company-private-composer --stability=dev --keep-vcs
```

### Setup

```
$ cd /data/www/company-private-composer
$ vi satis.json
```

```
{
  "name": "iBrand Private Composer Repository", 
  "homepage": "http://packagist.iBrand.com",
  "repositories": [
    { "type": "vcs", "url": "git@gitee.com:iBrand/test-private-composer.git" }
    // more vcs url.
  ],
  "require": {
    "ibrand/test-private-composer": "*",
    // you more package.
	},
  "archive": {
        "directory": "dist",
        "format": "tar",
        "prefix-url": "http://packagist.iBrand.com"
    }
}
```

解释下 `satis.json` 配置文件

- name：仓库名称，可以随意定义
- homepage：仓库主页地址
- repositories：指定包源
- require：指定包源版本，* 代码编译所有版本，如果想获取所有包，使用 require-all: true,
- directory: required, the location of the dist files (inside the output-dir)
- format: optional, zip (default) or tar
- prefix-url: optional, location of the downloads, homepage (from satis.json) followed by directory by default

### Authentication

在进行 `Build` 前，我们需要解决代码权限问题，因为前面的项目源码是私有的，所以服务器上需要有读取源码的权限，依然以码云举例：

**生成ssh公钥**

你可以按如下命令来生成 sshkey: 
```
$ ssh-keygen -t rsa -C "xxxxx@xxxxx.com" 
# Generating public/private rsa key pair...
# 三次回车即可生成 ssh key

```
查看你的 public key，并把他添加到码云（Gitee.com） SSH key添加地址:https://gitee.com/profile/sshkeys)
```
$ cat ~/.ssh/id_rsa.pub
# ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQC6eNtGpNGwstc....
```

### Build

```
php bin/satis build satis.json public/
```
执行后会在 `/data/www/company-private-composer/public` 生成仓库列表

![1](https://cdn.chenhow.com/use-satis-to-build-private-package/1.png)

### Setup Nginx

上一步已经生成了仓库列表，为了保证可访问需要通过 `nginx` or `apache` 把服务暴露出去，我们使用的是 nginx ，因此以 `nginx` 举例。

```
server {
    listen  80;
    server_name packagist.iBrand.com;
    root /data/www/company-private-composer/public;
    index index.php index.html;

    access_log /data/logs/packages-access.log;
    error_log  /data/logs/packages-error.log error;
    rewrite_log on;
    
    location ~* \.php$ {
        include fastcgi_params;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        fastcgi_pass 127.0.0.1:9000;
        fastcgi_index index.php;
    }

    location = /favicon.ico {
            log_not_found off;
            access_log off;
    }
}
```
配置好后需要执行 `service nginx reload` ，然后就可以通过访问  `http://packagist.iBrand.com` 看到自己的仓库列表，如下图：

![2](https://cdn.chenhow.com/use-satis-to-build-private-package/2.png)

### Usage

接下来就可以在项目中使用这个私有的 Composer 包仓库。

添加如下配置到项目中的 `composer.json` 文件中

```
"require": {
    "iBrand/test-private-composer": "~1.0"
  }
"config": {
    "preferred-install": "dist",
    "secure-http": false
  },
  "repositories": [
    {
      "type": "composer",
      "url": "http://packagist.iBrand.com/"
    }
  ]
```
## 待续
1. 实现 webhooks 源码更新时自动 Build


## 参考资料 

1. [Handling private packages][7]
2. [Hosting your own package][8]
3. [使用私有资源库][9]


[1]: packagist.org
[2]: https://getcomposer.org/doc/articles/handling-private-packages-with-satis.md
[3]: https://gitee.com/
[4]: https://gitee.com/
[5]: https://gitee.com/
[6]: https://coding.net/
[7]: https://getcomposer.org/doc/articles/handling-private-packages-with-satis.md
[8]: http://docs.phpcomposer.com/05-repositories.html#Hosting-your-own
[9]: http://docs.phpcomposer.com/05-repositories.html#Using-private