---
layout:     post
title:      在Win10下安装Anaconda环境并配置OpenCV
subtitle:   踏破铁鞋无觅处，柳暗花明又一村！
date:       2020-02-02
author:     薄论
header-img: img/post-bg-opencv.jpg
catalog: 	 true
tags:
    - OpenCV
    - Python

---

> 寒假无事，打算学一下OpenCV。于是不得不面对棘手的配置开发环境的问题。看着网上的教程，以为很简单，然而在煎熬8小时后，我发现这个坑属实大，而我更巧妙地踩到了大部分。
>
> 本篇将就我在Windows10系统下安装Anaconda环境，并配置好OpenCV的过程做下记录。



### 准备

首先，要考虑好通过哪种方式来安装OpenCV。网上普遍流传的方法主要有三种：

* 1、pip安装，在用cmd运行Python时，直接输入`pip install opencv-python`，若需要安装拓展包，输入`pip install opencv-contrib-python`即可。该方法最为简单方便，且不容易出问题，但由于不是在开发环境下安装，后期开发可能会出现功能缺失或者配置复杂。

* 2、在[OpenCV官网](https://opencv.org/)下载OpenCV.exe，安装OpenCV后，拷贝`..\opencv\build\python\cv2\python-*\cv2.pyd`文件到`..\Anaconda\Lib\site-packages`。此方法的问题在于官网很难进，下载速度较缓慢且拷贝之后可能需要对路径等相关参数进行配置，可能会造成问题。
* 3、在OpenCV包网站下载`.whl`文件，将其移至Anaconda文件夹后安装。我使用的即是本方法，并成功安装OpenCV的。

> OpenCV包网站有两个可选：
>
> 1、国外的：http://www.lfd.uci.edu/~gohlke/pythonlibs/#opencv
>
> 2、国内的（清华镜像源）：https://pypi.tuna.tsinghua.edu.cn/simple/opencv-contrib-python

不过现在还不急着安装，为了方便后面的环境配置不那么复杂，我们首先要把我们已经安装过的Python卸载掉！

```
当时在做这步操作的时候我也很慌张，但后来我查资料发现：Anaconda自带了一个Python环境，这个Python环境加入到PATH后，可能和我们早先的Python环境发生冲突，而导致Error）
```

卸载方法如下：1、打开`开始`菜单，随便选择一个程序，右键点击`卸载`，进入`卸载或更改程序`界面，找到Python后双击卸载。

![](https://img-blog.csdnimg.cn/20200202161037197.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQzMTc0MDg2,size_16,color_FFFFFF,t_70#pic_center)

2、确定是否卸载。`win+R`打开`运行`，输入`cmd`，输入`python`，若未能进入Python环境则说明卸载完成。



### 下载Anaconda并安装

进[Anaconda官网](https://www.anaconda.com/)，下载最新版本的Anaconda。（下载速度可能很慢，所以建议去下载[清华镜像](https://mirrors.tuna.tsinghua.edu.cn/anaconda/archive/)）

![](https://img-blog.csdnimg.cn/20200202161212197.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQzMTc0MDg2,size_16,color_FFFFFF,t_70#pic_center)

下载完成后，以管理员身份运行下好的`Anaconda3.exe`安装。按部就班的安装就好，唯独需要注意的一点是：在`Advanced Options`这一步时，不要勾选第一个选项（特别是当你电脑里还安装有PyCharm时）。

![](https://img-blog.csdnimg.cn/20200202161307640.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQzMTc0MDg2,size_16,color_FFFFFF,t_70#pic_center)

在安装完成后，打开`环境变量`配置界面，更改`Path`，将`..\Anaconda3`、`..\Anaconda3\Scripts`、`..\Anaconda3\Library\bin`三个路径加入到Path中。但打开`cmd`输入`Python`后，仍然无法打开，这时候我们还需要将`..\Anaconda3\pkgs\python-x`（这个取决于你安装的Anaconda版本）也加入到Path中。注意：`Anaconda3`是代指，以上路径均应该是你的Anaconda安装路径。此外，还应该将`..\Anaconda3\pkgs\python-x`置于Path的顶部。

![](https://img-blog.csdnimg.cn/20200202161335252.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQzMTc0MDg2,size_16,color_FFFFFF,t_70#pic_center)

完成这些后，Python是能正常打开的。



### 下载OpenCV包并安装

在国内的（清华镜像源）：https://pypi.tuna.tsinghua.edu.cn/simple/opencv-contrib-python，下载所需版本的OpenCV包，注意：版本`cp`后的数字代表了适配Python的版本。如果你的Python版本是3.7.4请务必选择cp37，其它依此类推。对于我们Windows64位系统，应当选择win_amd64系列。

![](https://img-blog.csdnimg.cn/20200202161441460.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQzMTc0MDg2,size_16,color_FFFFFF,t_70#pic_center)

将安装好的.whl文件拷贝到`..\Anaconda3\Lib\site-packages`文件夹中。打开`cmd`，进入以上文件夹。输入`pip install msgpack-python`、`pip install msgpack`、`pip install x.whl`，完成OpenCV包安装，`x`指的是你的whl包名(安装前两个包是为了防止插件缺少导致安装失败，若安装第一个包后安装第二个包失败，也没关系)。

验证：在`cmd`中打开Python，输入`import cv2`，若无报错，则说明安装成功。

![](https://img-blog.csdnimg.cn/20200202161508539.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQzMTc0MDg2,size_16,color_FFFFFF,t_70#pic_center)



### 在Jupyter中写一个OpenCV例程

下载图片：[lena.jpg](http://photocdn.sohu.com/20090729/Img265560887.jpg)。打开`Jupyter`，输入以下代码：

```python
import cv2 as cv

filename = "e:/lena.jpg"	#下载图片的路径
img = cv.imread(filename)	#读取图像
cv.imshow("Ohhhhhh", img)	#显示图像，并命名显示框为"Ohhhhhh"
cv.waitKey(0)				#图像界面保持（不关闭）
cv.destroyAllwindows()		#关闭图像界面

```

代码运行结果如下图所示：

![](https://img-blog.csdnimg.cn/20200202161553957.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQzMTc0MDg2,size_16,color_FFFFFF,t_70)

此外，我将相关文件做成了懒人包，有需要的可以点击链接直接进行下载：https://download.csdn.net/download/qq_43174086/12131049



### 后记

经过了将近八个小时的煎熬，终于把OpenCV的环境配好了……
中间经历了3次重装Anaconda、6次重装Python和次数不记的重装OpenCV.whl……
把能踩的坑基本踩了个遍，但看到测试代码让图像显示出来的时候，感觉折腾这么久也值了！

![](https://img-blog.csdnimg.cn/20200202161608887.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQzMTc0MDg2,size_16,color_FFFFFF,t_70)

**日后会更新OpenCV的学习笔记，毕竟配了这么久的环境，不用就浪费啦！**

