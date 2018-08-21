---
title: 把BLOG放进冰箱一共分几步
date: 2018-08-17 15:59:59
tags: web
---


开门篇，七叔简历还没准备好，就先写写怎么搭的这个博客站吧。
```
marsbase.xyz = GitHub + Hexo + Vagrant + Atom
```

看了这个等式就知道是个啥东东的猿/媛，就甭点开，费skr时间啦。如果有兴趣搭建自己的博客，又不太清楚从何下手，那么此篇或许/可能/大概/有点参考作用……

<!--more-->

---


# 0. 为何而建

[知乎](https://www.zhihu.com/)、[简书](https://www.jianshu.com)、[掘金](https://juejin.im)、[思否](https://segmentfault.com)等等，如今写blog的基地挺多，注册一个账号就能开始码码码，如此快餐，Why Not? <del>因为，幸福都是奋斗出来的！</del> 人生在于体验，七叔比较好这一口，所以选择自己建站，既能练个手又能依自己喜好定页面的样式，何乐而不为。啥？您说自己建站<del>如何在网上当网红装X</del>如何分享的问题？喏，生成sitemap提交给搜索引擎收录呀，感觉一条龙了有没有！

本篇就把整个过程撸一遍，可能篇幅上冗长，祝福您有那个耐心看完……


# 1. 本地准备

七叔是在Mac上虚拟ubuntu/trusty64进行环境部署的，因此下文都是基于这个环境写。Windows用户也可以看，因为装好虚拟机之后的步骤都是一样的。那麽，七叔要开车了……

### 1.1 直接部署还是包个虚拟机？

这个问题没有唯一答案，视具体情况决定。七叔这样考虑：
- 如果物理机环境比较简单，偏运营/设计类取向，一般不需要用CLI，那么建议打开terminal直接安装在Mac上（windows可用git-bash），一把梭；
- 如果是偏猿/媛取向，七叔掐指算您的环境一定比较复杂，Python/g++/JDK ···，现在为了搭博客再来个npm（js系玩家请忽略），233，推荐尝一下Vagrant吧。

### 1.2 Vagrant极简部署虚拟机

[Vagrant](https://www.vagrantup.com)是基于Ruby的虚拟机管理工具，比较推荐的组合是与[VirtualBox](https://www.virtualbox.org)搭配，当这两个都安装完成后，执行如下操作：
* 创建用来放虚拟机配置文件以及控制的目录
* 初始化配置文件
* 修改配置文件（下文有建议方案）
* 启动虚拟机

```
$ mkdir vmBlog
$ cd vmBlog
$ vagrant init
$ vim Vagrantfile
$ vagrant up
```

**简单不？和把大象塞进冰箱一样简单对吧？放心，后续的步骤也都是很简单的 Y(^_^)Y**

关于Vagrantfile该配置文件，建议初阶设置就写下面2条配置项即可，然后跑起来开心地用吧：
* box的配置项，七叔用centos<del>不会告诉你七叔是红帽系的</del>，当然可以根据您的喜好换别的操作系统，官方有很多box可选
* 私有网络选项，配置一个<del>吉利的</del>内部网络地址，方便登录、测试、传文件等

```
config.vm.box = "centos/7"
config.vm.network "private_network", ip: "192.168.33.91"
```

关于vagrant命令的常用简单操作选项，可以执行`vagrant -h`查看，本篇就不再赘述。


### 1.3一波操作安装Git和Hexo
哔哔完虚拟机，不管您是以什么方式登录，接下来的操作都是在虚拟机上执行。来吧，开始一波流安装：
* 更新repo，安装开箱必备的软件（centos的初始环境就是那么<del>任性</del>纯净）
* nodejs的安装建议直接下载[软件包](https://nodejs.org/en/download/)，目前稳定版本是8，用curl和yum的方式貌似不太靠谱
* 解压缩并且放到/usr/local目录下， 给系统指定node的路径并加载（下文有建议方案）
* 用npm安装hexo和git插件，在本地初始化并创建包含整个博客配置、模版、文章等等所有文件的目录，目录的名字可以随客官高兴，这里七叔用**blogBase**举例

```
$ sudo yum update
$ sudo yum install -y git vim wget
$ wget https://nodejs.org/dist/v8.11.3/node-v8.11.3-linux-x64.tar.xz
$ tar xvJf node-v8.11.3-linux-x64.tar.xz
$ sudo mv node-v8.11.3-linux-x64.tar.xz /usr/local/node
$ sudo vim /etc/profile
$ source /etc/profile
$ npm install -g hexo-cli
$ hexo init blogBase
$ cd blogBase
$ npm install
$ npm install hexo-deployer-git --save
```

ok，这一波把该装的基本都装完了。补充一些细节，就是在git和node安装之后，建议先检查一下是否安装成功以及版本有没有问题：
* 执行`git`如果返回了命令使用帮助信息，说明安装成功
* 执行`node -v`返回的应该是v8.11.3，或者至少是v8开头的
* 执行`npm -v`返回的应该是5.6.0，这个版本号比较关键，因为hexo很多模版需要基于3.0以上版本的npm

到了本环节最后一步，在`blogBase`目录下执行`hexo server`或者简写`hexo s`，正常情况下将有这样的返回：

```
INFO  Start processing
INFO  Hexo is running at http://localhost:4000/. Press Ctrl+C to stop.
```

然后，当您在浏览器上打开 http://192.168.33.91:4000/ 这个地址(如果是在物理机上直接安装，就是打开http://localhost:4000/ )，看到这个画面：

{% asset_img hexo_init_index.png %}

那么恭喜您，本地安装已经完成了！

# 2. 线上准备

上一阶段完成了博文编辑框架的搭建，您已经可以在自己的电脑上看到博客的样子，满心喜悦，自恋爆棚，“但是那帮狐朋狗友们看不到啊？！”，您又沉浸在了装X未遂的万分悲痛之中。莫捉急，接下来的内容就是把博客放到公网上去。

### 2.1 GitHub搭小窝

其实放到公网上有很多方案，例如租台云服务器，把操作系统部署、数据库安装、web服务安装、代码调试等等一顿全撸的折腾方案，当然也有零成本超便捷的躺鸡方案，七叔认为用Github+Hexo就有这么神奇。

首先，七叔默认您已经有了[GitHub](https://github.com)账号，并且掌握了git的基本使用（如果还没咋接触过git，可以查看[帮助](https://help.github.com)，这么牛的工具了解一下），那么请登录，接下来只需要在repository管理页面上新建一个以`NAME.github.io`作为固定格式命名且`NAME`就是您GitHub账号名的repository就完事儿了。有点拗口哈，请看下图：

{% asset_img github_set.png %}

完成后在本地`blogBase`目录下进行如下配置，双引号内填入您的账号相关信息：
```
$ git config --global user.email "you@example.com"
$ git config --global user.name "Your Name"
```
接下来，3条命令开启线上博客之旅：
```
$ hexo clean
$ hexo generate
$ hexo deploy
```
根据git提示填入账号密码，等待部署完成。在浏览器上打开GitHub的blog地址 https://NAME.github.io ，duang~~，您的博客已经出现在了公网上！

### 2.2 穿上私有域名

现在您的狐朋狗友们已经可以看到您的博客，但是您觉得URL拖着`github.io`的小尾巴不够个性，fine，来个私有域名吧。

七叔默认您已经有了自己的域名并且完成了备案。如果您还没有这些，您可以在自己认为靠谱的公有云（七叔目前用过阿里云和腾讯云）网站上注册一个心仪的域名并提交备案申请，这里七叔给您泼个冷水，备案流程略费神，如果对于URL个性化的意愿不太强烈，那就先用GitHub的地址凑合着吧。

本博客的域名`marsbase.xyz`是七叔在腾讯云上注册的，下图就是在腾讯云上对该域名进行解析设置：

{% asset_img cname_set.png %}

然后回到本地，在`blogBase/source/`目录下新建一个`CNAME`文件（注意文件名必须大写），文件内容为私有域名地址。新建文件后再次进行部署：
```
$ echo 'marsbase.xyz' > CNAME
$ hexo clean && hexo g && hexo d
```

好了，现在可以打开您的域名，duang~~，再次出现了您的博客，您的专属博客！

### 2.3 搜索引擎收录



```
npm install hexo-generator-sitemap --save
npm install hexo-generator-baidu-sitemap --save
```






# 3. 开始写吧

记下clone地址`https://github.com/NAME/NAME.github.io.git`，

hexo官方有很多不错的[themes](https://hexo.io/themes/)可以选择，本站就是套用了其中一个非常简洁的叫做[apollo](https://github.com/pinggod/hexo-theme-apollo)的模版。其他值得推荐的themes，如[next](https://github.com/theme-next/hexo-theme-next)是个功能很强大的模版，还有[cactus](https://github.com/probberechts/hexo-theme-cactus)是全响应式、多种素色可换、清爽型模版，<del>也是七叔的菜</del>。


那么现在说到编辑器了，七叔隆重推荐编写本文所采用的markdown神器[Atom](https://atom.io/)。此前编写github项目文档的时候用过几个编辑器，但是接触了Atom之后感觉真的好用，强力推荐！
安装完Atom之后，建议安装以下这些插件：
* `markdown-preview-plus`
* `markdown-scroll-sync`
* `language-markdown`
* `markdown-table-editor`

此外还有`markdown-image-paste`图片粘贴插件也是相当的牛X，但是由于其执行方式与hexo的图片路径匹配方式不太一致，所以这里就不作为重点推荐了。

想自己改模板？没问题呀，七叔用的landscape就有大牛写了详细调教[攻略](https://www.jianshu.com/p/b96fd206571a)
