---
layout: article
title: 使用GitHubPages+Jekyll+Text快速且免费的搭建个人博客
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
key: 2020-4-2-13
tags: 博客
category: blog
date: 2020-4-2 13:43:00 +08:00
mermaid: true
sharing: true
aside:
  toc: true
show_author_profile: true
---

## 创建GitHubPages仓库

如果没有github账号的话，请进入[github](https://github.com/)点击SignUp注册账号，点击右上角的YourRepositories然后点击New创建一个新的仓库，仓库名字为：你的GitHub账号.github.io  
然后你需要下载[git](https://git-scm.com/download/win)或者[github客户端](https://desktop.github.com/),如果使用github客户端的话直接进入客户端点击CloneRepositories将你创建的仓库克隆到本地，如果使用git的话需要使用git clone命令（xxxx为你的仓库的url链接）。

{% highlight Bash linenos %}
git clone xxxx                       
{% endhighlight %}

## 下载Text

因为你只需下载Text主题而不需要push所以你可以使用clone命令或者fork后用github客户端clone也可以进入[Text的仓库](https://github.com/kitian616/jekyll-TeXt-theme)直接下载zip包，然后将Text仓库内的所有内容全部复制到你自己的GithubPages仓库内。

## 修改_config.yml

进入你的本地仓库根目录打开_config.yml

### Site Settings设置

> => Site Settings
>##############################  
>text_skin: default # "default" (default), "dark", "forest", "ocean", "chocolate", "orange"  
highlight_theme: default # "default" (default), "tomorrow", "tomorrow-night", "tomorrow-night-eighties", "tomorrow-night-blue", "tomorrow-night-bright"  
url     : # the base hostname & protocol for your site e.g. https://www.someone.com  
baseurl : # does not include hostname  
title   : Your Site Title  
description: > # this means to ignore newlines until "Language & timezone"  
  Your Site Description

- 其中Text_shin和highlight_theme是设置主题样式的，可以根据喜好设置  
- url是设置域名的  
- baseurl可以不用设置  
- title是设置你的博客名字  
- description设置你博客的描述

这里重点讲一下域名，如果你不需要自己的域名可以直接使用GitHubPages自定义的域名也就是你刚刚创建的仓库的名字，如果你需要自己的域名就需要去各大域名服务商购买（注意：国内域名服务商购买域名需要备案），我建议大家需要的话可以去[namesilo](https://www.namesilo.com/)购买，其中最低价格大概1刀/年。下面我会讲解域名的购买和配置自己域名的步骤，如果不想要使用自己的域名可以跳过，不会对你的博客创建有任何影响。

1. 选购自己喜欢的域名

进入namesilo之后在搜索栏填写自己想要的域名前缀，然后搜索会在下方出现你输入的前缀+后缀组合，选择一个后点击购物车进行购买，namesilo可以使用支付宝和微信购买。

2. 使用DNS域名服务器

进入[DNSPod](https://www.dnspod.cn/)注册账号然后添加域名，然后给域名添加解析服务，直接添加两条CNAME类型的（@+CNAME和wwww+CNAME），地址都填写你创建的仓库名字即可。

3. 设置github仓库

进入你创建的GitHubPages仓库，点击Settings在GitHubPages栏中填写你刚刚购买的域名，然后将下方的钩打上开启HTTPS。

### Language and Timezone和Author and Social配置

> => Language and Timezone  
>##############################  
lang: # the language of your site, default as "en"  
timezone: # see https://en.wikipedia.org/wiki/List_of_tz_database_time_zones for the available values

> => Author and Social  
>##############################  
author:  
  type      : # "person" (default), "organization"  
  name      : Your Name  
  url       :  
  avatar    : # path or url of avatar image (square)  

- lang设置你的博客的语言，比如中文zh
- timezone设置你的博客的时区，如中国Asia/Shanghai
- Author必须设置的只有name：你的名字，avatar：你的头像的图床url（图床本人推荐[路过图床](https://imgchr.com/)）。type和url不用管它，后面还有一些配置自己按需设置即可。

### Sharing配置

> => Sharing  
>##############################  
sharing:  
  provider: false # false (default), "addtoany", "addthis", "custom"  
>
  > AddThis  
  addthis:  
    id: # AddThis pubid, e.g. ra-5xxxxxxxxxxx  

sharing:  provider:这里可以用来配置你的博客用于分享的组件，你可以简单的使用addtoany，或者使用addthis。但是addthis需要你自己进入[addthis官网](https://www.addthis.com/)，注册并且配置你自己的分享组件，而且在注册的时候可能需要翻墙，配置好你的addthis组件后将id复制并粘贴到配置文件即可。

### Comments配置

> => Comments  
>##############################  
comments:  
  provider: false # false (default), "disqus", "gitalk", "valine", "custom"  
>
  > Disqus  
  disqus:  
    shortname: # the Disqus shortname for the site  
>
  > Gitalk  
  # please refer to https://github.com/gitalk/gitalk for more info.  
  gitalk:  
    clientID    : # GitHub Application Client ID  
    clientSecret: # GitHub Application Client Secret  
    repository  : # GitHub repo  
    owner       : # GitHub repo owner  
    admin: # GitHub repo owner and collaborators, only these guys can initialize GitHub issues, IT IS A LIST.  
      # - your GitHub Id  
>
  > Valine  
  # please refer to https://valine.js.org/en/ for more info.  
  valine:  
    app_id      : # LeanCloud App id  
    app_key     : # LeanCloud App key  
    placeholder : # Prompt information  
    visitor     : # false (default)  
    meta        : # "[nick, mail, link]" (default) nickname, E-mail, Personal-site  

comments:  provider:这里用来配置你的博客的评论工具，你可选用disqus、gittalk、valine，我就简单的以gittalk为例来讲解如何配置。
1. 首先要创建一个GitHub仓库，名字可以随意设置
2. 然后你需要进入[github的工具配置页面](https://github.com/settings/apps/new)创建gittalk,谁便写一个名字，homepage和Authorization callback URL设置为你的博客域名其他不用管
3. 创建成功后你需要复制clientID和clientSecret粘贴到配置文件中，repository填写你刚刚创建的仓库名，owner和admin都是你的github账号。

### Pageview配置

> => Pageview  
>##############################  
pageview:  
  provider: false # false (default), "leancloud", "custom"  
>
>   Leancloud  
  leancloud:  
    app_id    : # LeanCloud App id  
    app_key   : # LeanCloud App key  
    app_class : # LeanCloud App class  

pageview:  provider:用来设置你的文章访问量
1. 你可将其配置为leancloud
2. 然后进入[leancloud官网](https://leancloud.cn/dashboard/applist.html#/apps)注册账号
3. 然后点击右上角控制台，然后点左边的应用-创建应用创建开发版
4. 然后点击你刚刚创建的应用点击存储，点击创建class
5. 设置一个名字然后创建并且将名字复制到app_class后
6. 之后点击左侧的设置然后点击应用key后复制app_id和app_key


### Analytics配置

> => Analytics  
>##############################  
analytics:  
  provider: false # false (default), "google", "custom"  
>
>   Google Analytics  
  google:  
    tracking_id : # Google Analytics id for the site  
    anonymize_ip: false # Anonymize IP tracking for Analytics  

analytics:  provider:这里用于配置你的站点信息统计，你可是设置成google使用谷歌的服务，先进入[google Analytics](https://analytics.google.com/)（这里需要翻墙），进入后创建服务然后将tracking_id复制到配置文件，anonymize_ip:设置为ture即可。（意义不大）

## 定制自己的logo

如果你需要自己定制logo，
1. 你需要替换_includes/svg/logo.svg
2. 然后进入[RealFaviconGenerator](https://realfavicongenerator.net/)点击Select your Favicon picture选择图片
3. 然后设置网站路径为/assets，点击Generate your Favicons and HTML code开始生成
4. 下载favicon包解压到/assets文件夹下，用HTML代码替换掉 _includes/head/favicon.html 文件中的代码

## 相关链接

- [TeXt Theme官方中文文档](https://tianqi.name/jekyll-TeXt-theme/docs/zh/configuration)
- [jekyll官方中文文档](http://jekyllcn.com/docs/home/)
