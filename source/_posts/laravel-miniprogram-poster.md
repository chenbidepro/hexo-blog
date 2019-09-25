---
title: Laravel 生成小程序图文海报最佳方案之一
date: 2018-11-06 07:31:36
tags: [laravel,laravel-package,微信小程序]
categories: [Laravel-packages]
---

> 目前已经更新 2.0 版本，支持生成的海报关联Model，支持是否重新生成海报等功能，具体更新请移步 github: [laravel-miniprogram-poster](https://github.com/guojiangclub/laravel-miniprogram-poster)

微信小程序官方并未提供分享到朋友圈的方法，所以目前基本整个行业都是使用生成图文海报发到朋友圈，然后识别太阳码进入到小程序。

通过谷歌或者百度有很多同学已经提供了一些解决方案，但是在我们使用后效果并不是很理想，主要体现在以下方面：

1. 通过PHP写入的字体效果并不理想。
2. 背景图片和微信头像合成后清晰度不够。
3. 无法实现一些复杂的效果。
4. 实现过程也较复杂。

最终我们找了一种认为非常合理的实现方式，就是基于 PhantomJS 实现，通过 PhantomJS 隐形浏览器截图的功能来生成海报。

> PhantomJS是一个基于webkit的JavaScript API。它使用QtWebKit作为它核心浏览器的功能，使用webkit来编译解释执行JavaScript代码。任何你可以在基于webkit浏览器做的事情，它都能做到。它不仅是个隐形的浏览器，提供了诸如CSS选择器、支持Web标准、DOM操作、JSON、HTML5、Canvas、SVG等，同时也提供了处理文件I/O的操作，从而使你可以向操作系统读写文件等。PhantomJS的用处可谓非常广泛，诸如网络监测、网页截屏、无需浏览器的 Web 测试、页面访问自动化等。

有以下优点：

1. 基于html可实现复杂的文字，图片，阴影效果。 
2. 清晰度和文件大小合理
3. 使用简单、即插即用

包地址：[laravel-miniprogram-poster](https://github.com/guojiangclub/laravel-miniprogram-poster) 求 star : )

## 体验

扫码进入商品详情页分享生成图文体验

![iBrand开源体验店](https://iyoyo.oss-cn-hangzhou.aliyuncs.com/post/miniprogramcode/1.jpg)

## 安装
```
composer require ibrand/laravel-miniprogram-poster 
```
- 低于 Laravel5.5 版本，`config/app.php` 文件中 'providers' 添加`iBrand\Poster\PhantoMmagickServiceProvider::class`

- 图片保存在  `storage/app/public` 下所以需要执行  `php artisan storage:link`

- 如需自定义配置请执行 `php artisan vendor:publish --provider="iBrand\Poster\PhantoMmagickServiceProvider" --tag="config"`

## 配置项

``` 
    return [
    	//图片存储位置
    	'disks'      => [
    		'MiniProgramShare' => [
    			'driver'     => 'local',
    			'root'       => storage_path('app/public/share'),
    			'url'        => env('APP_URL') . '/storage/share',
    			'visibility' => 'public',
    		],
    	],
    	//图片宽度
    	'width'      => '575px',
    	//放大倍数
    	'zoomfactor' => 1.5,
    	//0-100,100质量最高
    	'quality'    => 100,
    	//是否压缩图片
    	'compress'   => true,
    ];
```

## 使用
```
use iBrand\Miniprogram\Poster\MiniProgramShareImg;
    
$url = 'https://www.ibrand.cc/';
$result = MiniProgramShareImg::generateShareImage($url);

```
返回结果：
```
    [
        'url'  => 'http://xxx.png',   图片访问url
        'path' => 'path/to/image', 图片文件路径
    ]
```
## 字体安装

如果需要实现复杂的字体效果，需要安装字体，比如在 centos 上就没有微软雅黑的字体，所以如果生成的图片有指定的特殊字体，需要在服务器上进行安装。

* window 将下载的字体文件复制到C:Windows\Fonts目录下或者双击字体文件进行安装
* mac 下载的字体文件 双击字体文件进行安装
* centos
```
# 安装微软雅黑
wget -P /tmp/ https://iyoyo.oss-cn-hangzhou.aliyuncs.com/mirror/fonts/msyh.ttf
wget -P /tmp/ https://iyoyo.oss-cn-hangzhou.aliyuncs.com/mirror/fonts/msyhbd.ttf
wget -P /tmp/ https://iyoyo.oss-cn-hangzhou.aliyuncs.com/mirror/fonts/msyhl.ttf
cd /usr/share/fonts/lyx/
mkdir chinese
cd chinese
mv /tmp/msyhbd.ttf ./
chmod 755 *.ttf
yum -y install mkfontscale
mkfontscale
mkfontdir
fc-cache -fv
```

## Resource

项目基于[PhantomMagick](https://github.com/anam-hossain/phantommagick)

