---
title: ibrand-building-user-package
date: 2018-05-29 14:24:02
tags: [laravel,ibrand,package]
---

在开始进行注册登陆流程开发前，我们需要完成用户模块包的搭建，主要目的是封装一些用户模块常用的功能，方便日后在不同的项目中方便使用。

>  至少在 iBrand 中是这样，除了完成 iBrand 产品，我们也会承接一些高质量的项目，所以在其他项目中，我们会引用 iBrand 中基础的功能包。

并且在 [完成架构搭建](https://segmentfault.com/a/1190000014830550)  中，我们已经演示了如何建立一个 `package`

## 添加模块 

用户模块是一个常用的组件，对于组件我们统一放在 `modules/component` 目录下。

按照下面的结构建立好相关文件和文件夹

```
api-tutorial-source
├── app
├── ...
└── modules
    └── component
        └── user
            ├── LICENSE
            ├── README.md
            ├── composer.json
                ├── src
                │   └── Providers
                │       └── ServerServiceProvider.php
                └── tests
```
