---
title: Laravel 5.5 官方推荐 Nginx 配置学习
date: 2018-10-09 07:51:55
tags: [laravel,nginx]
---

Laravel 5.5 版本官方放出了 Nginx 服务器的配置，中文文档：[服务器配置 Nginx][1]

```nginx
server {
    listen 80;
    server_name example.com;
    root /example.com/public;

    add_header X-Frame-Options "SAMEORIGIN";   
    add_header X-XSS-Protection "1; mode=block"; 
    add_header X-Content-Type-Options "nosniff"; 

    index index.html index.htm index.php;

    charset utf-8;

    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }

    location = /favicon.ico { access_log off; log_not_found off; }  
    location = /robots.txt  { access_log off; log_not_found off; }  

    error_page 404 /index.php;

    location ~ \.php$ {
        fastcgi_split_path_info ^(.+\.php)(/.+)$;
        fastcgi_pass unix:/var/run/php/php7.1-fpm.sock;
        fastcgi_index index.php;
        include fastcgi_params;
    }

    location ~ /\.(?!well-known).* {
        deny all;
    }
}
```

自己并不擅长 Nginx，相信很多朋友跟我一样，让我们一起学习下 Nginx 的相关知识 : )

### 1.  add_header X-Frame-Options "SAMEORIGIN";

`X-Frame-Options` 响应头是用来给浏览器指示允许一个页面可否在` <frame>`, `<iframe>` 或者` <object> `中展现的标记。网站可以使用此功能，来确保自己网站的内容没有被嵌到别人的网站中去，也从而避免了点击劫持 (clickjacking) 的攻击。

X-Frame-Options 有三个值:

**DENY**
​    表示该页面不允许在 frame 中展示，即便是在相同域名的页面中嵌套也不允许。
​    
**SAMEORIGIN**
​    表示该页面可以在相同域名页面的 frame 中展示。
​    
**ALLOW-FROM uri**
​    表示该页面可以在指定来源的 frame 中展示。

该响应头设置应该比较常见，之前国外客户的安全团队有使用工具扫描我们项目的相关漏洞，其中就有这个 `clickjacking` 的问题，最终也是通过该设置来解决此问题。

### 2. add_header X-XSS-Protection "1; mode=block";

XSS 是跨站脚本攻击，是比较常见的网络攻击手段，改字段指示浏览器是否为当前页面开启浏览器内建的 XSS 过滤机制。 `1` 表示允许过滤器，`mode=block` 指示浏览器在检测到 XSS 攻击后禁止加载整个页面。

参考文章： [先知XSS挑战赛 知识点提要][2]

### 3. add_header X-Content-Type-Options "nosniff";

该响应头设置禁用浏览器对 Content-Type 类型进行猜测的行为。因为很多情况下服务器并没有很好的配置 Content-Type 类型，因此浏览器会根据文档的数据特征来确定类型，比如攻击者可以让原本解析为图片的请求被解析为 JavaScript。

> 我们发现以上三个比较常见的防攻击配置，还是非常实用的，建议使用，之前我们的服务器只使用了 `add_header X-Frame-Options "SAMEORIGIN";` 配置。

### 4. 不记录 favicon.ico 和 robots.txt 日志

```
    location = /favicon.ico { access_log off; log_not_found off; }
    location = /robots.txt  { access_log off; log_not_found off; }
```
`favicon.ico` 网站头像，默认是浏览器标签页上网站小图标以及收藏时显示的小图标。

如果未在html header中指定 `favicon.ico` 那么浏览器默认会去访问 `http://xxx.com/favicon.ico` , 不存在此文件的话，那么会导致404，同时会记录到 access_log 和 error_log 中。这种记录到日志文件中是没有必要性的，因此可以取消。

`robots.txt` 通常是搜索引擎蜘蛛（爬虫）会去爬取的文件，在行业规范中，蜘蛛去爬取一个网站的时候会首先爬取该文件来获知网站中哪些目录文件不需要爬取，在 SEO 中 `robots.txt` 的正确配置是对 SEO 非常有效果的。该文件也确实没有必要记录到日志中，而且大部分网站并不存在 `robots.txt` 文件。


> 以上这些配置是可以用在大部分的网站上的，不止是 Nginx 服务器，相信 Apache 服务器也有相关的配置，如果你正在用其他web服务器，以上类似的配置也建议使用。

[1]: https://d.laravel-china.org/docs/5.5/deployment
[2]: http://blog.csdn.net/yuanfeng20000/article/details/77919587