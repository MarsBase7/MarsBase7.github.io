---
title: 5分钟拥有私人BLOG站
date: 2018-08-25 17:01:43
tags: web
---


开门篇，七叔简历还没准备好，就先写写怎么搭的这个博客站吧。

As we know，搭建一个完整的博客站是个不小的工程，安装、部署一把梭，再快的速度，半天总是需要的。其实，博客的核心部分，通过例如**Hexo**这样的强大框架，真的可以在5分钟完成搭建。


<!--more-->

---
<Br/>
## 总概
本来七叔把[marsbase.xyz](http://www.marsbase.xyz)的整个博客搭建过程写成了一整篇博文，后来感觉篇幅有点太长了，影响阅读效果，就把整个文档拆分成了核心主体部分以及几个番外篇。

本篇就是核心主体部分，所谓的5分钟搭建博客站（网速、手速双快的jr可能都不需要5分钟）。

番外篇主要包括以下内容（后续将逐渐补全）：
- 服务器的准备
- 因特网部署
- 搜索引擎收录
- 关于“写”
- 一些简单的配置
- 多终端同步编辑

*终于XXXX年XX月XX日完成补全*

整个博客站的安装配置过程，也就是本系列的这些文章所涉及的环境，都是在**Centos7**操作系统下进行的。如果旁友们用的是其他版本的Linux或者OSX，基本上操作大相径庭；如果是Windows系列的用户，那么所需要的软件还是一样，只不过安装细节有所区别，本文不做赘述，<del>反正未来是Linux的。

---
<Br/>
## 5分钟记时开始……
博客核心部分的搭建其实是“从0到1”的过程，主要包含下面5个简单的步骤：
* 安装NVM
* 安装NodeJS
* 安装Hexo
* 站点目录初始化
* 站点启动测试

平均每个步骤耗时在1分钟左右。本文接下来的内容，就是具体阐述每个步骤的实操经历。

<Br/>

### 安装NVM
…… 1st min ……

[NVM](https://nvmexpress.org/)即`Node Version Manager`，是强大的NodeJS管理工具，安装便捷，免除了在安装NodeJS时需要手工下载、解压、配置等一堆麻烦费时的工作。是对博客搭建过程进行提速的关键。

使用以下命令进行安装：
```
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.11/install.sh | bash
```
或者wget方式：
```
wget -qO- https://raw.githubusercontent.com/creationix/nvm/v0.33.11/install.sh | bash
```
安装完成后检查：
```
command -v nvm
```
如果返回的是`nvm: command not found`，可以执行`source ~/.bashrc`，然后再重复上一步检查操作，确认安装完成后可进行下一个步骤。

<Br/>

### 安装NodeJS
…… 2nd min ……

安装NodeJS前，可以用NVM查看可安装的版本（非必要）：
```
nvm ls-remote
```
建议挑选标注有`Latest LTS`字样的最新稳定版本开始安装，比如七叔使用的`v8.12.0`：
```
nvm install v8.12.0
```
老套路，安装完成后检查：
```
node -v
npm -v
```
如果返回了版本号就表示安装完成，然后继续下一步操作。

<Br/>

### 安装Hexo
…… 3rd min ……

1条命令+1分钟等待即可：
```
npm install -g hexo-cli
```
安装过程中可能会有报错，请根据报错提示操作，或重新执行上一条命令。未报错或报错消失后是<del>没完没了的</del>检查环节：
```
hexo -v
```
如果返回了一堆包含了`hexo-cli`的软件包的版本号，就表示安装完成，go on……

<Br/>

### 站点目录初始化
…… 4th min ……

假设我们把站点所在的目录取名为`myblog`，之后博客站的所有文件都将放置在该目录下。在`myblog`的上级目录下执行hexo的初始化命令：
```
hexo init myblog
```
进入`myblog`目录，由npm自动执行必要的插件安装：
```
cd myblog
npm install
```
没有检查环节？很意外是不是？往下走……

<Br/>

### 站点启动测试
…… 5th min ……

其实最后一个步骤就是整体的检查环节。

在`myblog`目录下启动测试服务：
```
hexo server
```
正常情况下将有这样的返回（如有异常请根据提示处理或检查前面的步骤是否都正常完成）：
```
INFO  Start processing
INFO  Hexo is running at http://localhost:4000/. Press Ctrl+C to stop.
```
打开浏览器，访问http://localhost:4000/ ，如果能看到这个画面，表示我们已经成功搭建了一个博客站：

{% asset_img hexo_init_index.png %}

---
<Br/>

## 未完待续

本篇基于Hexo框架，在短短5分钟的时间内，从无到有完成了BLOG站核心部分的搭建，但是这样一个博客站还远没有达到实际可用的程度，还有相当一部分的周边工作需要处理。

如果对搭建自己的博客站有兴趣，又不太清楚该从何下手，那么本篇以及一系列番外篇或许/可能/大概/有点参考作用……

### 番外篇传送门（陆续补全中）：
- [服务器的准备](http://www.marsbase.xyz/BLOG%E7%AB%99%E6%90%AD%E5%BB%BA-%E7%95%AA%E5%A4%961---%E6%9C%8D%E5%8A%A1%E5%99%A8%E7%9A%84%E5%87%86%E5%A4%87.html)
- 因特网部署
- 搜索引擎收录
- 关于“写”
- 一些简单的配置
- 多终端同步编辑

---

<Br/>
but in **The End**, it doesn't even matter
