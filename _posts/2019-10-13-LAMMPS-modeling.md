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

`scale`指的是晶格和模拟盒子的比例因子，其值取决于`units`的值（即初始化时选取的单位），具体关系见下表：

| units类型  |              scale表示的意义               |
| :--------: | :----------------------------------------: |
| real/metal | 立方晶胞边长为`a`，则晶格间距为`a*scale`A  |
|    cgs     | 立方晶胞边长为`a`，则晶格间距为`a*scale`cm |
|     lj     |   好像是什么LJ势的降低密度（这个我也懵）   |

`origin`表示对单胞进行平移；`orient`表示对单胞进行旋转

#### region ID style args keyword arg ...

`region`命令用于定义一个空间几何区域。

`ID`表示现在定义的空间区域的代号。

`style`与`args`关联，`style`表示该区域的几何性质，`args`表示该几何性质的相关参数。具体对应关系如下：（这里只放几个常用的）

| style值 |               args值                |                   几何意义                   |
| :-----: | :---------------------------------: | :------------------------------------------: |
| delete  |                none                 |                 删除指定区域                 |
|  block  | `xlo`,`xhi`,`ylo`,`yli`,`zlo`,`zli` |            一个有范围的四方立方体            |
|  plane  |    `px`,`py`,`pz`,`nx`,`ny`,`nz`    | 过`(px,py,pz)`点，法矢为`(nx,ny,nz)`的一个面 |

`keyword`包括`side`,`units`,`move`,`rotate`。详情见下：

| keyword |                     可取的值及其对应意义                     |
| :-----: | :----------------------------------------------------------: |
|  side   |  `in`:指定几何体内侧为区域<br />`out`:指定几何体外侧为区域   |
|  units  | `lattice`:以晶格距离作为区域距离单位<br />`box`:以模拟盒子作为区域距离单位 |
|  move   |        `vx`,`vy`,`vz`分别指区域在x,y,z方向上的位移量         |
| rotate  |                 指旋转，因不常用，具体参数略                 |

注意：`region`常被用来作为初始化集合区域，换句话说，绝大部分`create_box`,`create_atoms`,`group`命令都是在`region`定义的区域下使用的。

不适用`move`和`rotate`关键字时，`region`定义的区域是静态的，但使用之后区域就会随时间而发生变化。

#### create_atoms type style args keyword values ...

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