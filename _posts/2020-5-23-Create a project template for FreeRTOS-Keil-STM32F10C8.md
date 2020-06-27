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

很多初学FreeRTOS的朋友在搭建自己的工程项目时一定会有很多的迷茫，所以我将在这里为大家分享如何创建自己的FreeRTOS+Keil+STM32F10C8工程模板，并且重点讲解容易出现error的地方，希望大家看过本文后都能熟练的创建工程模板。

<!--more-->

## 准备工作

想要完成本工程模板，大家首先需要下载好[FreeRTOS系统](https://freertos.org/a00104.html)以及[ST库](https://www.st.com/content/st_com/en/products/embedded-software/mcu-mpu-embedded-software/stm32-embedded-software/stm32-standard-peripheral-libraries/stsw-stm32054.html#resource),然后需要创建一个文件夹来存放如下图所示的工程的文件目录。

![gif](https://s1.ax1x.com/2020/06/23/NUGQF1.gif)

## 工程文件夹的创建

### ST库的拷贝

首先将ST库**Libraries**文件夹下的**CMSIS和STM32F10x_StdPeriph_Driver**拷贝到你创建的文件夹的**Libraries**文件夹下，然后将**STM32F10x_StdPeriph_Driver文件夹下的src和inc**这两个文件夹下的**stm32f10x_it.c和stm32f10x_it.h**拷贝到你创建的文件夹下的**User**文件夹下，最后将ST库**Libraries\CMSIS\CM3\DeviceSupport\ST\STM32F10x**目录下的**stm32f10x_conf.h**拷贝到你创建的文件夹下的**User**文件夹下。

|---
| ST库->自建工程文件夹 | 文件
|-|:-
| Libraries->Libraries | CMSIS,STM32F10x_StdPeriph_Driver
| STM32F10x_StdPeriph_Driver\src->User | stm32f10x_it.c
| STM32F10x_StdPeriph_Driver\inc->User | stm32f10x_it.h
| Libraries\CMSIS\CM3\DeviceSupport\ST\STM32F10x->User | stm32f10x_conf.h
|---

### FreeRTOS的拷贝

首先将你下载下来的FreeRTOS文件夹下的**FreeRTOS\Source**文件夹下的**所有文件**拷贝你创建的**FreeRTOS**文件夹下，然后将其中**portable**文件夹下的除了**RVDS和MemMang文件夹之外全部删除**，最后将你下载下来的FreeRTOS文件夹下的**FreeRTOS\Demo\CORTEX_STM32F103_Keil**文件夹下的**FreeRTOSConfig.h**拷贝到你创建的文件夹下的**User**文件夹下。

|---
| FreeRTOS系统->自建工程文件夹 | 文件
|-|:-
| FreeRTOS\Source->FreeRTOS | All File
| FreeRTOS\Demo\CORTEX_STM32F103_Keil->User | FreeRTOSConfig.h
| 删除FreeRTOS\Source\portable | 除RVDS和MemMang文件夹之外所有
|---

## Keil的配置

![Image](https://s1.ax1x.com/2020/06/27/N6MYpd.png)

接下来进入Keil在你创建的文件夹下的Project文件夹下创建工程硬件选择STM32F10C8，然后在工程下如上图所示分别创建文件夹并添加文件，其中**startup_stm32f10x_ld_vl.s**位于**Libraries\CMSIS\startup**文件夹下，**heap_x.c**在**FreeRTOS\portable\MemMang**文件夹下，port.c在FreeRTOS\portable\RVDS\ARM_CM3。  
最后需要配置魔术棒选项卡,首先修改**Target**选项卡下的Xtal(MHz)为12.0,然后给**Output**选项卡下的Create HEX File打上勾，然后修改**C/C++**选项卡下的Optimization为Level 3，然后在Include Paths选项卡中添加5条：
1. ..\Libraries\CMSIS
2. ..\Libraries\FMLIB\inc
3. ..\User
4. ..\FreeRTOS\include
5. ..\FreeRTOS\portable\RVDS\ARM_CM3

## 修改stm32f10x_it.c

先注释掉PendSV_Handler()与SVC_Handler()，然后如下所示修改SysTick_Handler()

{% highlight C linenos %}
void SysTick_Handler(void)
{    
    #if (INCLUDE_xTaskGetSchedulerState  == 1 )
      if (xTaskGetSchedulerState() != taskSCHEDULER_NOT_STARTED)
     {
    #endif  /* INCLUDE_xTaskGetSchedulerState */  
        xPortSysTickHandler();
    #if (INCLUDE_xTaskGetSchedulerState  == 1 )
     }
   #endif  /* INCLUDE_xTaskGetSchedulerState */
}
{% endhighlight %}

## 模板提供

[模板](https://github.com/HongjieTan/STM32F103C8-FreeRTOS-Keil_Prj_tem)。  

不过大家都看到这里了为何不尝试自己创建一个呢QWQ
