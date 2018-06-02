title: python环境配置
date: 2018-05-2 16:41:49
tags:

---

**前言:**

- Python是一种计算机程序设计语言。你可能已经听说过很多种流行的编程语言，比如非常难学的C语言，非常流行的Java语言，适合初学者的Basic语言，适合网页编程的JavaScript语言等等。
- 那Python是一种什么语言？
  - 首先，我们普及一下编程语言的基础知识。用任何编程语言来开发程序，都是为了让计算机干活，比如下载一个MP3，编写一个文档等等，而计算机干活的CPU只认识机器指令，所以，尽管不同的编程语言差异极大，最后都得“翻译”成CPU可以执行的机器指令。而不同的编程语言，干同一个活，编写的代码量，差距也很大。
  - 比如，完成同一个任务，C语言要写1000行代码，Java只需要写100行，而Python可能只要20行。

<!--more-->

- 所以Python是一种相当高级的语言。

  - 你也许会问，代码少还不好？代码少的代价是运行速度慢，C程序运行1秒钟，Java程序可能需要2秒，而Python程序可能就需要10秒。


  - 那是不是越低级的程序越难学，越高级的程序越简单？表面上来说，是的，但是，在非常高的抽象计算中，高级的Python程序设计也是非常难学的，所以，高级程序语言不等于简单。

- 用Python可以做什么？

  - 可以做日常任务，比如自动备份你的MP3；可以做网站，很多著名的网站包括YouTube就是Python写的；可以做网络游戏的后台，很多在线游戏的后台都是Python开发的。总之就是能干很多很多事啦。

  - Python当然也有不能干的事情，比如写操作系统，这个只能用C语言写；写手机应用，只能用Swift/Objective-C（针对iPhone）和Java（针对Android）；写3D游戏，最好用C或C++。

    ​										**注:** 本教程中安装python版本为python3.x版本

    ​										**环境**: windows

### 1. 安装Python 3.6

目前，Python有两个版本，一个是2.x版，一个是3.x版，这两个版本是不兼容的。由于3.x版越来越普及，我们的教程将以最新的Python 3.6版本为基础。电脑上安装的Python版本是最新的3.6.x

首先，根据你的Windows版本（64位还是32位）从Python的官方网站下载Python 3.6对应的[64位安装程序](https://www.python.org/ftp/python/3.6.4/python-3.6.4-amd64.exe)或[32位安装程序](https://www.python.org/ftp/python/3.6.4/python-3.6.4.exe)

 ![python-pip01](img\python-pip01.png)

**特别要注意**勾上`Add Python 3.6 to PATH`，添加到环境变量中. 然后点“Install Now”即可完成安装

### 2. 运行Python

- 安装成功后，打开命令提示符窗口，敲入python后，会出现两种情况：
- 情况一：

![python-pip02](img\python-pip02.png)

   看到上面的画面，就说明Python安装成功！

你看到提示符`>>>`就表示我们已经在Python交互式环境中了，可以输入任何Python代码，回车后会立刻得到执行结果。现在，输入`exit()`并回车，就可以退出Python交互式环境（直接关掉命令行窗口也可以）。

- 情况二：得到一个错误：

```
‘python’ 不是内部或外部命令，也不是可运行的程序或批处理文件。

```

![python-pip03](img\python-pip03.png)

这是因为Windows会根据一个`Path`的环境变量设定的路径去查找`python.exe`，如果没找到，就会报错。如果在安装时漏掉了勾选`Add Python 3.6 to PATH`，那就要手动把`python.exe`所在的路径添加到Path中

- 桌面 `我的电脑` 右键点击属性:

  ![python-pip04](img\python-pip04.png)

- 进入到环境变量:

  ![python-pip05](img\python-pip05.png)

- 在变量值里 末尾添加输入你python 的地址 

   格式为`;C:\Users\Administrator\AppData\Local\Programs\Python\Python36-32\Scripts\ `


- 如果你你还不会怎么修改环境变量，建议把Python安装程序重新运行一遍，
  - **务必记得勾上**`Add Python 3.6 to PATH`。

### 3. 最后就能顺利运行了







































