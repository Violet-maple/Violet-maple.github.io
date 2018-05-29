---
title: django快速搭建网页
date: 2018-05-27 14:23:10
tags:
---

# 使用Django快速搭建网页

- `Django`是一个开放源代码的Web应用框架，由Python写成
- 它采用了MTV的框架模式，即模型M、模板T和视图V。
- 它最初是被开发来用于管理劳伦斯出版集团旗下的一些以新闻内容为主的网站，即是CMS(内容管理系统)软件
- 它于2005年7月在BSD许可证下发布
- 这套框架是以比利时的吉普赛爵士吉他手Django Reinhardt来命名的                 **详见**: [Django教程](https://docs.djangoproject.com/zh-hans/2.0)

<!--more-->

## 1. 使用pycharm创建Django

### 1-1 打开pycharm选择创建Django项目, 名字为car

​	   		  ![Django-01](https://github.com/Violet-maple/Violet-maple.github.io/blob/hexo/source/_posts/img/Django-01.png?raw=true)

- 在左下角点击Terminal打开命令窗口输入下面命令启动服务

  ```python
  python manage.py runserver
  ```

  ![Django-02](https://github.com/Violet-maple/Violet-maple.github.io/blob/hexo/source/_posts/img/Django-02.png?raw=true)

  - 点击上图地址处, 网页出现下图样式, 则开启成功

  ![Django-03](https://github.com/Violet-maple/Violet-maple.github.io/blob/hexo/source/_posts/img/Django-03.png?raw=true)

- 没有成功就应该安装Django, 再重新启动服务

  ```python
  pip install django   # 默认最新版本 或者 pip install django==1.11  指定为1.11版本
  ```

- 在Terminal命令窗口下Ctrl + C 关闭服务

- 在Terminal命令窗口输入下面命令,安装pymysql

```
pip install pymysql
```

- 安装成功后, 点击car项目下的init 导入其中:

  ![Django-04](https://github.com/Violet-maple/Violet-maple.github.io/blob/hexo/source/_posts/img/Django-04.png?raw=true)

### 1-2 创建应用

- 创建一个search的app应用  命令如下:

  ```
  python manage.py startapp search
  ```

- 修改配置文件settings

  ```python
  LANGUAGE_CODE = 'zh-hans'      # 语言设置为中文
  TIME_ZONE = 'Asia/Chongqing'   # 设置时区
  # 添加app应用
  INSTALLED_APPS = [
      'django.contrib.admin',
      'django.contrib.auth',
      'django.contrib.contenttypes',
      'django.contrib.sessions',
      'django.contrib.messages',
      'django.contrib.staticfiles',
      'search',
  ]
  ```

### 1-3 建立模型

- 打开app应用search中的models.py , 输入类carrecord:

  ![django05](https://github.com/Violet-maple/Violet-maple.github.io/blob/hexo/source/_posts/img/Django-05.png?raw=true)

## 2. 建立数据库

### 2-1打开数据库mysql 并加入一些车辆数据

- 使用win+R 打开windows 命令窗口  **前提**: 默认安装好了mysql数据库

  ```
  net start mysql
  ```

- 修改配置文件settings 

  ```
  DATABASES = {
      'default': {
          'ENGINE': 'django.db.backends.mysql',
          'NAME': 'car',
          'HOST':'LOCALHOST',
          'PORT':3306,
          'USER':'root',
          'PASSWORD':'123456'
      }
  }
  ```

- 先进行生成迁移, 在Temimal窗口命令下输入该命令:

  ```
  python manage.py makemigrations 
  ```

- 再进行迁移到mysql表里, 命令如下:

  ```
    python manage.py migrate
  ```

- 打开数据库随意加入数据供后面使用:

  ![django06](https://github.com/Violet-maple/Violet-maple.github.io/blob/hexo/source/_posts/img/Django-06.png?raw=true)

## 3. 建立视图和模板

### 3-1建立视图

- 打开views.py

  ![django07](https://github.com/Violet-maple/Violet-maple.github.io/blob/hexo/source/_posts/img/Django-07.png?raw=true)

  ```python
  # 通过render渲染页面后先用set_cookie方法设置cookie后再返回HttpResponse对象
  # 第一个参数是cookie的名字 第二个参数是cookie的值 第三个参数是过期时间(秒)
  response.set_cookie('last_visit_time', current_time, max_age=1209600)
  ```

  ```pytho
  # 第一个参数是要转换成JSON格式(序列化)的对象
  # encoder参数要指定完成自定义对象序列化的编码器(JSONEncoder的子类型)
  # safe参数的值如果为True那么传入的第一个参数只能是字典
  # return HttpResponse(json.dumps(record_list),content_type='application/json;charset=utf-8')
  ```

- 打开url 进行设置, 访问网页是的地址:

  ```python
  from search import views

  urlpatterns = [
      path('search/', views.ajax_search),
      path('admin/', admin.site.urls),
  ]
  ```

### 3-2 建立模板页Templates

![Django-08](https://github.com/Violet-maple/Violet-maple.github.io/blob/hexo/source/_posts/img/Django-08.png?raw=true)

​	**说明**: 此模板页大致这样, 可根据你自己的喜好样式设计, 此处就不作过多解释

### 3-3 建立static静态加载

- 在配置文件settings里设置

  ```python
  STATICFILES_DIRS = [os.path.join(BASE_DIR, 'static')]
  ```

## 最后

- 在开启服务, 输入:

  ```
  python manage.py runserver
  ```

- 在网页上输入http://127.0.0.1:8000/search/

- 进入后 , 输入车牌号进行查询验证:

  ![Django-09](https://github.com/Violet-maple/Violet-maple.github.io/blob/hexo/source/_posts/img/Django-09.png?raw=true)

### 以上就是大概流程, 如有好的建议, 请指正!

- 感谢阅读















