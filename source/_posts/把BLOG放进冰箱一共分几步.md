---
title: 把BLOG放进冰箱一共分几步…
date: 2018-08-13 09:54:59
tags:
  blog
  atom
---


开门篇，七叔简历还没准备好，就先写写怎么搭的这个博客站吧。
`marsbase.xyz = GitHub + Hexo + Vagrant + Atom`
看了这个等式就知道是个啥东东的猿/媛，就甭点开，费skr时间啦。如果有兴趣搭建自己的博客，又不太清楚从何下手，那么此篇或许可能大概有点参考作用……

<!--more-->

---

<br/>
# 0. 为何而建

[知乎](https://www.zhihu.com/)、[简书](https://www.jianshu.com)、[掘金](https://juejin.im)、[思否](https://segmentfault.com)等等，如今写博文的基地挺多，注册一个账号就能开始码，Why Not？

<del>因为，幸福都是奋斗出来的！</del> 人生在于体验，七叔比较好这一口，所以选择自己建站，既能练个手又能依自己喜好定页面的样式，何乐而不为。

啥？您说<del>如何在网上装X</del>分享的问题？生成sitemap提交给百度收录呀，可以一条龙了有没有！

本篇就不涉及搜索引擎收录问题了哈，大而全的文章太拖泥带水，怕您没耐心看。

<br/>
# 1. 本地准备

七叔是在Mac上虚拟ubuntu/trusty64进行环境部署的，因此下文都是基于这个环境写。Windows用户也可以看，因为思路上大同小异，只不过有些操作细节上不够具备针对性。

### 1.1 直接部署还是包个虚拟机？

这个问题没有唯一答案，视具体情况决定。七叔这样考虑：
- 如果物理机环境比较简单，偏运营/设计类取向，一般不需要用CLI，那么建议打开terminal直接安装在Mac上，而且可以直接跳到1.3了；
- 如果是偏猿/媛取向，七叔打赌您的环境一定比较复杂，Python/g++/JDK ···，现在为了搭博客再来个npm（js系玩家请忽略），233，推荐尝一下Vagrant吧。

### 1.2 Vagrant极简部署虚拟机

[Vagrant](https://www.vagrantup.com)是基于Ruby的虚拟机管理工具，比较推荐的组合是与[VirtualBox](https://www.virtualbox.org)搭配，当这两个都安装完成后，执行如下：
```
$ mkdir vmBlog  #用来放虚拟机配置文件以及启动的目录
$ cd vmBlog
$ vagrant init  #初始化配置文件
$ vim Vagrantfile  #修改配置文件
$ vagrant up  #启动虚拟机
```

**简单不？和把大象塞进冰箱一样对吧？放心，后续的步骤也都是很简单的 Y(^_^)Y**

这里补充一些细节：
* 关于Vagrantfile该配置文件，建议初级设置就把下面3条配置项的注释去掉并改一改，然后跑起来开心地用吧

```
config.vm.box = "ubuntu/trusty64"  #可以换别的操作系统，官方有很多可选box
config.vm.network "private_network", ip: "192.168.33.91"  #配置了内部网络，方便登录、测试、传文件等等
config.vm.synced_folder "./data", "/home/vagrant/"  #将宿主机目录（data目录需要自己先创建）映射到虚拟机的家目录，方便管理文件
```

* 关于vagrant命令的常用简单操作选项


```
$ vagrant ssh  #虚拟机up之后可以ssh进入命令行界面
$ vagrant halt #把虚拟机关了
$ vagrant destroy  #把虚拟机删了
```

### 1.3 一波操作安装Git和Hexo
哔哔完虚拟机，开始一波流安装。

```
$ sudo apt-get update  #更新软件源
$ curl -sL https://deb.nodesource.com/setup_8.x | sudo -E bash -  #目前nodejs官方稳定版本为v8，该命令指定了接下来要安装的版本以及安装源
$ sudo apt-get install -y nodejs  #安装nodejs，注意不要漏了上面那段命令，因为默认源的版本很低
$ sudo apt-get install -y git  #安装牛X的Git，GitHub必需品
$ sudo npm install -g hexo-cli  #安装hexo
$ hexo init blogBase  #在本地初始化并创建包含整个博客配置、模版、文章等等所有文件的目录，目录的名字可以随客官高兴，这里七叔用blogBase举例
```

ok，这一波把该装的基本都装完了。补充一些细节，就是在git和node安装之后，建议先检查一下是否安装成功以及版本有没有问题。

```
$ git  #如果返回了命令使用帮助信息，说明安装成功
$ node -v  #返回的应该是v8.11.3，或者至少是v8开头的
$ npm -v  #返回的应该是5.6.0，这个版本号比较关键，因为hexo很多模版需要基于3.0以上版本的npm
```

本阶段最后一步，在`blogBase`目录下执行`hexo server`或者简写`hexo s`，正常情况下将有这样的返回：

```
INFO  Start processing
INFO  Hexo is running at http://localhost:4000/. Press Ctrl+C to stop.
```

然后，当您在浏览器上打开 http://192.168.33.91:4000/ 这个地址，看到这个画面：

{% asset_img hexo_init_index.png %}

那么恭喜您，本地安装已经完成了！

<br/>

# 2. 线上准备


<br/>

# 3. 开始写吧

<br/>

建议执行`dpkg-reconfigure tzdata`修改时区（默认为0时区，Asia/Shanghai在+8），方便创建博文时自动准确记录当地时间。

hexo官方有很多不错的[themes](https://hexo.io/themes/)可以选择，本站就是套用了其中一个非常简洁的叫做[apollo](https://github.com/pinggod/hexo-theme-apollo)的模版。其他值得推荐的themes，如[next](https://github.com/theme-next/hexo-theme-next)是个功能很强大的模版，还有[cactus](https://github.com/probberechts/hexo-theme-cactus)是全响应式、多种素色可换、清爽型模版，<del>也是七叔的菜</del>。
