---
title: Laravel SMS 短信发送包
date: 2018-10-28 08:13:45
tags: [laravel,sms,laravel-package]
---

# Laravel Sms

Laravel 贴合实际需求同时满足多种通道的短信发送组件

![build](https://cdn.chenhow.com/laravel-sms-package/build.png)

基于业务需求在 [overtrue/easy-sms][1] 进行扩展开发，主要实现如下功能：

1. 支持短信验证码直接在 config 中配置模板ID
2. 支持短信验证码自定义长度
3. 支持短信验证码有效分钟，默认5分钟
4. 支持短信验证码重试次数，防止用户意外输错验证码导致需要再次发送验证码的问题
5. 支持短信验证码未验证时，用户再次请求验证码，在有效分钟内验证码保持一致
6. 集成短信发送路由，支持 web 和 api 发送方式
7. 支持验证码调试，debug 模式下可直接查询手机号目前有效的验证码，debug 模式下同时不会验证验证码的正确性
8. 支持验证码发送记录到数据库，方便查看发送日志和错误原因

> 包地址：[ibrand/laravel-sms][2]

**TODO：**

1. 支持语音验证码

## 安装

```
composer require ibrand/laravel-sms:~1.0 -vvv
```

低于 Laravel5.5 版本

`config/app.php` 文件中 'providers' 添加
```
iBrand\Sms\ServiceProvider::class
```

`config/app.php` 文件中 'aliases' 添加

```
'Sms'=> iBrand\Sms\Facade::class
```

## 使用

### 发送验证码

实现了发送短信验证码路由，支持 web 和 api ，可以自定义路由的 prefix。
```
'route' => [
        'prefix' => 'sms',
        'middleware' => ['web'],
    ],
    
or

'route' => [
        'prefix' => 'sms',
        'middleware' => ['api'],
    ],
```

POST请求 `http://your.domain/sms/verify-code` 

参数：mobile

备注：为了支持开发时的调试，在发送验证码时不去验证手机号本身的有效性，请在发送验证码前自行验证。

返回参数：

```
{
    "status": true,
    "message": "短信发送成功"
}
```

### 验证验证码

```
    use iBrand\Sms\Facade as Sms;
    

    if (!Sms::checkCode(\request('mobile'), \request('code'))) {
            //验证失败，处理自身业务
        }

```

### 配置模板 ID

在 `config/ibrand/sms.php` 的 `gateways` 参数可以直接添加 `code_template_id` 来配置模板 id

```
    // 可用的网关配置
        'gateways' => [

            'errorlog' => [
                'file' => '/tmp/easy-sms.log',
            ],

            'yunpian' => [
                'api_key' => '824f0ff2f71cab52936axxxxxxxxxx',
            ],

            'aliyun' => [
                'access_key_id' => 'dalvTXXX',
                'access_key_secret' => 'XXXX',
                'sign_name' => '阿里云短信测试专用',
                'code_template_id' => 'SMS_80215252'
            ],

            'alidayu' => 
                //...
            ],
        ],
```

### 配置 Content

非模板类通道，可以通过 config/ibrand/sms.php 自定义短信内容

`'content' => '【your signature】亲爱的用户，您的验证码是%s。有效期为%s分钟，请尽快验证。'`

### debug 

在实际开发中会存在并不用真实发出验证码的情况，因此在 debug 模式下，可以通过

`http://your.domain/api/sms/info?mobile=1898888XXXX` 来直接只看某个手机号当前有效验证码信息。


> 欢迎大家 star 和提交 issue   :)


[1]: https://github.com/overtrue/easy-sms/
[2]: https://github.com/ibrandcc/laravel-sms