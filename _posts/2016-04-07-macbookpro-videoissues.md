---
title: 遭遇苹果的显卡门
date: 2016-04-07 10:44:42
tags:
- macbookpro
categories:
- macos
---

### 工作电脑突然坏了
前段时间，工作用的MacBookPro坏了，就在桌上放了一夜，第二天就无法进入系统。具体的现象如下：

	开机后，出现苹果的Logo，下方的进度条在三分之一处停止不动，然后会有几种情况，
	- 有一定的几率直接重启；
	- 有一定的几率直接白屏，而且风扇的声音很大；
	- 有极小极小的几率能顺利的启动。

### 天才吧检测出显卡门
驱车一小时来到就近的Apple Store，没有提前预约，又排队两个小时。经过工作人员的检测，确定电脑的故障是显卡门缺陷导致的。关于显卡门可以看这里：[适用于视频问题的 MacBook Pro 维修扩展计划](http://www.apple.com/cn/support/macbookpro-videoissues/)。

工作人员说可以换主板，但是目前店里面没有货，需要从其他门店调，可能要一周。原因找到了，但维修的建议我并未采纳，因为：

	- 出于数据安全考虑，不能把电脑留在店里面一周；
	- 我需要把电脑寄回公司，由IT解决；
	- 公司重新寄了一台电脑给我，已经在路上了。

然后开始了折腾之旅。
<!-- more -->
### 折腾之旅
公司寄的电脑很快就到了，但是一些资料，需要转移到新的电脑上。这时，我开始了折腾，虽说结果看来，有些似乎没有意义，但`生命在于折腾`，不是么？

显卡既然不能用，那么我进入纯命令行模式，那么系统就能跑起来，然后挂载个U盘，或者开启个ftp服务，那么资料就能拷贝出来了？！

#### 单用户模式
开机按住`⌘+s`，系统会进入单用户模式，其实就是一个纯命令行模式。再这个模式下，能够访问到系统盘下面的所有数据，但不巧的是，我硬盘当时单独分了一个区放了部分数据，而这个分区无论如何都挂载不上。此外，单用户模式下面，是没有网络支持的。所以，这种方式只能作罢。

#### 目标磁盘模式
开机按住`⌘+t`，系统进入目标磁盘模式。也就是两台Mac电脑连在一起，另外一台电脑可以把这台电脑当做一个大大的移动硬盘，访问上面的数据。不得不说，这个功能非常好。但是限制也是有的，就是连接线。OS X系统支持两种连接方式：一个是苹果自己的Thurdblot雷电连接线，另外一个是Firewire火线连接。这两种线我手上都没有。所以，这种方式只能作罢。

#### Linux引导启动
那么，换个系统如何？我突然想起柜子里面有几年前刻录的Ubuntu安装光盘，可以直接Live CD的方式运行Ubuntu。于是我找到了Ubuntu光盘，另外还有几张没有刻录的空白DVD光盘。

##### 光盘启动Ubuntu
把[Ubuntu](http://www.ubuntu.com/desktop)的光盘放入光驱，然后`⌘+o`光盘启动，看到Ubuntu选择启动的界面，瞬间觉得希望在前面。但是很快，希望就破灭了。启动项里面都是带界面的，所以启动到一半就提示错误，应该还是显卡的问题。或许，我找个纯命令行界面的Linux发行版，能解决这个问题。

	Tips: MacBook 开机按住触摸板，可以在加载系统前，弹出光驱中的光盘。

##### USB启动TinyCore
[TinyCore](http://www.tinycorelinux.net)是一个非常小巧的发行版。Core版本只有4M左右，带GUI界面的10M左右，功能全一点的100M。正好车上有个2G的USB用来播放歌曲的，直接拿来做启动盘。从网上下载好了TinyCore的iso虚拟盘，并把U盘制作成了启动盘。
``` bash
$ dd of=/dev/disk4 if=tinycore.iso
```
然后，启动盘制作好了后，坏掉的Macbook竟然无法从U盘启动。开机`⌘+o`并不会出现U盘，也许是USB接口供电不够，无论如何这条路不行。所以只好还原了U盘。
``` bash
$ dd of=/dev/disk4 if=/dev/zero
```
既然还有空白的光盘，不如刻录一张，继续从光盘启动？

##### 光盘启动TinyCorePlus
这次，我下载的TinyCorePlus版本，并通过Virtual Box创建了个虚拟机，测试里面同样包含纯命令行的启动方式。接下来要做的就是刻录光盘了。网上的教程上都说，通过系统自带的磁盘工具可以直接刻录光盘，但事实上，我看到的不太一样，也许是因为我的系统是目前最新的 `OS X 10.11 EI Capitan`。花了一点时间，找到个免费的好工具[Burn](http://burn-osx.sourceforge.net/Pages/English/home.html), 最终完成了光盘启动盘的制作。

放入光盘，顺序启动，甚至带GUI的选项也能启动，可能是没用到显卡吧。然后才意识到一个很严重的问题，根本无法访问OS X系统下面的数据。因为OS X系统分区和另外一个数据分区，都默认使用了HFS+文件系统，而TinyCore是没有提供支持的。所以，挂载不上这两个分区，也就不能访问里面的数据。好吧，折腾了这么久，原来都是`瞎折腾-_-!`。

### 最简单粗暴的解决办法
万万没想到，我最后还是把数据拷贝出来了。我手上有两台电脑，一台无法进入系统，一台正常，而他们都是MacbookPro。于是，我干嘛不直接换硬盘呢？是的，我真这么干了。

MacbookPro后盖上一共10颗螺丝，其中3颗比较长，拆下的时候我习惯按顺序摆放。后盖卸下来后，硬盘位于左下角，硬盘上方有一个挡条，上面两颗螺丝卸下来后，硬盘就能直接取下。然后把硬盘换入正常的电脑，就可以启动，并进入系统备份资料，删除资料，后面删除整个用户清除个人数据。最后把硬盘还原回去，接着就可以把电脑寄回去了。

### 后记
换个情景，也许开始在`天才吧`就能处理好，也就没有后续这么多事情。后面的过程真的很折腾，似乎也没什么用，但是这种折腾让我找到久违的痛快。

#### Mac系统下显示dd的进度
通过brew安装pv，然后切换到root
``` bash
$ pv -cN source < /dev/zero | dd of=/dev/disk3 bs=4m
```
