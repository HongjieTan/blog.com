---
layout: article
title: 如何搭建自己的VPN——基于V2Ray和SSR
mode: immersive
header:
  theme: dark
article_header:
  type: overlay
  theme: dark
  background_color: '#203028'
  background_image:
    gradient: 'linear-gradient(135deg, rgba(34, 139, 87 , .4), rgba(139, 34, 139, .4))'
    src: https://s1.ax1x.com/2020/06/23/NNFAUA.jpg
key: 2020-5-4-11
tags: VPN
category: blog
date: 2020-5-4 11:40:00 +08:00
mermaid: true
sharing: true
aside:
  toc: true
show_author_profile: true
---

很多朋友因各种原因需要搭建一个自己私有的VPN但又不知道从何处下手，我将会在这里位大家带来用SSR和V2Ray搭建VPN的详细流程，让电脑小白也能用上自己的VPN。

<!--more-->

## 准备

一台可以上网的电脑，一点点英语基础。

## 下载Xshell

![Xhell](https://s1.ax1x.com/2020/06/24/NaLKPS.png)

因为我们需要一个软件用于连接海外的服务器，所以需要下载Xshell。进入Xshell官网的[个人与学校界面](https://www.netsarang.com/en/free-for-home-school/)填写名字和Email将Xshell Only选项勾选，你将会收到一封Email点击里面的连接即可免费下载。

## 购买一个VPS服务器

接下来我们就需要购买一海外台服务器来帮我们传输外面世界的数据，现在的VPS服务商种类繁多，不过搭建个人VPN还是简易选择国外的服务商，目前最便宜的服务商应该是[cloudcone](https://cloudcone.com/offers/),可以买到2.8刀一个月的服务器，而且活动频繁经常会有20刀一年的活动，再者就是[vultr](https://my.vultr.com/)拥有3.5刀一个月的服务器（他们也提供2.5刀的不过只支持IPv6），而且这两家实际收费都是按小时收费的，服务器被墙了也不用害怕。不过服务器这东西总体而言一分钱一分货，便宜永远是靠超售的不稳定换来的，如果你想更多的了解服务器的行情可以去[VPS推荐](https://www.vpstry.com/)了解。  
此处以cloudcone为例讲解：

1. 首先注册或者登录后进入主页,点击圈内的按钮，进入下一步
![home](https://s1.ax1x.com/2020/06/24/NdnPJS.jpg)

2. 因为cloudcone在国内被墙了所以需要修改一下地址，在**.com**后面加上**.cn**即可。

![change](https://s1.ax1x.com/2020/06/24/NdnSdP.jpg)

3. 进入服务器创建界面后，第一步选择centos下圆圈内的选项它自带有BBR加速可以省去后面一个步骤，第二部在Hostname后买你随意输入一个名字，第三步点击圆圈内的按钮开始创建服务器。

![vps](https://s1.ax1x.com/2020/06/24/NdnCi8.jpg)

4. 等待几分钟服务器的创建后，可以在服务器管理界面点击Access可以查看到HOST下的服务器IP地址，也会以邮件的形式发送给你,主机地址就在IP Address后面，密码就存放在Password的后面。

> Dear thj,
>
>We are pleased to tell you that your new instance has been deployed. Please note that it might take another several minutes to boot up and start pinging.
>
>**Instance Information**<br>
>>Hostname: hhhh.tt.com<br>
CPU: 1 cores<br>
RAM: 0.5 GB<br>
Disk: 10 GB<br>
IPs: 1 static addresses<br>
>
>**Instance** Credentials
>>IP Address: 173.82.208.194<br>
Username: root<br>
Password: 7jAo8E4Z4vHM<br>

## 开始搭建VPN

### 需要注意的坑

#### 服务器时间问题

有很多时候无法连接VPN并不一定是服务器被墙而是时间差距过大，特别的你如果使用V2Ray的动态端口一定要修改服务器时间不然必定无法使用。

{% highlight Bash linenos %}
date                                                                  
\cp -f /usr/share/zoneinfo/Asia/Shanghai /etc/localtime               
{% endhighlight %}

#### 防火墙问题

有时候防火墙也会成为我们无法连接的VPN的阻碍，需要我们手动关闭。

{% highlight Bash linenos %}
firewall-cmd --state                                           
systemctl stop firewalld.service                               
systemctl disable firewalld.service                            
{% endhighlight %}

### 用Xshell连接你的VPS

![Xhelladd](https://s1.ax1x.com/2020/06/24/NdZHw6.gif)

名称随意填写，主机填写你购买的VPS的IP地址，端口号填写你购买的VPS的端口号，用户名一般都是root，密码是你购买VPS后邮件收到的或界面上显示的密码。  
注意：如果你多次使用Xshell连接不上就证明你购买的服务器IP被墙了，你只能删除这个服务器了重新创建以达到更换可用IP的目的。当然VPS服务商搭建服务器是需要时间，一般网站上也会有提示，在服务器没有搭建成功时是无法连接成功的，所以还是需要一点点的耐心。

### 搭建V2Ray

我之所以首先讲解V2Ray的搭建主要因为本人更推荐使用V2Ray，V2Ray相比于SSR提供了更多的花样，比如动态端口、Nginx+TLS+WS+CDN等，不过这些手段也只是减少被墙风险不一定100%安全。

#### 在Xhell上输入一键搭建V2Ray的命令

{% highlight Bash linenos %}
bash <(curl -s -L https://git.io/v2ray.sh)                           
{% endhighlight %}

#### 命令行设置V2ray属性

选择安装后会选择传输协议，协议后缀在下面的命令行提示中都有，而且一般我们也就是使用基本的无后缀协议或者dynamicPort动态端口，我们主要就是需要注意一下前缀，其中TCP是最稳定的，mKCP使用的是UDP所以速度相对较快但不稳定，QUIC是谷歌弄的一个新的基于UDP的通信协议，一般情况选择TCP前缀的就好了。之后基本就是一路回车了。

![V2](https://s1.ax1x.com/2020/06/24/Nd1oXF.png)

#### BBR加速(如果你在购买VPS这一步购买了自带BBR的请跳过这一步)

你可以使用下面的命令测试是否有安装BBR，只要输出带有bbr就说明已经安装了。

{% highlight Bash linenos %}
lsmod | grep bbr                           
{% endhighlight %}

输入下面的代码后按提示敲几下回车就可以，然后就是等待服务器自己下载和安装了。

{% highlight Bash linenos %}
wget --no-check-certificate https://github.com/teddysun/across/raw/master/bbr.sh

chmod +x bbr.sh

./bbr.sh                        
{% endhighlight %}

#### 下载和配置客户端

1. 下载客户端

[PC端](https://github.com/v2ray/v2ray-core/releases)

[Android端](https://github.com/2dust/v2rayNG/releases)

IOS端：kitsunebi或Kitsunebi Lite（只不过都需要美区的Apple ID）

2. 导入配置

PC端可以使用命令生成URL链接，复制之后直接进入客户端导入。

{% highlight Bash linenos %}
v2ray url                           
{% endhighlight %}

手机端可使用命令生成一个二维码链接，在Chrome打开后用手机客户端扫码即可。

{% highlight Bash linenos %}
v2ray qr                          
{% endhighlight %}

### 搭建SSR

#### 在Xhell上输入一键搭建SSR的命令

SSR的搭建虽然选项比V2ray多但是很多选项都是以默认为主，而且容易理解，即使要自己选择的也都是可以随意进行选择，你只需要设置好端口和密码后，加密协议和混淆、插件等完全可以凭喜好选择也可以直接回车按默认选择，所以我就不做详细的介绍了。

{% highlight Bash linenos %}
yum -y install wget
wget -N --no-check-certificate https://raw.githubusercontent.com/ToyoDAdoubi/doubi/master/ssr.sh && chmod +x ssr.sh && bash ssr.sh                          
{% endhighlight %}

#### BBR加速(如果你在购买VPS这一步购买了自带BBR的请跳过这一步)

你可以使用下面的命令测试是否有安装BBR，只要输出带有bbr就说明已经安装了。

{% highlight Bash linenos %}
lsmod | grep bbr                           
{% endhighlight %}

输入下面的代码后按提示敲几下回车就可以，然后就是等待服务器自己下载和安装了。

{% highlight Bash linenos %}
wget --no-check-certificate https://github.com/teddysun/across/raw/master/bbr.sh

chmod +x bbr.sh

./bbr.sh                        
{% endhighlight %}

#### 下载和配置客户端

1. 下载客户端

[Windows端](https://github.com/shadowsocksr-backup/shadowsocksr-csharp/releases)

[Mac端](https://github.com/shadowsocksr-backup/ShadowsocksX-NG/releases)

[Android端](https://github.com/shadowsocksr-backup/shadowsocksr-android/releases/tag/3.4.0.8)

IOS端：shadowrocket（只不过都需要美区的Apple ID）

2. 导入配置

因为SSR一键搭建脚本在搭建成功后会直接返回URL和二维码链接，所以大家直接取用就可以了。

## 相关

- 首先感谢这位大佬，我初学自建VPN就是学习的他的文章，如果你还想学习一些新出现的还不太成熟的个人VPN技术也可以关注大佬的文章：[Alvin9999](https://github.com/Alvin9999/new-pac/wiki/ss%E5%85%8D%E8%B4%B9%E8%B4%A6%E5%8F%B7)
- 再要感谢那些甘愿冒着生命危险为我们开发这些技术的大佬们。
- 如果你想要进一步关注VPS市场可以看：[VPS推荐](https://www.vpstry.com/)
