---
title: Windows + Homestead 5 搭建 Laravel 开发环境
date: 2018-11-16 14:33:31
tags: [homestead,laravel,virtualbox]
categories: [Laravel 教程]
---

## 统一开发环境

为了保证在学习和工作过程中避免因为开发环境不一致而导致各种各样的问题，Laravel 官方为了我们提供了一个完美的开发环境 Laravel Homestead，让我们无需再本地安装 PHP，web 服务器或任何服务软件。

Homestead 可以在任何 Windows、Mac 或 Linux 系统上运行，它包括了 Nginx Web 服务器、PHP 7.1、MySQL、PostgresSQL、Redis、Memcached、Node 以及开发 laravel 应用所需的东西。

**Homestead 内置软件：**
- Ubuntu 16.04
- Git
- PHP 7.1
- Nginx
- MySQL
- MariaDB
- Sqlite3
- Postgres
- Composer
- Node (带有 Yarn、Bower、Grunt 和 Gulp)
- Redis
- Memcached
- Beanstalkd
- Mailhog
- ngrok

> 对于初学者相关的软件和知识点可能还不太了解，但是无需担心，在后续的教程中会陆续使用并且会有相应的章节进行详细的讲解。

本次系列教程，我们将使用目前最新的版本 Homestead 5.1.0 + vagrant 2.0.1 + VirtualBox 5.2.6 + Win10 来进行开发环境的搭建。

> 这套软件环境在 Win7 上也可以正常使用。

相关的软件我们已经整理在百度网盘上，有如下软件：
1. Git：对源码进行版本管理。
2. TortoiseGit：对于初学者不熟悉命令时，可以使用该可视化工具代理命令来管理源码。
3. SourceTreeSetup：图形化 git 管理 + Git Flow 工具
4. Xshell：安全的终端管理软件，通过 SSH 来登录 Linux 系统。
5. VirtualBox： 虚拟机软件
6. Vagrant：创建虚拟化开发环境工具
7. homestead-virtualbox5.1.0: Homestead VirtualBox 下的虚拟机文件。
8. WinSCP: WinSCP是一个Windows环境下使用SSH的开源图形化SFTP客户端。同时支持SCP协议。它的主要功能就是在本地与远程计算机间安全的复制文件。
9. Notepad++: 是 Windows操作系统下的一套比较好用文本编辑器，不仅有语法高亮度显示，也有语法折叠功能，并且支持宏以及扩充基本功能的外挂模组。

以上软件可在百度网盘上进行下载，链接：https://pan.baidu.com/s/1bqVD5MJ 密码：4lku

建议安装先后顺序：[Git][1]->[TortoiseGit][2]->[Xshell][3]->SourceTreeSetup->[VirtualBox][4]->[Vagrant][5]->Homestead

除了 Homestead 外，其他软件傻瓜式下一步安装下去即可，一些软件的使用在后续章节也会做相关介绍。

> 重要说明：因为篇幅有限，文中牵涉的软件暂时不会进行详细的介绍，该文章最终目的是保证初学者按照操作后，能够成功建立开发环境。 Vagrant 的一些常用命令，可以通过 Google 或百度搜索相关文章。

## 安装 Homestead

需要先安装好 Git,VirtualBox,Vagrant 三个必要软件。

### 添加 Homestead Box

在C盘下新建 `workspace` 文件夹，并且把下载好的 `homestead-virtualbox5.1.0.box` 文件拷贝到该目录下，并且右键选择 `Git Bash Here` 在当前目录打开命令窗口。

