---
title: Laravel 开源电商常见问题
date: 2018-12-13 06:01:13
tags: [laravel,开源电商]
categories: [开源电商]
---

问： H5 微商城有没有开源计划？

> 暂时没有计划。项目成立在2016年9月，当时只有 vue1.x 的版本，而目前 vue 流行的是 2.x 的版本，而且前端团队重心在微信小程序上，再加上团队时间有限，所以暂不考虑开源。

问： 数据库表结构有文档吗？

> 暂未提供，请关注官网或者入群，更新后会及时通知。

问： 为什么 Laravel  app 目录下没有任何代码？

> iBrand 项目需求庞大，整个项目是基于模块化开发，所以相关源码在 modules 目录下，请注意源码结构说明一文。

问： 请问 Laravel 版本？

> 目前 iBrand 所有后端 Laravel 项目都是基于 5.5 LTS 版本进行开发，开源项目也是如此，主要是保证稳定性。
>
> 但是在 Laravel 下个 LTS 出来后，我们将同步升级到最新的 LTS 版本。

问： 数据库文件在哪？

> 项目是通过 Laravel migration 来创建数据库，请按照[体验与部署](https://www.ibrand.cc/open/article?course_id=1&chapter_id=1&article_id=26)文档进行操作，完成后数据表将完整创建。