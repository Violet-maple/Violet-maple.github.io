titlle: 使用centos部署django项目

data: 2018-06-10

---

#### nginx + uWSGI + django的处理流程

- 首先nginx 是对外的服务接口，外部浏览器通过url访问nginx,
- nginx 接收到浏览器发送过来的http请求，将包进行解析，分析url，如果是静态文件请求就直接访问用户给nginx配置的静态文件目录，直接返回用户请求的静态文件，
- 如果不是静态文件，而是一个动态的请求，那么nginx就将请求转发给uwsgi,uwsgi 接收到请求之后将包进行处理，处理成wsgi可以接受的格式，并发给wsgi,wsgi 根据请求调用应用程序的某个文件，某个文件的某个函数，最后处理完将返回值再次交给wsgi,wsgi将返回值进行打包，打包成uwsgi能够接收的格式，uwsgi接收wsgi发送的请求，并转发给nginx,nginx最终将返回值返回给浏览器。

<!--more-->

- 要知道第一级的nginx并不是必须的，uwsgi完全可以完成整个的和浏览器交互的流程，但是要考虑到某些情况
  - 安全问题，程序不能直接被浏览器访问到，而是通过nginx,nginx只开放某个接口，uwsgi本身是内网接口，这样运维人员在nginx上加上安全性的限制，可以达到保护程序的作用。
  - 负载均衡问题，一个uwsgi很可能不够用，即使开了多个work也是不行，毕竟一台机器的cpu和内存都是有限的，有了nginx做代理，一个nginx可以代理多台uwsgi完成uwsgi的负载均衡。
  - 静态文件问题，用django或是uwsgi这种东西来负责静态文件的处理是很浪费的行为，而且他们本身对文件的处理也不如nginx好，所以整个静态文件的处理都直接由nginx完成，静态文件的访问完全不去经过uwsgi以及其后面的东西。

uWSGI是一个Web服务器，它实现了WSGI协议、uwsgi、http等协议。Nginx中HttpUwsgiModule的作用是与uWSGI服务器进行交换。

### 1. 安装MariaDB

- 安装命令

```
yum -y install mariadb mariadb-server
```

- 安装完成MariaDB，首先启动MariaDB

```
systemctl start mariadb
```

- 设置开机启动

```
systemctl enable mariadb
```

### 2. 设置密码

- 命令: 

```
mysql_secure_installation
```

- Enter current password for root:<–初次运行直接回车
- 设置密码

```
Set root password? [Y/n] <– 是否设置root用户密码，输入y并回车或直接回车
```

```
New password: <– 设置root用户的密码
Re-enter new password: <– 再输入一次你设置的密码

其他配置

Remove anonymous users? [Y/n] <– 是否删除匿名用户，回车或者n回车

Disallow root login remotely? [Y/n] <–是否禁止root远程登录,回车 n回车

Remove test database and access to it? [Y/n] <– 是否删除test数据库，回车 n回车

Reload privilege tables now? [Y/n] <– 是否重新加载权限表，回车 y回车

初始化MariaDB完成，接下来测试登录

mysql -u root -p
```

### 3. 开启远程连接

在mysql数据库中的user表中可以看到默认是只能本地连接的，所有可以添加一个新的用户，该用户可以远程访问

#### 3-1. 创建用户

```
# 先使用数据库
use mysql;

# 针对ip
create user 'root'@'192.168.10.10' identified by 'password';

#全部
 create user 'root'@'%' identified by 'password';
```

#### 3-2. 授权

```
# 给用户最大权限
grant all privileges on *.* to 'root'@'%' identified by 'password';

# 给部分权限(test 数据库)
grant all privileges on test.* to 'root'@'%' identified by 'password' with grant option;
```

 ```
# 刷新权限表
flush privileges;
 ```

```
# 查看
show grants for 'root'@'localhost';
```

- 接下来就可以在远程的数据库可视化工具中直接访问该服务器中的mysql了。

