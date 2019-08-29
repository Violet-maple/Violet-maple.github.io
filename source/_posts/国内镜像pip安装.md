title: 镜像使用
date: 2019-06-21 10:23:45
tags:

------



- ## 国内源：

清华：https://pypi.tuna.tsinghua.edu.cn/simple

阿里云：http://mirrors.aliyun.com/pypi/simple/

中国科技大学 https://pypi.mirrors.ustc.edu.cn/simple/

华中理工大学：http://pypi.hustunique.com/

山东理工大学：http://pypi.sdutlinux.org/ 

豆瓣：http://pypi.douban.com/simple/

*note：使用时下载失败，要求使用**https**源，要注意。*

<!--more-->

- ## 临时使用：

可以在使用pip的时候加参数-i https://pypi.tuna.tsinghua.edu.cn/simple
例如：pip install -i https://pypi.tuna.tsinghua.edu.cn/simple pyspider，这样就会从清华这边的镜像去安装pyspider库。

- ## 永久修改，一劳永逸：

[Linux](http://lib.csdn.net/base/linux)下，修改 ~/.pip/pip.conf (没有就创建一个文件夹及文件。文件夹要加“.”，表示是隐藏文件夹)

内容如下：

```powershell
[global]
index-url = https://pypi.tuna.tsinghua.edu.cn/simple
[install]
trusted-host=mirrors.aliyun.com
```

windows下，直接在user目录中创建一个pip目录，再新建文件pip.ini。（例如：C:\Users\admin\pip\pip.ini）内容同上。

引用：[让PIP源使用国内镜像，提升下载速度和安装成功率- microman - 博客园](http://www.cnblogs.com/microman/p/6107879.html)