![图片描述][https://cdn.chenhow.com/laravel-homestead-install/1.png]

通过 `vagrant box add` 命令来完成 `Homestead box` 的添加

```
$ vagrant --version
# Vagrant 2.0.1  查看 vagrant 版本，表示 vagrant 已经正常安装

$ vagrant box add laravel/homestead homestead-virtualbox5.1.0.box
```
执行结果如下图所示：

![图片描述][https://cdn.chenhow.com/laravel-homestead-install/2.png]

### 配置 Homestead

执行如下命令：

```
$ git clone https://github.com/laravel/homestead.git
$ cd homestead
$ bash init.sh
```
执行结果如下图所示：

![图片描述][https://cdn.chenhow.com/laravel-homestead-install/3.png]

执行完成后会生成 `Homestead.yaml` 文件，使用 Nodepad++ 打开该配置文件，相关配置的作用已经通过 `# +文字的方式进行了说明`，如下所示：
```
---
ip: "192.168.10.10"
memory: 2048
cpus: 1
provider: virtualbox

# 虚拟机配置，包含了IP地址，内存，cpu，以及驱动类型(virtualbox)

authorize: ~/.ssh/id_rsa.pub

keys:
    - ~/.ssh/id_rsa

# ssh 密钥文件，用来直接登录虚拟主机，后面也会用到此密钥，在后面从 Github 拉取源码时会用到

folders:
    - map: ~/code
      to: /home/vagrant/code

# 文件映射目录，通过该配置会把 Windows 系统下的文件自动同步到虚拟机上。`~/code` 代表当前系统用户目录下的 `code` 目录，如示例中系统的路径就是`C:\Users\32780\code`,`32780`是当前登录系统的用户名称。 

sites:
    - map: homestead.test
      to: /home/vagrant/code/public
      
# 站点配置，会自动生成 Laravel 的 nginx 虚拟主机文件。

databases:
    - homestead
# 数据库配置，在后续的过程中不是很常用
```

在最后我们为了实现一个简单的 `hello world`，请改为如下配置：
```
---
ip: "192.168.10.10"
memory: 2048
cpus: 1
provider: virtualbox

authorize: ~/.ssh/id_rsa.pub

keys:
    - ~/.ssh/id_rsa

folders:
    - map: C:/workspace/code
      to: /home/vagrant/code

sites:
    - map: homestead.test
      to: /home/vagrant/code

databases:
    - homestead
```

### 生成 SSH key
在启动 Homestead 虚拟主机前我们需要生成 SSH key，执行如下命令：

```
$ ssh-keygen -t rsa -C "xxxxx@xxxxx.com"  #请替换成你自己的邮箱
# Generating public/private rsa key pair...
# 三次回车即可生成 ssh key
```

![图片描述][https://cdn.chenhow.com/laravel-homestead-install/4.png]

### 启动 Homestead 虚拟主机

执行 `vagrant up` 前还需要再做一点小改动，才能保证正常启动。

打开 `C:\Users\32780\.vagrant.d\boxes\laravel-VAGRANTSLASH-homestead` 目录
> 请把 32780 替换成你目前登录 windows 系统的用户名

**两个改动：**
1. 把文件夹 `0` 改成当前 Homestead 的版本号 `5.1.0`
2. 添加 metadata_url 文件，内容只添加 `https://app.vagrantup.com/laravel/boxes/homestead` 即可，不要存在任何多余的空格字符。

接下来在 `C:\workspace\homestead` 目录下执行 `vagrant up` 启动虚拟主机。

![图片描述][https://cdn.chenhow.com/laravel-homestead-install/5.png]

## Hello World

### 添加 index.html

在 `C:\workspace\code` 目录下添加 `index.html` 文件，内容只要一个简单的 `hello world` 即可。创建成功后，文件会自动同步到 `Homestead` 虚拟主机上。

### 添加 hosts

用 Nodepad++ 打开 `C:\Windows\System32\drivers\etc\hosts` 文件，添加如下代码：

```
192.168.10.10 homestead.test
```

### 只差一步

浏览器输入 `http://homestead.test` 

![图片描述][https://cdn.chenhow.com/laravel-homestead-install/6.png]

## Hello Laravel

接下来把 Laravel 部署到虚拟机中去，就跟完成 hello world 一样，会稍微复杂一点点。

### 下载源码

教程中的示例源码我们放在了 github 上，地址：https://github.com/ibrandcc/api-tutorial-source

在 `c:\workspace` 目录下右键 `Git Bash Here` ，打开命令窗口，执行如下代码来 `clone` 源码。

```
git clone https://github.com/ibrandcc/api-tutorial-source.git
```

![clipboard.png](https://cdn.chenhow.com/laravel-homestead-install/7.png)


执行完毕后会多出一个 `api-tutorial-source` 目录。

### 配置 Homestead.yaml

**新增文件同步目录**

在 `folders:` 下添加如下代码
```
    - map: C:/workspace/api-tutorial-source
      to: /mnt/www/api.ibrand.test
```

把源码目录映射同步到虚拟主机上的 `/mnt/www/api.ibrand.test` 目录下。

**增加虚拟主机**

在 `sites:` 下添加如下代码

```
    - map: api.ibrand.test
      to: /mnt/www/api.ibrand.test/public
```

> 请注意文件中的空白处必须是空格键打出来的空格，不可用 Tab 键。

添加完成后，`Homestead.yaml` 文件内容如下：

```
---
ip: "192.168.10.10"
memory: 2048
cpus: 1
provider: virtualbox

authorize: ~/.ssh/id_rsa.pub

keys:
    - ~/.ssh/id_rsa

folders:
    - map: C:/workspace/code
      to: /home/vagrant/code
    - map: C:/workspace/api-tutorial-source
      to: /mnt/www/api.ibrand.test
sites:
    - map: homestead.test
      to: /home/vagrant/code
    - map: api.ibrand.test
      to: /mnt/www/api.ibrand.test/public

databases:
    - homestead

```

### 重载 `Homestead.yaml`

在更改后，需要通过 `vagrant reload --provision` 命令重启虚拟主机并且重载 `Homestead.yaml` 中的配置信息。

![clipboard.png](https://cdn.chenhow.com/laravel-homestead-install/8.png)

### 配置 Xshell 进入虚拟机

启动之前安装的 [Xshell][12] 软件

![clipboard.png](https://cdn.chenhow.com/laravel-homestead-install/9.png)

点击`新建`，添加新的会话配置
- 名称：homestead
- 主机：192.168.10.10

![clipboard.png](https://cdn.chenhow.com/laravel-homestead-install/10.png)

点击左侧的 `用户身份验证`，用户名和密码都输入：`vagrant`

![clipboard.png](https://cdn.chenhow.com/laravel-homestead-install/11.png)

点击确定按钮，保存设置。


![clipboard.png](https://cdn.chenhow.com/laravel-homestead-install/12.png)

点击连接按钮，进行会话连接，第一次连接会弹出如下提示框，选择`接受并保存`

![clipboard.png](https://cdn.chenhow.com/laravel-homestead-install/13.png)

操作完成后，会成功登入虚拟机，登入成功后执行 `sudo bash` 命令切换到 `root` 账号


![clipboard.png](https://cdn.chenhow.com/laravel-homestead-install/14.png)

### 安装 Laravel

```
$ cd /mnt/www/api.ibrand.test/
$ composer install
$ cp .env.example .env
$ php artisan key:generate
```


![clipboard.png](https://cdn.chenhow.com/laravel-homestead-install/15.png)


### 添加 hosts

用 Nodepad++ 打开 `C:\Windows\System32\drivers\etc\hosts` 文件，添加如下代码：

```
192.168.10.10  api.ibrand.test
```

### 最后一步
浏览器输入 `http://api.ibrand.test` 

![clipboard.png](https://cdn.chenhow.com/laravel-homestead-install/16.png)


## 总结

过程稍微有点复杂，但是搭建好这个环境可以避免后续再开发过程中的很多问题，特别是开发完成后部署到生产服务器，几乎是不会有兼容性的问题。这一点在 iBrand 产品各个客户的生产环境上已经得到验证，而且这也是 Laravel 官方推荐的开发方式，所以值得大家去掌握。

对于刚入门的初学者来说可能不会用 vagrant ，也不懂其中的原理，因为篇幅原因没办法对所有的细节说明到位，只需要暂时知道出现的命令的作用和意义，更多的用法可以通过自己的探索去学习掌握，而且后续教程中也会慢慢讲到更多的知识点。

> 有任何问题欢迎咨询 : )

[1]: https://segmentfault.com/a/1190000013327701
[2]: https://segmentfault.com/a/1190000013328013
[3]: https://segmentfault.com/a/1190000013327628
[4]: https://segmentfault.com/a/1190000013328888
[5]: https://segmentfault.com/a/1190000013328919
[12]: https://segmentfault.com/a/1190000013327628