```
# 访问数据库
mysql -u root -p
```

### 4. 安装python3.6

在centos中(**Linux系统**)，系统默认只提供python2.7的版本，但是项目我们使用的python3.6的版本

所以我们自己安装python3

#### 4-1. 安装Python3的方法

- 首先安装依赖包

```
yum -y groupinstall "Development tools"
```

```
yum -y install zlib-devel bzip2-devel openssl-devel ncurses-devel sqlite-devel readline-devel tk-devel gdbm-devel db4-devel libpcap-devel xz-devel
```

- 然后根据自己需求下载不同版本的Python3，我下载的是Python3.6.2

```
wget https://www.python.org/ftp/python/3.6.2/Python-3.6.2.tar.xz
```

```
然后解压压缩包，进入该目录，安装Python3
```

```
tar -xvJf  Python-3.6.2.tar.xz            # 解归档
cd Python-3.6.2                           # 进入
./configure --prefix=/usr/local/python3
make && make install
```

最后创建软链接

```
ln -s /usr/local/python3/bin/python3 /usr/bin/python3
```

```
ln -s /usr/local/python3/bin/pip3 /usr/bin/pip3
```

### 5. 安装环境

#### 5-1. 安装virtualenv

```
yum install python-virtualenv
```

#### 5-2. 创建虚拟环境

```
mkdir env      # 创建env目录 
cd env         # 进入
virtualenv --no-site-packages axfenv  # 创建axfenv的虚拟环境
```

```
cd axfenv
```

```
# 激活虚拟环境
source bin/activate
```

#### 5-3. 安装环境需要的包

```
pip3 install -r re_install.txt    # 递归式安装
```

```
其中re_install.txt文件中记录的是需要安装包的名称以及对应的版本
```

### 6. 部署

**该部署采用的是cenots7系统来部署:**

Django的项目中，在工程目录下settings.py文件中有一个DEBUG=True参数，如果DEBUG=False则会出现js,css，img无法加载的情况出现。

**原因如下：**

Django框架仅在开发模式下提供静态文件服务。当我开启DEBUG模式时，Django内置的服务器是提供静态文件的服务的，所以css等文件访问都没有问题，但是关闭DEBUG模式后，Django便不提供静态文件服务了。想一想这是符合Django的哲学的：这部分事情标准服务器都很擅长，就让服务器去做吧！

#### 6-1. 测试环境中部署方式

在测试环境中一般都直接使用python manage.py runserver的方式去运行项目。其中就涉及到DEBUG=False的修改，静态目录的修改等，具体修改如下：

```
修改settings.py配置文件中的DEBUG=False模式，修改ALLOEWD_HOST=['*']
```

```
修改工程目录下的urls.py, 先导入serve
```

```
from django.views.static import serve
```

```
urlpatterns = [
    url(r'^admin/', admin.site.urls),
    url(r'^axf/', include('axf.urls', namespace='axf')),
	# 增加以下的url路由
    url(r'^static/(?P<path>.*)$', serve, {"document_root": settings.STATICFILES_DIRS[0]}),
    url(r'^$', views.home)
]
```

#### 6-2. 正式环境中部署方式

正式环境中部署为nginx+uwsgi来部署django项目

##### 6-2-1. 安装nginx

a）添加nginx存储库

```
yum install epel-release
```

b) 安装nginx

```
yum install nginx
```

c) 运行nginx

- Nginx不会自行启动。要运行Nginx

```
systemctl start nginx
```

nginx的运行命令：

```
 systemctl status nginx 查看nginx的状态
 systemctl start/stop/enable/disable nginx 启动/关闭/设置开机启动/禁止开机启动
```

d）系统启动时启用Nginx

```
systemctl enable nginx
```

e）如果您正在运行防火墙，请运行以下命令以允许HTTP和HTTPS通信：

```
sudo firewall-cmd --permanent --zone=public --add-service=http 
```

```
sudo firewall-cmd --permanent --zone=public --add-service=https
```

