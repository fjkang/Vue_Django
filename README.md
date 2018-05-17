## 前端vue部分
### 创建vue环境
1. 使用vue-cil创建vue项目
~~~bash
npm i -g vue-cli
vue-init webpack frontend
~~~
2. 进入frontend文件夹,下载依赖文件
~~~bash
cd frontend
npm i
npm run build
~~~
其中npm run build用于生成dist文件夹
------------------------------------------
## 后端django部分
### 创建虚拟环境:我使用是conda
1. 创建环境:conda create -p <存放的路径> <需要安装的包>
~~~bash
conda create -p E:\BVND\Mywebsite\django-env python
~~~
### 进入虚拟环境,创建django项目:
1. 激活环境:activate <环境路径>
~~~bash
activate E:\BVND\Mywebsite\django-env
~~~
进入到环境后,命令行中会显示为:
~~~bash
(E:\BVND\Mywebsite\django-env) E:\BVND\Mywebsite>
~~~
2. 使用django:django-admin startproject <项目名称>
~~~bash
(E:\BVND\Mywebsite\django-env) E:\BVND>django-admin startproject Mywebsite
~~~
3. 进入项目,创建app
~~~bash
(E:\BVND\Mywebsite\django-env) E:\BVND>cd Mywebsite
(E:\BVND\Mywebsite\django-env) E:\BVND\Mywebsite>python manage.py startapp backend
~~~
### 设置指向前端vue的对应文件
1. 修改 DIRS
~~~python
TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': ['frontend/dist'], # 这里修改成dist文件夹
        'APP_DIRS': True,
        'OPTIONS': {
            'context_processors': [
                'django.template.context_processors.debug',
                'django.template.context_processors.request',
                'django.contrib.auth.context_processors.auth',
                'django.contrib.messages.context_processors.messages',
            ],
        },
    },
]
~~~
2. 修改statci文件夹路径
~~~python
STATICFILES_DIRS = [
    os.path.join(BASE_DIR, '/frontend/dist/static')
] # 这里修改为vue的static目录
~~~
3. 修改url.py中的模板指向
~~~python
from django.contrib import admin
from django.urls import path
from django.views.generic.base import TemplateView # 导入指定模板的包

urlpatterns = [
    path('admin/', admin.site.urls),
    path(r'', TemplateView.as_view(template_name='index.html'))
    # 这里修改模板为vue的html
]
~~~
------------------------------------------
## 创建django的后台超级管理员
1. 从models.py生成数据库表
~~~bash
python manage.py migrate
~~~
2. 创建超级用户
~~~bash
python manage.py createsuperuser
~~~
输入user,email,password即创建成功
3. 进入后台测试
~~~bash
python manage.py runserver
~~~
浏览器进入http://127.0.0.1:8000/admin

------------------------------------------
## 云服务部分
* 切换到root用户:
~~~bash
su -i
~~~