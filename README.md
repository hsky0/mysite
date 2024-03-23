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

## 