---
layout:     post
title:      Lammps学习笔记-建模
subtitle:   Modeling
date:       2019-10-13
author:     薄论
header-img: img/post-bg-ios9-web.jpg
catalog: 	 true
tags:
    - LAMMPS
---

> 英文文档参考[LAMMPS_Documation](https://lammps.sandia.gov/doc/Manual.html)；
中文文档参考[LAMMPS常用命令中文翻译索引](http://www.52souji.net/lammps-command-index.html)

### 前言
昨天搞初始化，今天搞建模，说起来Lammps只会越学越难。

### 命令

#### lattice style scale keyword values ...

`lattice`命令用来定义晶格。

`style`表示晶格类型，其详细参数信息如下：

| style的值 |       晶格类型       |                             要点                             |
| :-------: | :------------------: | :----------------------------------------------------------: |
|   none    | 不定义原胞和相关基组 |              `create_atoms`不能使用它定义的晶格              |
|    sc     |       简单立方       |                      三维，1个基本原子                       |
|    bcc    |       体心立方       |                      三维，2个基本原子                       |
|    fcc    |       面心立方       |                      三维，4个基本原子                       |
|    hcp    |       密排六方       |                      三维，4个基本原子                       |
|  diamond  |      金刚石结构      |                      三维，8个基本原子                       |
|    sq     |      正方形单胞      |                   二维，边向量`a1 = 1 0 0`                   |
|    sq2    |      正方形单胞      |                   二维，边相量`a2 = 0 1 0`                   |
|    hex    |      长方形单胞      |         二维，边相量`a1 = 1 0 0`,`a2 = 0 sqrt(3) 0`          |
|  custom   |    矢量自定义晶胞    | 三维，用户可自定义边矢量和单胞中一系列基本原子的坐标来定义晶格 |



#### create_atoms type style args keyword values …
`create_atoms`命令用来在晶格阵点上创建原子，或创建一个单独的原子，或创建一些列随机原子。

`type`表示创建的原子类型序号，只是标识符，无实际意义。

`style`表示被创建原子的属性，与之后的`args`有关联，详情见下表：

| style类型 |     args的值     |                      args的意义                      |
| :-------: | :--------------: | :--------------------------------------------------: |
|    box    |       none       |                         none                         |
|  region   |    region-ID     |               `region`内原子才会被创建               |
|  single   |      x,y,z       |          要创建原子的坐标（基矢为原胞基矢）          |
|  random   | N,seed,region-ID | 在`region-ID`的盒子内按照`seed`种子创建`N`个随机原子 |

`keyword`表示参数设定关键字，`keywords`值与`values`值相关，详情见下表：

| keyword值 |   values的值   |                         values的含义                         |
| :-------: | :------------: | :----------------------------------------------------------: |
|   basis   |    M itype     |               指定特定的`M`原子的类型为`itype`               |
|   remap   |   yes or no    | `yes`代表周期性边界条件起作用；`no`代表所创建原子在盒子外会消失（仅在`singe`下使用） |
|   units   | lattice or box |   用于确定单位是`lattice`（晶格距离）还是`box`（模拟盒子）   |

另外，`create_atoms`可以向以及存在的体系中继续添加原子，即该命令可以重复使用，从而在模拟盒子内创建多组原子。

### 后记

初始化都有这么多，我人晕了。这篇大部分是来自我科段姓教授的博客（为啥我会知道这些，因为你看下图）

![1570920074677](https://s2.ax1x.com/2019/10/13/ujBVU0.png)

非常感谢他，这是[原文链接](http://www.52souji.net/lammps-command-index.html)，除了copy他的资料以外，其实我还加入了些自己对Lammps操作命令的理解，但愿我能尽快学完后面的东西吧！