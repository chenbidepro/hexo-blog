---
title: 修复 github 项目的语言属性
date: 2018-12-27 11:49:01
tags: [github]
categories: [Laravel-tips]
---

## issue

[Laravel 开源电商项目源码](https://github.com/guojiangclub/ecommerce-open-api) 被 github 判断认为是 HTML 项目，但是实际项目并没有多少 html 代码。

![issue](https://cdn.chenhow.com/fix-github-project-type-issue/1.png)

这就尴尬了，只有默默的通过 google 搜索  `github change project type` 发现这篇文章：[How to Change Repo Language in GitHub](https://hackernoon.com/how-to-change-repo-language-in-github-c3e07819c5bb)

简单来说只要在项目根目录下添加 `.gitattributes` 文件，然后通过设置相关目录或者类型文件标记为相关语言即可。

## gitattributes 

支持的属性语法如下：

- linguist-documentation
- linguist-language
- linguist-vendored
- linguist-generated
- linguist-detectable

## usage

```
*.css linguist-vendored
*.js linguist-language=Vue
modules/* linguist-language=PHP
```
## resources

[github/linguist](https://github.com/github/linguist)







