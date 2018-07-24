---
title: 手机验证码
date: 2018-05-29 14:20:42
tags: [laravel,手机验证码]
---

随着国家实名制的要求，在17年就要求所有提供账号服务的网站（系统）都需要绑定手机号（因为目前手机号的办理都需要实名制）来实现用户实名制，所以目前90%以上的网站都提供了手机号+验证码的注册方式。



常见的手机验证码发送与验证需要以下条件：

1. 需要一个可靠的验证码发送通道供应商。
2. 需要一个签名名称。

> 签名名称，是在验证码中需要包含品牌信息，而且有严格的格式要求：【iBrand艾游】，iBrand艾游就是签名名称，通过签名在短信的最前面或者最后面。



为了简易开发流程，我们封装了  [laravel-sms](https://github.com/ibrandcc/laravel-sms) 包来实现短信的发送与验证，它具备以下特点：

1. 支持短信验证码直接在 config 中配置模板ID
2. 支持短信验证码自定义长度
3. 支持短信验证码有效分钟，默认5分钟
4. 支持短信验证码重试次数，防止用户意外输错验证码导致需要再次发送验证码的问题。
5. 支持短信验证码未验证时，用户再次请求验证码，在有效分钟内验证码保持一致。
6. 集成短信发送路由，支持 web 和 api 发送方式。
7. 支持验证码调试，debug 模式下可直接查询手机号目前有效的验证码
8. 支持短信验证码发送记录保存到数据库



## 安装 laravel-sms

通过 `xShell` 登陆到项目根目录。



通过 `composer` 命令安装 `laravel-sms package`

```bash
composer require ibrand/laravel-sms:~1.0 -vvv
```

 执行 `artisan` 命令把配置文件发布到 `config` 目录

```bash
php artisan vendor:publish --provider="iBrand\Sms\ServiceProvider"
```

 执行完毕后会生成 `config/ibrand/sms.php` 文件。







将使用 iBrand 封装的

