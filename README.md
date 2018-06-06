* [项目介绍](项目介绍)
* [开发前准备](开发前准备)
* [开发流程](开发流程)
* [注意事项](注意事项)
* [总结](总结)


项目介绍
---
###### 这是一个使用Django开发的博客系统，具有添加博客、查看博客、删除博客等功能，同时还有一个后台管理系统

开发前准备
---
###### 由于使用python和Django开发框架开发，因此需要先安装python和Django环境，目前我电脑上安装的python3.6版本，Django框架直接通过命令就可以安装：
```
pip install django
```

开发流程
---
###### Django一般是通过命令行来创建项目，创建APP，运行server等，因此要熟练记住几个常用命令，同时这也是一般的开发步骤：  
1. 创建一个项目目录，然后cd到这个目录  
2. 通过Django命令创建项目，命令如下：
```
django-admin startproject Blog  #Blog是项目名
```
3. 通过上述命令创建项目成功后，就会在目录中看到刚才创建的Blog项目，目录结构如下：
```
Blog
  Blog
    _init_.py
    settings.py   # 项目配置文档，添加APP、设置数据库等重要操作都在这个文件中
    urls.py       # 路径映射，在浏览器中输入的url在这个文件中进行匹配
    wsgi.py
  db.sqlite3      # Django自带的数据库，可在settings中修改
  manage.py       # 命令工具，后续创建APP，同步models，启动服务器都要通过这个命令
```
4. cd到Blog项目目录，通过如下命令创建app:  
```
python manage.py startapp Polls # Polls是app名
````
5. 接着就会发现在Blog项目中多了一个Polls包，这就是刚刚创建的app,目录结构像下面这样：  
```
Blog
  Blog               #这个目录上面已经介绍了，就不展示了
  Polls
    migrations
      __init__.py
    __init__.py  
    admin.py       # 将models.py中创建的对象注册到管理后台
    apps.py       
    models.py      # 创建对象，然后将会把这些对象同步到数据库 ①
    tests.py     
    views.py       # 接受请求，返回响应，②
  db.sqlite3
  manage.py
 ```
 ###### 注：上面用小圆圈标注的是经常操作的文件，很重要  
 6. 在settings.py中将刚创建的app注册，否则以后看不到效果,如图：
 ```
 INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'Polls.apps.PollsConfig'   #这里就是我刚才创建的app,名字是Polls
]
```
7. 接下来可以运行命令启动服务器，看下简单的效果：
```
python manage.py runserver
```
8. 在浏览器中输入：http:127.0.0.1:8000，就会看到Django的安装成功的页面，这里就不展示了，自己尝试

注意事项
---
总结
---
