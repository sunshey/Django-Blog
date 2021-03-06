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
9. 接下来就来编写models，创建类对象，我这里创建两个对象，如下图：
```
from django.db import models


# Create your models here.
class Question(models.Model):
    question_text = models.CharField(verbose_name='问题', max_length=100)
    pub_date = models.DateTimeField()

    def __str__(self):
        return self.question_text


class Choice(models.Model):
    question = models.ForeignKey(Question, on_delete=models.CASCADE)
    choice_text = models.CharField(max_length=200)
    votes = models.IntegerField(default=0)

    def __str__(self):
        return self.choice_text
```
10. 创建了models对象后，就要运行命令同步对象，如下：
```
python manage.py makemigrations  # 同步对象    

python manage.py migrate   # 将同步的对象生成数据库对象

```
11. 在views.py中创建一个最简单视图处理函数，如下：
```
from django.shortcuts import render
from django.http import HttpResponse

# Create your views here.

def index(request):
    return HttpResponse('ok')
    
```
12. 在应用Polls中创建urls.py文件，文档结构如下：
```
Blog
  ...
Polls
  migrations
  __init__.py
  admin.py
  ...              # 省略号代表和上面结构一样，没有列出来
  urls.py

```
13. 打开刚刚创建的urls.py文件，在里面配置url匹配，如下：
```
from django.urls import path
from . import views

urlpatterns = [
    path('index', views.index, name='index'),
]
```
然后打开Blog项目下urls.py文件，将刚刚配置url添加进来，如下：
```
from django.contrib import admin
from django.urls import path,include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('poll/',include('Polls.urls'))
]
```
14. 以上配置好之后，运行第7步的命令启动服务器，并在浏览器地址栏输入：http：127.0.0.1:8000/poll/index,就会在浏览器看到OK  
15. django强大的一个原因就是它自带后台管理系统，通过运行命令创建超级管理员，如下：
```
python manage.py createsuperuser
```
按照提示，输入完成就创建成功了  
16. 在浏览器中输入http:127.0.0.1:8000/admin,就进入后台管理系统,输入刚才创建的超级用户就可以登录进去了  
17. 进入后台管理系统后发现没有我刚刚创建的应用，怎么办呢？打开admin.py,将应用注册，如下：
```
from django.contrib import admin
from .models import Question,Choice
# Register your models here.

admin.site.register(Question)
admin.site.register(Choice)
```
刷新刚才的管理后台，就会看到我们创建的对象了，就可以在这里添加question和choice对象了  
18. 接下来在视图中使用我们的对象，打开view.py,添加如下代码：
```
from django.shortcuts import render
from django.http import HttpResponse
from .models import Question


# Create your views here.

def index(request):
    question_list = Question.objects.all()[:10]
    context = {
        question_list: question_list
    }
    return render(request,'polls/index.html',context)
 ```
 我们还要在Polls下创建templates文件夹，然后在templates文件夹下创建polls文件夹（先这样创建吧，后面再讲原因），然后在polls中创建index.html,html中的内容如下： 
 ```
 {% if question_list %}                  # % 是Django的模板表达式，以后会大量使用
    {% for question in question_list %}
        <li>{{ question }}</li>
    {% endfor %}
{% endif %}
```
再次访问http:127.0.0.1:8000/poll/index,就会看到我们刚才在管理系统后台添加的数据  
19. 到这里Django开发基本完成了，不过还有一些常用的命令要记一下，如下：
* python manage.py shell 进入调试模式
* python manage.py help 获取命令
* python manage.py sqlmigrate Polls 0001  查看生成的数据库过程  
20. models的操作也是非常重要的，这个要根据文档来学习

注意事项
---
总结
---
