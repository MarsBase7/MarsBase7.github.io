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

七叔是在Mac上虚拟Centos/7进行环境部署的，因此下文都是基于这个环境写。Windows用户也可以看，因为装好虚拟机之后的步骤都是一样的。那麽，七叔要开车了……

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

### 2.1 GitHub借坑搭窝

其实放到公网上有很多方案，例如租台云服务器，把操作系统部署、数据库安装、web服务安装、代码调试等等一顿全撸的折腾方案，当然也有零成本超便捷的躺鸡方案，七叔认为用Github+Hexo就有这么神奇。

首先，七叔默认您已经有了[GitHub](https://github.com)账号，并且掌握了git的基本使用（如果还没咋接触过git，可以查看[帮助](https://help.github.com)，这么牛的工具了解一下），那么请登录，接下来只需要在repository管理页面上新建一个以`NAME.github.io`作为固定格式命名且`NAME`就是您GitHub账号名的repository就完事儿了。有点拗口哈，请看下图：

{% asset_img github_set.png %}

完成后在本地`blogBase`目录下进行git相关配置，双引号内填入您的账号相关信息：
```
$ git config --global user.email "you@example.com"
$ git config --global user.name "Your Name"
```
修改`_config.yml`文件的deploy配置：
```
deploy:
  type: git
  repo: https://github.com/YOURNAME/YOURNAME.github.io.git
  branch: master
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

然后回到本地，在`blogBase/source/`目录下新建一个`CNAME`文件（注意文件名必须大写），文件内容为私有域名地址（下面以七叔的域名举例）。新建文件后再次进行部署：
```
$ echo 'www.marsbase.xyz' > CNAME
$ hexo clean && hexo g && hexo d
```

好了，现在可以打开您的域名，duang~~，再次出现了您的博客，您的专属博客！

### 2.3 搜索引擎收录

您对于狐朋狗友们的膜拜还不够满意，您想让狐朋的狐朋们以及狗友的狗友们都能在搜索引擎上搜寻到您的神采飞扬。对于如此不要脸的需求，七叔认为`0 error(s),0 warning(s)`。

搜索引擎收录其实是个挺讲究的事，对于想要精心经营个人站点的同学，值得深入探究。本篇只涉及搜索引擎收录的几个必要的操作过程，不进行详细展开。
* [百度站长](https://ziyuan.baidu.com/)、[Google Search Console](https://www.google.com/webmasters/tools/home?hl=zh-CN) 都可以进行收录（谷歌该怎么登录你懂的），收录前有必要的域名验证环节，跟着提示要求办就行了。
* **非常抱歉地通知您：** 百度无法收录`*.github.*`相关域名，私有域名映射之后能否被收录七叔没尝试，如果想被百度收录，建议将博客部署到[coding](https://coding.net/)上，coding的使用习惯与github类似，是个相当不错的国产代码托管平台。
* 七叔是把本站收录到了Goole Search Console上，收录效率非常快，当天就能在google上搜到`www.marsbase.xyz`

{% asset_img search_console.png %}

搜索引擎收录过程中需要提供个人站点地图，因此收录之前建议安装sitemap插件，安装完插件之后，每次hexo部署都会在`blogBase/public/`目录下自动生成`sitemap.xml`文件：
```
$ npm install hexo-generator-sitemap --save
```

线上准备部分到此告一段落，往后余生，您已走上了奔向“网红”的康庄大道……


# 3. 开始写吧

准备工作哔哔了那么多，**写**才是博客的核心。

### 3.1 <del>MA</del>SSAGE该用牛刀

上文所搭建的环境，需要用Markdown编辑博文，而且Markdown也是目前比较流行的写作语言，它可以通过简单的标记语法使文本内容具备相应的格式和形式，有很多商业的和开源的文本编辑器支持Markdown语法。这些编辑器各有千秋，七叔不打算装模作样做个对比分析，因为用过了才知道。

七叔只将编写本文所采用的编辑器（估计以后也只用它）推荐给大家，Markdown神器——[Atom](https://atom.io/)了解一下。此前七叔在编写github项目文档的时候用过几个编辑器（包括iA Writer），但是接触了Atom之后感觉真的好用，强力推荐！

下载&安装完Atom之后，建议安装以下这些插件（ **setting -> install** ）：
* `markdown-preview-plus`
* `markdown-scroll-sync`
* `language-markdown`
* `markdown-table-editor`

此外还有`markdown-image-paste`图片粘贴插件也是相当的牛X，但是由于其执行方式与hexo的图片路径匹配方式不太一致，所以这里就不作为重点推荐了。

* [x]工欲善其事，必先利其器

### 3.2 BLOG可以更个性

“博客的样式我看厌了” “我想要换另类的风格” “给本宫来个极致的简洁”

Hey, have U heard of **THEME**s, homie ?

{% asset_img hexo_themes.png %}

没错，hexo官网上有很多不错的[themes](https://hexo.io/themes/)可以选择，各种口味，总有一款适合您。其中值得推荐的，如[next](https://github.com/theme-next/hexo-theme-next)是个功能很全很强大的主题，还有[cactus](https://github.com/probberechts/hexo-theme-cactus)是全响应式、多种素色可换、清爽型主题，<del>都是七叔的菜</del>。但七叔最终还是选择了hexo的默认主题landscape，相对简洁、功能全面，更主要的是，花太多时间在主题样式的研究上，不如将博文好好打磨成精品。

更换主题的操作很简单（以next为例）：
* 将主题下载到`blogBase/themes`目录下
* 修改`blogBase/_config.yml`文件，指定要使用的主题

```
$ git clone https://github.com/theme-next/hexo-theme-next themes/next
$ sed -ie 's/theme: landscape/theme: next/g' _config.xml
```

有精力自己改主题的同学，七叔用的landscape就有先行者写了详细[调教攻略](https://www.jianshu.com/p/b96fd206571a)，供参考。据说相当多的hexo主题是在landscape基础上改出来的，足见其经典。

* [x]巧笑倩兮，美目盼兮

### 3.3 多台设备切换怎么破

首先，哥/姐，您好有钱，可以在多台设备上切换。笔记本上写了标题和提纲，换台式机截几个图写几段code，出门旅游不忘在macbook air上留住当时的好心情……爽哉爽哉！

言归正传，多设备之间需要同步的其实就是本地hexo主文件夹下的文件，那么思路就相对明确了：
* 对`blogBase/`目录进行git初始化并新建分支（分支名用**hexoBase**举例）
* 将新分支添加到您在2.1阶段创建的repository下
* 从A设备push到GitHub，
* 切换到B设备，首次clone，之后pull到本地
* 切换到C设备、D设备……N设备，均同上

#在A设备上操作
```
$ git init
$ git add .
$ git commit
$ git branch hexoBase
$ git checkout hexoBase
$ git remote add origin https://github.com/YOURNAME/YOURNAME.github.io.git
$ git push origin hexoBase
```
#在B设备上操作
```
$ git clone https://github.com/YOURNAME/YOURNAME.github.io.git
$ git pull origin hexoBase
```
用GitHub实现多设备同步的方式需要您对git的原理及操作有一定的了解，不小心容易自挖版本覆盖的陷阱，尝鲜的旁友谨慎选择。

* [x]刀枪剑戟斧钺钩叉，烧饼馒头包子麻花

---

# Summary
写到最后了，容许七叔在片尾装一把洋气，对全文做个聚合：

|KEY|Num|
|-|-|
|执行Bash命令|42条|
|安装必要软件|8个|
|修改配置文件|3个|
|操作涉及网站|4个|

收工，希望本文对您有所帮助，祝开博愉快！God bless your BLOG！
