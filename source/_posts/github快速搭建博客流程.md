---
title: github快速搭建博客流程
date: 2018-05-26 15:18:26
tags:
---

# 使用hexo和github搭建个人博客

- 环境 - windows系统

## 什么是Hexo/Git/Node.js ?

- Hexo 是一个快速、简洁且高效的博客框架。Hexo 使用 [Markdown](http://daringfireball.net/projects/markdown/)（或其他渲染引擎）解析文章，在几秒内，即可利用靓丽的主题生成静态网页
- Git是目前世界上最先进的分布式版本控制系统（没有之一）
  - 特点是高端大气上档次！
- Node.js平台是在后端运行JavaScript代码

<!--more-->

## 1.安装

### 1-1安装git

- Windows用户建议使用这个下载地址:  [下载地址](https://git-scm.com/download/win),  找到合适的版本进行下载

- 安装时, 默认选项安装即可

- 安装完成后，还需要最后一步设置，在命令行输入：

  ```
  $ git config --global user.name "Your Name"
  $ git config --global user.email "email@example.com"
  ```

- 因为Git是分布式版本控制系统，所以，每个机器都必须自报家门：你的名字和Email地址。你也许会担心，如果有人故意冒充别人怎么办？这个不必担心，首先我们相信大家都是善良无知的群众，其次，真的有冒充的也是有办法可查的。

- 注意`git config`命令的`--global`参数，用了这个参数，表示你这台机器上所有的Git仓库都会使用这个配置，当然也可以对某个仓库指定不同的用户名和Email地址

### 1-2安装Node.js

- Windows用户建议使用这个下载地址:  [下载地址](https://nodejs.org/en/),  建议下载大多数人用的版本
- 安装时务必选择全部组件，包括勾选`Add to Path`

### 1-3安装Hexo

- 检查是否安装好以上两个程序, 在桌面点击鼠标右键, 选择

  ​		![01](https://github.com/Violet-maple/Violet-maple.github.io/blob/hexo/source/_posts/img/01.jpg?raw=true)

  在打开的类似Linux的命令行中输入命令:

  ```
  $ git --version
  $ note --version
  ```

  - 在以上的两个必备的应用程序安装完成之后
  - 我们再安装Hexo.

- 使用如下命令安装:

  ```
  $ npm install -g hexo-cli
  ```

## 2.建站

### 2-1初始化

- 安装 Hexo 成功后，执行下列命令，Hexo 将会在指定文件夹中新建所需要的文件

  ```
  $ hexo init <folder>  # folder 是文件夹名 
  $ cd <folder>         # 进入新建号的文件夹中
  $ npm install
  ```

  - 如果没有出现error, 则成功建立了站

### 2-2安装server

- 执行以下命令:

  ```
  $ npm install hexo-server --save
  ```

  - 启动server, 试一试看成功没:

  ```
  $ hexo server
  ```

  ​		![02](https://github.com/Violet-maple/Violet-maple.github.io/blob/hexo/source/_posts/img/02.jpg?raw=true)

  我们把这个网址复制下来并粘贴到浏览器中打开即可看到 Hexo 默认的主题博客了，

  如果没有成功的话建议查看自己是否有操作不当的地方或者命令错误

- 回到命令窗口Ctrl + c 关闭server

## 3.部署

### 3-1建立Git仓库

- 因为要使用到 Git ， 所以我们需要先建立一个新的仓库，(默认都申请了一个GitHub账号了)

  ​	![03](https://github.com/Violet-maple/Violet-maple.github.io/blob/hexo/source/_posts/img/03.jpg?raw=true)

​	注意: 这里在建仓库的时候, 仓库的名字一定要按照 <用户名>.github.io 这样的格式来写:

​	例:

​		![04](https://github.com/Violet-maple/Violet-maple.github.io/blob/hexo/source/_posts/img/04.jpg?raw=true)

### 3-2 生成SSH keys

- 添加 ssh keys , 使用如下命令操作生成SSH key:

  ```
  $ ssh-keygen -t rsa -C "user@mail.com"   # mail 最好是建立GitHub有效的邮箱
  ```

  输入此命令时候, 会出现设置密码: 直接Enter 3次 , 即设置密码为空

- 最后在用户目录下(administrator)得到两个文件:id_rsa.pub 和 id_rsa

- 生成密钥完成后， 我们将 id_rsa.pub 中的所有内容拷贝到github中相应的位置中去

- 找到 github 中的设置 :

  ​		![05](https://github.com/Violet-maple/Violet-maple.github.io/blob/hexo/source/_posts/img/05.jpg?raw=true)

  ​

![06](https://github.com/Violet-maple/Violet-maple.github.io/blob/hexo/source/_posts/img/06.jpg?raw=true)

​	   		   						![07](https://github.com/Violet-maple/Violet-maple.github.io/blob/hexo/source/_posts/img/07.jpg?raw=true)

   

​    将我们生成的在 id_rsa.pub 中的内容 拷贝到其中

### 3-3 更该配置

- 在你的初始化文件夹下找到 _config.yml 这个文件并打开

- 这里最好使用 **Sublime**或者 **Hbuilder** 打开

- 打开后做如下修改，这里注意空格 repository 就是 github 为我们生成的一个地址:

  ​		![09](https://github.com/Violet-maple/Violet-maple.github.io/blob/hexo/source/_posts/img/09.jpg?raw=true)

- 将其地址复制到建站文件中的配置文件:  _config.yml 用sublime 等打开修改

  ​		![08](https://github.com/Violet-maple/Violet-maple.github.io/blob/hexo/source/_posts/img/08.jpg?raw=true)

### 3-4 生成静态网页并部署

- 命令如下:

  ```
  $ hexo clean    # 清理缓存
  $ npm install hexo-deployer-git --save
  $ hexo g -d
  ```

- 在你建站的文件夹里找到:

  ​		![10](https://github.com/Violet-maple/Violet-maple.github.io/blob/hexo/source/_posts/img/10.jpg?raw=true)

- 进入source/_posts/hello_world.md    此为默认的主页页面, 更改你的即可! 

- 更改了内容记得输入命令更新下: 

  ```
  $ hexo d -g
  ```

- 其他配置后续跟进...

## 最后

- 最后在浏览器中输入我们的地址即可 即 username.github.io 其中username为你自己创建仓库的名字
- 当然在以上操作中如果遇到什么错误 这里也许可以帮到你       [Hexo帮助](https://hexo.bootcss.com/docs/troubleshooting.html)

















