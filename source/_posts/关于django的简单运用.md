title: 关于django项目的建立
date: 2018-06-2 10:23:45
tags:

---

### 关于django 对http页面的处理

**前言:**

1. 本教程中使用到的python版本均为python3.x版本，由于本人安装的是python3.6.4版本，所以以下的运行环境均是在此基础上进行的

<!--more-->

2. virtualenv使用场景:当开发成员负责多个项目的时候，每个项目安装的库又是有很多差距的时候，会使用虚拟环境将每个项目的环境给隔离开来。

     比如，在有一个老项目已经开发维护了3年了，里面很多库都是比较老的版本了。例如python使用的是2.7版本的。但是新项目使用的python版本是3.6的。为了解决这种项目执行环境的冲突，所以引入了虚拟环境virtualenv。当然除了virtualenv可以起到隔离环境的作用，还有其他技术方案来实现，而且上线流程简单，大大减轻运维人员的出错率，比如每一个项目使用一个docker镜像，在镜像中去安装项目所需的环境，库版本等等

- 新建项目学生管理系统
  - windows环境

### 1. 建立独立虚拟环境

<!--more-->

***注意***: 特此默认已经有python/pip等软件 没有就移到  [配置python环境](https://violet-maple.github.io/2018/05/02/python环境的配置/#more)

#### 1-1安装virtualenv

- 进入命令模式中输入:


- 进入命令模式, 进入到目录d:中:

```
pip instatll virtualenv  
```

#### 1-1 创建虚拟环境

- 先查看一下安装虚拟环境有那些参数，是必须填写的

  ![virtualenv_help](img\virtualenv_help.png)

**注意**两个参数：
--no-site-packages和-p参数 

  进入指定盘中输入命令:

```
virtualenv --no-site-packages venv    # 建一个独立干净环境
```

*注释*:-p参数是指定python版本 如果计算机有python2.x和python3.x就要指定版本后面跟版本..

​	以下是指定安装虚拟环境中的python版本的安装方式：

​	![virtualenv_env_p](img\virtualenv_env_p.png)

#### 1.3 进入/退出venv

```python
cd venv/Scripts/文件夹  使用activate命令   # 进入
deactivate                                # 退出
```

- 查看pip安装过包的版本

```
pip freeze
```

- 查看pip安装过的包

```
pip list
```

### 2. 安装包

- 大致有这些:

```python
pip install django==1.11                 # 指定django版本1.11
pip install pyMySQL                      # pymysql是python第3方库
pip install djangorestframework==3.4.6   # 指定为3.4.6版本(才能用下面django_filter中的filters模块)
pip install pillow
pip install django_filter
...
```

- 上述安装可以直接把包/库名写入一个 xxx.text文件中:

```python
pip isntall -r xxx.txt   # 这样就全部一次性安装好了(-r 递归式的安装)
```

### 3. 创建一个Django项目

```python
django-admin startproject day010    # 项目名为day010
```

#### 3-1 项目创建好了, day010项目文件夹里面有:

- day010:
  - __init__.py              # 指明该目录结构是一个python包，暂无内容，在后期会初始化一些工具会使用到
  - setting.py        # Django项目的配置文件，其中定义了本项目的引用组件，项目名，数据库，静态资源，调试模式，域名限制等
  - urls.py             # 项目的URL路由映射，实现客户端请求url由哪个模块进行响应
  - wsgi.py            # 定义WSGI接口信息，通常本文件生成后无需改动
- manage.py： 是Django用于管理本项目的管理集工具，之后站点运行，数据库自动生成，数据表的修改等都是通过该文件完成。

#### 3-2 运行Django项目

- Terminal中输入:

  ```
  python manage.py runserver
  ```

#### 3-3 创建app

```python
python manage.py startapp app   # 此处app只是一个app名
```

<br>

此处可参照     [django快速搭建网页](https://violet-maple.github.io/2018/05/27/django快速搭建网页/#more)

