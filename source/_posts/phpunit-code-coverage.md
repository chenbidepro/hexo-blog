---
title: phpunit 单元测试之代码覆盖率
date: 2018-11-10 12:33:32
tags: [phpunit,单元测试]
categories: [laravel-tips]
---

最近团队在不断完善项目中的单元测试用例，会用到代码覆盖率分析，本来以为 homestead 应该默认安装了 xdebug ，所以使用 `phpunit --coverage-html ./tests/codeCoverage` 来生成 html 报告，但是执行后提示如下错误
```
Error:         No code coverage driver is available
```
这是因为没有安装或启用 xdebug 导致。

**个人环境：**

PHP 7.2.0-1+ubuntu16.04.1 + Homestead 

## install xdebug

```sh
$ wget https://xdebug.org/files/xdebug-2.6.0.tgz
$ tar xvzf xdebug-2.6.0.tgz
$ cd xdebug-2.6.0
$ phpize7.2
$ ./configure --enable-xdebug
$ make
$ sudo make install
```

### enable xdebug for php
```
find /usr/ -name "xdebug.so"
```
```
/usr/lib/php/20170718/xdebug.so  //刚刚安装的 xdebug 2.6.0 版本
/usr/lib/php/20131226/xdebug.so
/usr/lib/php/20160303/xdebug.so
/usr/lib/php/20151012/xdebug.so
```
```
vi /etc/php/7.2/cli/php.ini
```
添加如下代码到 `php.ini` 结尾处
```
zend_extension="/usr/lib/php/20170718/xdebug.so"
xdebug.remote_enable = 1
xdebug.remote_connect_back = 1
xdebug.remote_port = 9000
xdebug.max_nesting_level = 500
```

## build code coverage report

有两种方法：

1.直接执行 `phpunit --coverage-html ./tests/codeCoverage` 命令
2.在 `phpunit.xml` 添加如下代码：

```xml
<logging>
   <log type="coverage-html" target="./tests/codeCoverage" charset="UTF-8"/>
</logging>
```
然后直接执行 `phpunit` 即可。

完成会在 `tests/codeCoverage` 目录下生成 html 报告，如下所示：


![clipboard.png](https://cdn.chenhow.com/phpunit-code-coverage/1.png)

通过这样的分析，能够更好的帮助我们完善单元测试，保证代码测试的完整性，也能让我们的代码更加健壮。

