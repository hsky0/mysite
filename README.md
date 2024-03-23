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
    path('', include((hp_url, 'homepage'), namespace='homepage')),
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
    <p>
    This is mysite!
    </p>
    <p> A test version.</p>
   ```
- 随着开发的进行，该文件夹中的文件会越来越多，越来越丰富

## 连接外部数据库
1. 安装依赖文件
   ```console
    pip install psycopg2-binary
    pip install dj-database-url
   ```

    |Package	|Description|
    |-|-|
    |psycopg2	|This is the most popular Python adapter for **communicating** with a **PostgreSQL database**.|
    |DJ-Database-URL	|This enables you to specify your database details via the **DATABASE_URL** environment variable(you’ll obtain your database’s URL from the Render Dashboard).|

2. 修改setting.py文件
   ```python
    # Import dj-database-url at the beginning of the file.
    import dj_database_url


    # Replace the SQLite DATABASES configuration with PostgreSQL:
    DATABASES = {
        'default': dj_database_url.config(
            # Replace this value with your local database's connection string.
            default='postgresql://postgres:postgres@localhost:5432/mysite',
            conn_max_age=600
        )
    }
   ```
- 这里default后的值应修改为直接创建的PostgreSQL数据库的URL链接
- python3 -m gunicorn mysite.asgi:application -k uvicorn.workers.UvicornWorker (运行服务器)


## 建立静态服务
1. 安装'whitenoise[brotli]'包
   pip install 'whitenoise[brotli]'

2. 修改setting.py文件
   ```python
    MIDDLEWARE = [
    'django.middleware.security.SecurityMiddleware',
    'whitenoise.middleware.WhiteNoiseMiddleware',
    ...
    ]


    # Static files (CSS, JavaScript, Images)
    # https://docs.djangoproject.com/en/5.0/howto/static-files/

    # This setting informs Django of the URI path from which your static files will be served to users
    # Here, they well be accessible at your-domain.onrender.com/static/... or yourcustomdomain.com/static/...
    STATIC_URL = '/static/'
    STATIC_ROOT = os.path.join(BASE_DIR,'static') 
    # This production code might break development mode, so we check whether we're in DEBUG mode
    if not DEBUG:
        # Tell Django to copy static assets into a path called `staticfiles` (this is specific to Render)
        STATIC_ROOT = os.path.join(BASE_DIR, 'staticfiles')
        # Enable the WhiteNoise storage backend, which compresses static files to reduce disk use
        # and renames the files with unique names for each version to support long-term caching
        STATICFILES_STORAGE = 'whitenoise.storage.CompressedManifestStaticFilesStorage'

   ```
   - 记住，在STATIC_URL = '/static/'后一定要加上STATIC_ROOT = os.path.join(BASE_DIR,'static')，不然会报错
   - 最后，在setting.py文件中，ALLOWED_HOSTS = ['*']，该变量的括号里要加上*号
## 创建管理用户 

1. 命令  
   python3 manage.py createsuperuser

2. 按提示输入相应的信息

## 部署到render
1. 创建依赖文件requirements.txt
2. 将该项目所用到的依赖包写入该文件
   ```txt
   dj-database-url==2.1.0
   Django==5.0.3
   gunicorn==21.2.0
   psycopg2-binary==2.9.9
   sqlparse==0.4.4
   asgiref==3.8.0
   uvicorn==0.29.0
   whitenoise==6.0.0
   ```
   - 以上为本次项目所需的依赖包
  

3. 将项目推送到Github仓库
4. 在render网站上创建web service


## 最后，一个非常重要的有点：配置web server其地区一定要和数据库的一致，如果不一致就删掉该web重新创建一个