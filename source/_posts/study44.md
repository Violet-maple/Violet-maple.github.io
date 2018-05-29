---
title: study44
date: 2018-05-28 23:51:27
tags:
---

### 1. 创建虚拟环境

- 创建独立的干净的虚拟环境

<!--more-->

- 进入命令行模式输入:

```python
pip instatll virtualenv
virtualenv --no-site-packages djangoenv
```

- 查看pip安装过的包

```
pip freeze
```

- 查看所有安装过的包

```
pip list
```

- 先装一下库：

```python
pip install django==1.11   # 指定1.11版本  默认是最新版
pip install pymysql        # 装python模块 pymysql
```

### 2. 创建项目并且运行

```
django-amdin startproject day1
python manage.py startapp app
python manage.py runserver
```

### 3. 配置pymysql

```
import pymysql
pymysql.install_as_MySQLdb()
```

### 4. url反向解析

- html页面的解析

```html
src = '/app/left/'                    # 绝对路径
src = "{% url 'namespace:name' %}"    # 动态路径  方便更改
```

### 5. 静态解析

- 方法一:

```html
<img src='/statc/img/xxx.css'>
```

- 方法二:

```html
{% load static %}   # 要先加载一下
<img src='{% static "img/xxx.css" %}'>
```

### 6. 过滤器 '|'

- lower 
- upper
- date:y-m-d h:m:s
- add:1 
- add:-1

### 7. get和filter区别

- get一定要确定能获取到唯一一个对象
- filter：能获取很多对象，queryset
- first():获取第一个
- [:1]
- last()：获取最后一个

### 8. 分页

- paginator对象
- page_range：获取当前一共有多少页 range(1,3)
- page对象：
- 通过page获取paginator对象：page.paginator
- has_next:是否有上一页
- has_previous：是否又下一页
- next_page_number：上一页的页码
- previous_page_number：下一个的页码





