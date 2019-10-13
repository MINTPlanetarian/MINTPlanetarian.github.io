---
layout:     post
title:      Lammps学习笔记-初始化
subtitle:   Define atom
date:       2019-10-12
author:     薄论
header-img: img/post-bg-ios9-web.jpg
catalog: 	 true
tags:
    - LAMMPS
---

> 英文文档参考[LAMMPS_Documation](https://lammps.sandia.gov/doc/Manual.html)；
中文文档参考[LAMMPS常用命令中文翻译索引](http://www.52souji.net/lammps-command-index.html)

### 前言
最近在英国属实慌的一批，回国就要开始着手搞课设，但是自己连Lammps的基本命令都没有摸清。
在网上搜索了英文版和中文版的资料，现将找到的些不错的资料记录于此。
感谢各位大佬的知识分享，可以说有了他们计算材料学才会在今天发展的如此迅速。

### 命令
#### atom_style style args
`atom_style`命令用来定义模拟过程中原子的类型，它会决定原子包括哪些属性。
常见的style命令有：`angle`、`atomic`、`body`、`bond`、`charge`

|  命令  |         命令指定参数         |        命令操作对象        |
| :----: | :--------------------------: | :------------------------: |
| angle  |          键能和键角          |      刚性的弹性聚合物      |
| atomic |     只需要一些默认的参数     | 液体（偏胶体）、固体、金属 |
|  body  | 质量、惯性矩、四元数、角动量 |        几乎所有物质        |
|  bond  |           键的参数           |         弹性聚合物         |
| charge |           电学参数           |      有电场的原子体系      |

显然我们最常用的是`atomic`，按照使用举例，我们并不需要定义什么参数，参见下图：

![atom_style](https://s2.ax1x.com/2019/10/13/uj0e6H.png)

#### boundary x y z

`boundary`命令用来设置模拟盒子的边界条件。

`x`,`y`,`z`可取`p`/`s`/`f`/`m`中的一个字母或两个字母的组合。

- `p`：周期性边界条件periodic
- `f`：非周期性固定边界条件fixed
- `s`：非周期性包覆边界条件shrink-wrapped
- `m`：非周期性包覆最小值边界条件minimum value

显然，我们常用`boundary p p p` 来表示x，y，z三方向都具有周期性。

`p`指的是粒子闯过盒子边界还在盒子里；

`f`指的是粒子穿过盒子边界就会消失；

`s`指的是盒子的界面会随着粒子的移动而移动，而使粒子一直在盒子里；

`m`类似于`s`，但是多了一个盒子的边界最小值

#### dimension N

`dimension`命令用来定义模拟的维度。

`N`可取`2`或`3`，表示模拟系统的维度为2或者3。

#### units style

`units`命令用来定义模拟过程中使用的单位类型，它决定了所有输入脚本、数据文件和所有输出到屏幕、日志文件以及dump文件中物理量的单位。

`style`可取：`lj`/`real`/`metal`/`si`/`cgs`/`electron`。

**1、`lj`类型**

`lj`类型没有单位，Lammps将基本量全部看做为1，而其他各物理量则与这些基本量有倍数关系，其值表示的其实就是（对基本量的）倍数。

基本量包括：`mass`、`sigma`、`epsilon`、`Boltzmann constant(k=R/NA)`

除`Boltzmann constant`以外，其他参数都与模拟原子本身有关，可以通过查找文献或拟合的方式来确定（至于怎么拟合：作势能曲线图，利用最小二乘拟合）

`lj`类型转换关系见下图：![lj](https://s2.ax1x.com/2019/10/13/uj054K.png)

**2、`real`类型**

`real`类型有单位且对参数不做定义和修改。*特别注意：距离单位为埃，能量单位为千卡。*

![real](https://s2.ax1x.com/2019/10/13/ujB9gg.png)

**3、`metal`类型**

`metal`类型多适用于金属（我猜的）。*特别注意：能量单位为电子伏特。*

![1570919797020](https://s2.ax1x.com/2019/10/13/ujBCvQ.png)

常见的应该就这三个，其他略了，参见：[units合集](http://www.52souji.net/lammps-command-units.html)

### 后记

初始化都有这么多，我人晕了。这篇大部分是来自我科段姓教授的博客（为啥我会知道这些，因为你看下图）

![1570920074677](https://s2.ax1x.com/2019/10/13/ujBVU0.png)

非常感谢他，这是[原文链接](http://www.52souji.net/lammps-command-index.html)，除了copy他的资料以外，其实我还加入了些自己对Lammps操作命令的理解，但愿我能尽快学完后面的东西吧！