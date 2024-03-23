# Django创建Web程序

## Django项目的创建

1. 安装虚拟环境  
   python3 -m venv venv

2. 激活虚拟环境  
   source/venv/bin/activate

3. 安装django  
   pip isntall django       (可以指定安装的版本：pip install django=5.0.3)

4. 创建项目  
   django-admin startproject mysite .   (注意一定要加最后的点号，不然创建之后没有manage.py文件)

5. 运行服务器，测试创建项目是否成功  
   python3 manage.py runserver

## 编写简单的页面

1. 创建应用程序  
   python3 manage.py startapp homepage      (homepage是我的应用程序的名称)

2. 在mysite/setting.py文件中添加应用程序
   ```python
    INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',

    # 我的应用程序
    'homepage',
    ]
   ```

3. 在mysite/urls.py文件中添加应用程序的URL
   ```python
    from django.contrib import admin
    from django.urls import path, include

    from homepage import urls as hp_url

    urlpatterns = [
        path('admin/', admin.site.urls),

        #我的应用程序
        path('', include(hp_url, 'homepage'), name='homepage'),
    ]

   ```

4. 定义主页面的URL，在homepage命令下创建一个名为urls.py的文件
   ```python
    from django.urls import path, include

    from . import views

    urlpatterns = [

        path('homepage/', views.index, name='index'),
    ]
   ```

5. 编写视图函数(homepage/views.py)
   ```python
    from django.shortcuts import render

    # Create your views here.
    def index(request):
        return render(request, 'homepage/index.html')
   ```  

6. 编写模板，在homepage目录下创建templates文件夹，再在该文件夹下创建homepage文件夹，再在该文件夹下创建文件index.html
   ```html

   ```