```
sudo firewall-cmd --reload
```

运行结果如下:

![图](https://github.com/Violet-maple/Violet-maple.github.io/blob/hexo/source/_posts/img/django_centos_nginx.png?raw=true)

#### 6-3. 配置uwsgi

##### 6-3-1. 安装uwsgi

```
pip3 install uwsgi
```

- 然后进行环境变量的配置， 建立软连接

```
ln -s /usr/local/python3/bin/uwsgi /usr/bin/uwsgi
```

![图](https://github.com/Violet-maple/Violet-maple.github.io/blob/hexo/source/_posts/img/django_centos_uwsgi.png?raw=true)

#### 6-4. 配置项目代码，配置项目nginx，配置uwsgi.ini等

本案例的配置文件，都习惯将每一个项目的配置文件，日志文件，虚拟环境放在一起，这样开发方便，运维也方便维护

项目的目录结构如下：

![图](https://github.com/Violet-maple/Violet-maple.github.io/blob/hexo/source/_posts/img/django_centos_project_mulu.png?raw=true)

其中：

conf是配置文件，用于存放项目的nginx.conf文件，uwsgi.ini文件

logs是日志文件，用于存放nginx的启动成功和失败文件，以及uwsgi的运行日志文件

env是用于存放虚拟环境

src是项目文件，该目录下上传的是目录代码

#### 6-4-1. 配置nginx.conf文件

<b>首先</b>：编写自己项目的nginx.conf文件如下：

每一个项目对应有一个自己定义的nginx的配置文件，比如爱鲜蜂项目，我定义为axfnginx.conf文件

```python
server {
     listen       80;
     server_name 47.106.189.34 localhost;

     access_log /home/logs/access.log;
     error_log /home/logs/error.log;

     location / {
         include uwsgi_params;
         uwsgi_pass 127.0.0.1:8890;
     }
     location /static/ {
         alias /home/src/axf/static/;
         expires 30d;
     }
     loction /media/ {
       alias /home/src/axf/media/;
     }
 }
```

<b>其次</b>：修改总的nginx的配置文件，让总的nginx文件包含我们自定义的项目的axfnginx.conf文件

总的nginx配置文件在：/etc/nginx/nginx.conf中

![图](https://github.com/Violet-maple/Violet-maple.github.io/blob/hexo/source/_posts/img/django_centos_nginx_peizhi.png?raw=true)

- 以上步骤操作完成以后，需要重启nginx：

```
systemctl restart nginx
```

如果自定义的axfnginx.conf文件没有错误的话，查看nginx的运行状态会有如下的结果：

![图](img/django_centos_nginx_status.png)

#### 6-4-2. 配置uwsgi文件

- 在conf文件夹下除了包含自定义的axfnginx.conf文件，还有我们定义的uwsgi.ini文件

```python
[uwsgi]
projectname = axf   # 赋值
base = /home/src    # 赋值

master = true       # 守护进程

processes = 4       # 进程个数

pythonhome = /home/env/axfenv    # 虚拟环境

chdir = %(base)/%(projectname)   # 项目地址

pythonpath = /usr/local/python3/bin/python3    # 指定python版本

module = %(projectname).wsgi                  # 指定uwsgi文件

socket = 127.0.0.1:8890        # 和nginx通信地址:端口  (与上述本地地址一致)

logto = /home/logs/uwsgi.log   # 日志文件地址
```

- 运行项目:

```
# 在建好的/home/conf文件下运行:
uwsgi --ini uwsgi.ini
```

- 启用几次后, 端口容易被占 如下:

![图](https://github.com/Violet-maple/Violet-maple.github.io/blob/hexo/source/_posts/img/nginx-uwsgi-port.png?raw=true)

```
netstat -lntp     # 查看端口号
killall -9 uwsgi  # 结束所有的uwsgi 占用的端口号
```

- 再运行上述uwsgi  --ini uwsgi.ini 则成功了

