---
layout: article
title: 搭建FreeRTOS+Keil+STM32F10C8工程模板
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
key: 2020-5-23-13
tags: FreeRTOS
category: blog
date: 2020-5-23 11:40:00 +08:00
mermaid: true
sharing: true
aside:
  toc: true
show_author_profile: true
---

今天我在这里为大家分享如何创建自己的FreeRTOS+Keil+STM32F10C8工程模板，并且重点讲解容易出现error的地方，希望大家看过本文后都能熟练的创建工程模板。

<!--more-->

## 准备工作

想要完成本工程模板，大家首先需要下载好[FreeRTOS系统](https://freertos.org/a00104.html)以及[ST库](https://www.st.com/content/st_com/en/products/embedded-software/mcu-mpu-embedded-software/stm32-embedded-software/stm32-standard-peripheral-libraries/stsw-stm32054.html#resource),然后需要创建一个文件夹来存放如下图所示的工程的文件目录。

<iframe height=500 width=500 src="https://imgchr.com/i/NNLQC4">

## ST库的拷贝

首先将ST库Libraries文件夹下的CMSIS和STM32F10x_StdPeriph_Driver拷贝到你创建的文件夹的Libraries文件夹下，然后将STM32F10x_StdPeriph_Driver文件夹下的crc和inc这两个文件夹下的stm32f10x_it.c和stm32f10x_it.h拷贝到你创建的文件夹下的User文件夹下，最后将ST库\Libraries\CMSIS\CM3\DeviceSupport\ST\STM32F10x目录下的stm32f10x_conf.h拷贝到你创建的文件夹下的User文件夹下。

## FreeRTOS的拷贝

首先将你下载下来的FreeRTOS文件夹下的FreeRTOS\Source文件夹下的所有文件拷贝你创建的FreeRTOS文件夹下，然后将其中portable文件夹下的除了RVDS和MemMang文件夹之外全部删除，最后将你下载下来的FreeRTOS文件夹下的FreeRTOS\Demo\CORTEX_STM32F103_Keil文件夹下的FreeRTOSConfig.h拷贝到你创建的文件夹下的User文件夹下。

## Keil的配置

![Image](https://s1.ax1x.com/2020/06/23/NUSQwn.png)

接下来进入Keil在你创建的文件夹下的Project文件夹下创建工程硬件选择STM32F10C8，然后在工程下如上图所示分别创建文件夹并添加文件，其中startup_stm32f10x_ld_vl.s位于Libraries\CMSIS\startup文件夹下，heap_x.c在FreeRTOS\portable\MemMang文件夹下。  
最后需要配置魔术棒选项卡,首先修改Target选项卡下的Xtal(MHz)为12.0,然后给Output选项卡下的Create HEX File打上勾，然后修改C/C++选项卡下的Optimization为Level 3，然后在Include Paths选项卡中添加5条：
1. ..\Libraries\CMSIS
2. ..\Libraries\FMLIB\inc
3. ..\User
4. ..\FreeRTOS\include
5. ..\FreeRTOS\portable\RVDS\ARM_CM3

## Fixing Bugs

编译之后Keil会报错提示我们缺少port系列的函数，这是因为FreeRTOS的task.c需要调用到port.c中的函数却没有包含它，所以我们需要打开task.c添如下代码：

{% highlight c linenos %}
#include port.c
{% endhighlight %}{:.success}
