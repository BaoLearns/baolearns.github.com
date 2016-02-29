---
layout: post
title: Django简明教程
description: 简单的Django入门教程
category: blog
---


#Django简明教程

---

大家好,很荣幸给大家介绍Django.

Django是Python的一个重量级的Web库,能够较块地开发网站.

Django是与音乐艺人同名,并且有一次的电影也是讲Django的故事,Django的发音很憋足,中文直译是姜戈.

很多著名的网站就是用Django开发的,比如Instagram,Disqus等详细信息请参照官方文档[Sit Using Django](https://www.djangoproject.com/start/overview/).

Django采用了MVC设计模式,即Model,View,Controller.

你可以访问[Django的官方网站](https://www.djangoproject.com)获取更多的信息.

由于Django是Python语言下的一个库,所以学习Django之前你必须要了解基本的Python语法,你可以参见[廖大神的Python入门教程](https://www.liaoxuefeng.com).

Django现在最新的版本是1.9,所以本教程是使用**Python 2.7** + **Django 1.9** .如果你使用Python 3.x也没关系,Django已完美支持Python 3.x .

##安装django
---

###Linux下的安装

安装django之前首先要有python运行环境.

####pip命令安装

通过pip命令安装Django

```
pip install Django
```

这样就会自动安装最新版本的django.

####下载源码安装

去官方网站[https://www.djangoproject.com/download/](https://www.djangoproject.com/download/)下载django,下载到本地并解压.

然后输入命令:

```
cd Django-1.9
python setup.py install
```

这样就会安装好django.

####查看django是否安装成功

在python运行环境下,输入:

```
import django
print django.VERSION
```
如果能够输出django的版本号,说明您已经安装成功了,congratulation.

##我的第一个django项目

---
输入命令建立django的第一个项目mysite:

```
django-admin.py startproject mysite    # [mysite就是项目的名字]
cd mysite
```
**注意以下操作默认在外部的mysite目录键入命令!**

django给我们创建了很多文件,使用tree命令展开mysite.

```
tree
```

```
.
├── manage.py
└── mysite
    ├── __init__.py
    ├── settings.py
    ├── urls.py
    └── wsgi.py

1 directory, 5 files
```
manage.py是一个用python写的命令文件,可以用这个文件加上参数,对我们的项目进行操作.mysite文件夹包含一个__init__.py,这个是包的文件,可以不用管;setting.py文件是我们项目的配置文件,这里面可以对我们的项目进行一些配置,比如语系,数据库等;urls.py就是访问我们的web项目时的url处理文件,不同的url采用不同的处理,这也是说django是通过url来执行不同的函数,显示不同的效果,wsgi.py先暂时可以不用管.

django帮我们干了很多事情,我们可以不修改任何地方就可以在本地运行django项目.键入如下命令:

```
#这是采用默认的ip地址和端口号(127.0.0.1:8000)
python manage.py runserver
或
#自行指定ip地址和端口号
python manage.py runserver 0.0.0.0:50003
```

在浏览器中输入127.0.0.1:8000,可得到如下信息:

```
It worked!
Congratulations on your first Django-powered page.

Of course, you haven't actually done any work yet. Next, start your first app by running python manage.py startapp [app_label].

You're seeing this message because you have DEBUG = True in your Django settings file and you haven't configured any URLs. Get to work!

```

这就说明,我们自己已经运行了第一个django web程序,你可以看到,我们并没有做什么动作,所以说django为我们做了很多事.

停掉开发服务器,可在命令行输入Ctrl + C .

#Hello World

---

很多时候学习一门语言,惯例的做法是先输出"Hello World",而我不想打破常规,让我们来看看"Hello World"是怎样在django中输出的.

等等,我们的项目里多了一个文件db.sqlite3,这是什么文件.这个多出来的文件,是因为mysite/setting.py配置了数据库是sqlite3数据库,配置如下:

```
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',
        'NAME': os.path.join(BASE_DIR, 'db.sqlite3'),
    }
}
```

以上信息是说,使用了sqlite3数据库,数据库的名字是os.path.join(BASE_DIR, 'db.sqlite3'),这个目录就是在mysite/下.

首先我们得先建一个app,一个网站就是由多个app组个而成.

```
python manage.py startapp blog
```

这样就新建了一个名叫blog的app应用,看看现在mysite有什么文件和文件夹.

```
.
├── blog
│   ├── admin.py
│   ├── apps.py
│   ├── __init__.py
│   ├── migrations
│   │   └── __init__.py
│   ├── models.py
│   ├── tests.py
│   └── views.py
├── db.sqlite3
├── manage.py
└── mysite
    ├── __init__.py
    ├── __init__.pyc
    ├── settings.py
    ├── settings.pyc
    ├── urls.py
    ├── urls.py~
    ├── urls.pyc
    ├── wsgi.py
    └── wsgi.pyc

3 directories, 18 files

```

blog文件夹就是新建的app应用,里面的文件models.py是blog app的数据库操作,views.py是当要访问blog这个app的时候要怎样处理,其他文件暂时不会用到,用到时会逐一解释.

新建了一个blog app,在配置文件里的使用哪些app增加上blog即可.还记得配置文件是哪个文件吧,mysite/setting.py .

```
# Application definition

INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    #这是新加入的app应用,注意末尾的','号
    'blog', 
]

```

现在来看看mysite/urls.py文件.

```
from django.conf.urls import url
from django.contrib import admin

urlpatterns = [
    url(r'^admin/', admin.site.urls),
]
```

urlpatterns里面包含了一个url(r'^admin/', admin.site.urls),第一个参数是一个正则表达式匹配,就是匹配开头是admin/的url地址,第二个参数是当匹配到时采取的动作,也就是说当访问了127.0.0.1:8000/admin/时,处理函数是admin.site里的urls函数.好了,现在我们增加一个url,当访问127.0.0.1:8000/index/ 的时候,让它显示"Hello World".

```
from django.conf.urls import url
from django.contrib import admin
#导入模块
from blog.views import index

urlpatterns = [
    url(r'^admin/', admin.site.urls),
    url(r'^index$', index), #当访问开头是index时,处理函数是blog.views.index
]
```

现在blog.views下的index方法还没有写,现在编辑blog/views.py,输入如下文字:

```
from django.http import HttpResponse

def index(request):
    return HttpResponse("<h1>Hello World</h1>")
```

这个index函数,会返回一个HttpResponse,内容是<h1\>Hello World<h1\>.

浏览器输入:

```
127.0.0.1:8000/index
```

你就会得到"Hello World".

#持续更新中...

---

#关于本教程

---

本教程可供任何人免费使用学习,但严禁用于商业用途.**(纪念伟大的人Aaron swartz 1986-2013)**由于作者水平所限,本教程难免有错误之处,还望大家海涵并指出,有人很问题请用邮箱联系我LBaoCode@Gmail.com .