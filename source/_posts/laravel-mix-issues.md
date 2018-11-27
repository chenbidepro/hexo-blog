---
title: laravel-mix-issues
date: 2018-11-27 06:49:59
tags: [laravel,mix]
---

按照 laravel 官方文档在准备使用 [laravel-mix][1] 时遇到了很多问题，许多同学应该会遇到同样的问题，自己花了一些时间来解决这些问题，在此做个笔记帮助大家减少填坑的时间。

### 环境
1. laravel v5.4
2. node v6.10.2
3. npm v3.10.10

> Homestead 中 node 和 npm 默认的版本如上述所示

### 问题

#### 1. 直接执行 npm intall 会出现 symlink 错误

![symlink_error](/laravel-mix-issues/symlink_error.png)

该错误是自己没有仔细看官方文档导致，需要执行 `npm install --no-bin-links`

> 如果你使用的是 Windows 系统或运行在 Windows 系统上的 VM, 你需要在运行 npm install 命令时将 --no-bin-links 开启

#### 2. cross-env: not found

正确执行 npm 安装成功后，执行 `npm run dev` 会提示 `cross-env:not found` 错误。在 laravel 5.4 中 package.json 中的内容如下：
```
{
  "private": true,
  "scripts": {
    "dev": "npm run development",
    "development": "cross-env NODE_ENV=development node_modules/webpack/bin/webpack.js --progress --hide-modules --config=node_modules/laravel-mix/setup/webpack.config.js",
    "watch": "cross-env NODE_ENV=development node_modules/webpack/bin/webpack.js --watch --progress --hide-modules --config=node_modules/laravel-mix/setup/webpack.config.js",
    "watch-poll": "npm run watch -- --watch-poll",
    "hot": "cross-env NODE_ENV=development node_modules/webpack-dev-server/bin/webpack-dev-server.js --inline --hot --config=node_modules/laravel-mix/setup/webpack.config.js",
    "prod": "npm run production",
    "production": "cross-env NODE_ENV=production node_modules/webpack/bin/webpack.js --progress --hide-modules --config=node_modules/laravel-mix/setup/webpack.config.js"
  },
  "devDependencies": {
    "axios": "^0.16.2",
    "bootstrap-sass": "^3.3.7",
    "cross-env": "^5.0.1",
    "jquery": "^3.1.1",
    "laravel-mix": "^1.0",
    "lodash": "^4.17.4",
    "vue": "^2.1.10"
  }
}
```
请按照如下更改
```
{
  "private": true,
  "scripts": {
    "dev": "npm run development",
    "development": "node node_modules/cross-env/dist/bin/cross-env.js NODE_ENV=development node_modules/webpack/bin/webpack.js --progress --hide-modules --config=node_modules/laravel-mix/setup/webpack.config.js",
    "watch": "node node_modules/cross-env/dist/bin/cross-env.js NODE_ENV=development node_modules/webpack/bin/webpack.js --watch --progress --hide-modules --config=node_modules/laravel-mix/setup/webpack.config.js",
    "watch-poll": "npm run watch -- --watch-poll",
    "hot": "node node_modules/cross-env/dist/bin/cross-env.js NODE_ENV=development node_modules/webpack-dev-server/bin/webpack-dev-server.js --inline --hot --config=node_modules/laravel-mix/setup/webpack.config.js",
    "prod": "npm run production",
    "production": "node node_modules/cross-env/dist/bin/cross-env.js NODE_ENV=production node_modules/webpack/bin/webpack.js --progress --hide-modules --config=node_modules/laravel-mix/setup/webpack.config.js"
  },
  "devDependencies": {
    "axios": "^0.16.2",
    "bootstrap-sass": "^3.3.7",
    "cross-env": "^5.0.1",
    "jquery": "^3.1.1",
    "laravel-mix": "^1.0",
    "lodash": "^4.17.4",
    "vue": "^2.1.10",
    "vue-loader": "^13.0.0"
  }
}

```
注意看 scripts 中的区别

#### 3. no such file or directory , scandir ‘…/node_modules/node-sass/vendor

重建 node-sass 即可，请务必执行如下命令：

`npm rebuild node-sass --no-bin-links`

#### 4. TypeError: loader.charAt is not a function

需要安装最新版本的 vue-loader

`npm install vue-loader --save-dev --no-bin-links`

### 结束

本来准备使用laravel+vue2 来写点小demo，在安装运行过程中遇到以上4个问题，4个问题是按顺序出现的，按照以上进行解决基本是能够正常执行通过的，有什么问题可以留言交流。


[1]: http://d.laravel-china.org/docs/5.4/mix#running-mix