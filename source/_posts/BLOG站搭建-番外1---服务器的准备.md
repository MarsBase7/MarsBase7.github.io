---
title: BLOG站搭建（番外1）--服务器的准备
date: 2018-09-06 11:27:31
tags: web
---

《5分钟拥有私人BLOG站》的第一个番外篇，主要聊聊Hexo环境部署在本地还是虚拟机的问题，以及基于**Vagrant**的虚拟机部署推荐方案。

<!--more-->

---

七叔是在Mac上虚拟Centos/7进行环境部署的（抱歉，七叔是红帽系），因此下文都是基于这个环境写。Windows用户也可以看，因为部署虚拟机的步骤基本都是一样的。那麽，七叔要开车了……

<Br/>

## 直接部署还是包个虚拟机？

这个<del>可能具备散群能力的</del>问题没有唯一答案，视具体情况决定。七叔简单粗暴地认为：
- 如果物理机环境比较简单，偏运营/设计类取向，一般不需要用CLI，那么建议打开terminal直接安装在Mac上（windows可用git-bash），一把梭；
- 如果是偏猿/媛取向，七叔掐指算您的环境一定比较复杂，Python/g++/JDK ···，现在为了搭博客再来个npm（js系玩家请忽略），233，**推荐尝一下Vagrant吧**。

大概，点开本篇的旁友们，还是希望使用虚拟机进行部署的吧，本问题就不展开讨论了哈。收！

<Br/>

## Vagrant极简部署虚拟机

[Vagrant](https://www.vagrantup.com)是基于Ruby的虚拟机管理工具，比较推荐的组合是与[VirtualBox](https://www.virtualbox.org)搭配，当这两个都安装完成后，执行如下几步简单的操作即可完成虚拟机部署：

### 1. 创建虚拟机配置目录并初始化
假设我们将虚拟机配置文件所在的目录取名为`blogVM`，进入该目录对vagrant配置文件初始化：
```
mkdir blogVM
cd blogVM
vagrant init centos/7
```
上述命令执行后，会在`blogVM`目录下生成`Vagrantfile`文件，该文件存放了虚拟机的启动配置信息，接下来对其进行修改。

### 2. 修改配置文件
虚拟机配置文件有很多配置项可以修改，虚拟机启动时会根据配置项进行一系列的自动化安装部署，非常方便。为了配合博客站的需求快速使用，七叔在这里只推荐了内网配置项的修改（便于文件传输和修改），其他的很多配置项虽然不提及但也值得深入研究。

使用任意编辑器修改`Vagrantfile`文件（例如vim）：
```
vim Vagrantfile
```

在`Vagrantfile`文件中配置内网地址（ip地址自定）：
```
config.vm.network "private_network", ip: "192.168.33.91"
```

其他配置项就按默认的即可，对于博客站本地环境来说完全够用。

### 3. 启动虚拟机
非常简单的一条命令启动虚拟机：
```
vagrant up
```
由于Vagrant是基于box（可视为操作系统的镜像）来启动虚拟机，因此如果第一次启动，Vagrant会花费一定时间来下载这个box，当然我们也可以提前下载好，具体参阅[官方box](https://app.vagrantup.com/boxes/search)。

启动命令之后可以通过status命令检查启动状态：
```
vagrant status
```
如果返回`running (virtualbox)`，就表示虚拟机已正常启动。

随后，就可以直接登录该虚拟机进行操作了：
```
vagrant ssh
```

关于Vagrant命令的常用简单操作选项，可以执行`vagrant -h`查看，本篇就不再赘述。

<Br/>

## 可能会遇到的问题

这一部分比较有针对性，是centos7的官方box在第一次启动后，对于搭建博客站来说可能会遇到的一些小问题，其他的Linux版本也许会有同样的问题，仅做参考。

* #### **必要软件缺失的问题**

第一次启动的centos7，有很多软件未安装，repo源也没更新。为了方便后续的博客站搭建，七叔推荐执行以下操作：
```
sudo yum update
sudo yum install -y net-tools git vim wget ntp
```
部分软件安装完成后需要重启，例如ntp服务：
```
sudo systemctl restart ntpd
```

* #### **时区的问题**

一般情况下，操作系统默认使用UTC，如果想调整到北京时间，可以执行以下命令：
```
sudo ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
```
然后用`date`命令检查下时间是否调正确了。


* #### **中文显示异常的问题**

当我们在创建博文的时候，文章的名称如果包含中文，默认情况下centos7的terminal会将这些中文显示成乱码。当然这个问题不会影响博文的正常显示，只不过可能会要了强迫症的命，233。要解决这个问题，只需安装相应的中文支持包即可：
```
sudo yum install kde-l10n-Chinese
sudo yum reinstall glibc-common
```
现在可以在terminal中看到正常的中文显示了，比如刚才的`date`命令就返回了中文显示的时间信息。

* #### **无法通过ssh命令远程登录的问题**

这是centos7的初始安全策略，可以修改`/etc/ssh/sshd_config`文件的`PasswordAuthentication`参数为`yes`（默认为`no`）：
```
PasswordAuthentication yes
```
保存配置文件并重启sshd服务后就可以打开ssh远程登录功能：
```
sudo systemctl restart sshd
```
centos7官方box带的初始账号密码都为`vagrant`，现在可以用该账号以及在初始化时配置的内网ip地址来测试验证ssh登录了：
```
ssh vagrant@192.168.33.91
vagrant@192.168.33.91's password: vagrant
```

---

<Br/>
but in **The End**, it doesn't even matter
