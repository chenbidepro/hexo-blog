---
title: Homestead 安装 phpMyAdmin 作为数据库管理客户端
date: 2018-12-19 14:46:05
tags: [laravel,php,phpmyadmin,mysql]
categories: [Laravel 教程]
---


## 简介

> phpMyAdmin 是一个以PHP为基础，以Web-Base方式架构在网站主机上的MySQL的数据库管理工具，让管理者可用Web接口管理MySQL数据库。借由此Web接口可以成为一个简易方式输入繁杂SQL语法的较佳途径，尤其要处理大量资料的汇入及汇出更为方便。其中一个更大的优势在于由于phpMyAdmin跟其他PHP程式一样在网页服务器上执行，但是您可以在任何地方使用这些程式产生的HTML页面，也就是于远端管理MySQL数据库，方便的建立、修改、删除数据库及资料表。也可借由phpMyAdmin建立常用的php语法，方便编写网页时所需要的sql语法正确性。

12 年通过 WordPress 接触 phpMyadmin，当时大部分的虚拟主机服务商都提供 phpMyAdmin 来管理 mysql 数据，对于不了解 mysql 命令的初学者更加易于学会使用，而且它相对于其他客户端工具（navicate，SQLyog）来说是免费开源的。

在整个系列教程中，因为 Laravel  Database Migrations 的强大，并不会经常通过 phpMyAdmin 来管理 mysql，最主要目的是用来更加直观的查看数据库中数据表的结构和数据。

## 下载

1. 通过官网进行下载： [phpmyadmin][1]
2. 百度网盘： https://pan.baidu.com/s/1bqVD5MJ 密码：4lku

## 安装

### 解压文件

下载后请解压到工作目录(`C:\workspace`)，并把文件夹命名为 `phpMyAdmin` 如下图所示：

![clipboard.png](https://cdn.chenhow.com/laravel-homestead-phpmyadmin/1.png)

### Homestead.yaml

**新增文件同步目录**

在 `folders:` 下添加如下代码
```
    - map: C:/workspace/phpMyAdmin
      to: /mnt/www/phpMyAdmin
```

把源码目录映射同步到虚拟主机上的 `/mnt/www/phpMyAdmin` 目录下。

**增加虚拟主机**

在 `sites:` 下添加如下代码

```
    - map: phpmyadmin.test
      to: /mnt/www/phpMyAdmin
```

> 请注意文件中的空白处必须是空格键打出来的空格，不可用 Tab 键。

### 重载 Homestead.yaml

在 `C:\workspace\homestead` 目录，右键 `Git Bash Here` 打开命令行，执行 `vagrant provision` 命令重载 `Homestead.yaml` 文件。


### 添加 hosts

用 Nodepad++ 打开 `C:\Windows\System32\drivers\etc\hosts` 文件，添加如下代码：

```
192.168.10.10  phpmyadmin.test
```

### 配置

执行完毕 `vagrant provision` 并且添加 `host` 好以后，我们就可以通过浏览器访问 `phpmyadmin.test` 来到 phpMyadmin 的管理界面了。

为了能够顺利登入 phpMyadmin，我们还需要继续一些配置。

#### config.inc.php

把 `C:\workspace\phpMyAdmin\config.sample.inc.php` 文件复制一份并命名为 `config.inc.php`

此时我们访问 `phpmyadmin.test` ，并用 vagrant 中 mysql 的账号(**`homestead`**)密码(**`secret`**)登录会遇到如下错误提示:

![clipboard.png](https://cdn.chenhow.com/laravel-homestead-phpmyadmin/2.png)

这是因为 vagrant 默认会给所有的文件 `777` 权限，而 phpMyAdmin 又不允许这样而导致的，因为是本地环境，我们可以通过配置去忽略这个提示。

用 Notepad++ 打开 `C:\workspace\phpMyAdmin\libraries\config.default.php` 文件,在 `2961` 行 

```
$cfg['CheckConfigurationPermissions'] = true;
```
改为 
```
$cfg['CheckConfigurationPermissions'] = false;
```

完成以上配置后，就可以正常登入 phpMyAdmin 


#### 配置短语密码

登入 phpMyAdmin 后在下方有一个报警提示 `配置文件现在需要一个短语密码。`

我们需要在 phpMyAdmin 的配置文件 `config.inc.php` 里的 `blowfish_secret ` 配置去设置一个密码，phpMyAdmin 会用到这个密码去加密 Cookie 。

在之前打开的命令行窗口中输入 `openssl rand -base64 32` 命令，会得到一串字符串 `IDbwuz5M0yTke6ZzKTnfW35VZ46DEnDbC5h+8AILjlI=`

![clipboard.png](https://cdn.chenhow.com/laravel-homestead-phpmyadmin/3.png)

复制返回来的随机密码，然后打开 config.inc.php，搜索 $cfg['blowfish_secret'] ，把复制的密码粘贴到这个配置的后面。
```
$cfg['blowfish_secret'] = 'IDbwuz5M0yTke6ZzKTnfW35VZ46DEnDbC5h+8AILjlI=';
```
保存配置文件，回到浏览器，重新登录，警告就会消失了。

## 总结

整个操作完成后，我们可以在 phpMyAdmin 的管理界面看到已经配置好的 `homestead` 数据库。


![clipboard.png](https://cdn.chenhow.com/laravel-homestead-phpmyadmin/4.png)

在后面的学习过程中，我们能够通过 phpMyAdmin 快速的查看数据库，数据表，执行 SQL 语句，导入导出数据等操作。




[1]: https://www.phpmyadmin.net/