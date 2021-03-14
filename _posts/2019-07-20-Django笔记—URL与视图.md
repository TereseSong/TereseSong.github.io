---
layout:     post
title:      Django笔记
subtitle:   Django笔记-URL与视图
date:       2019-07-20 22:20:00
author:     terese
header-img: img/post-16.jpg
catalog:   true
tags:
    - Django
    - 笔记
---

1. Django 基本命令``python manage.py runserver``
以指定端口号``python manage.py runserver 9000``
如果想要在其他电脑上也能访问本网站，那么需要指定ip地址为0.0.0.0。
``python manage.py runserver 0.0.0.0:8000。``
2. URL映射
实例
```py
from django.contrib import admin
from django.urls import path
from book import views

urlpatterns = [
    path('admin/', admin.site.urls),
    path('book/',views.book_list)
]
```
3. URL添加参数
实例
```	py
from django.contrib import admin
from django.urls import path
from book import views

urlpatterns = [
    path('admin/', admin.site.urls),
    path('book/',views.book_list),
    path('book/<book_id>/',views.book_detail)
]
```
views.py中
```py
def book_detail(request,book_id):
    text = "您输入的书籍的id是：%s" % book_id
    return HttpResponse(text)
```
也可以通过查询字符串的方式传递一个参数过去，实例：
在访问的时候就是通过/book/detail/?id=1即可将参数传递过去
```py
urlpatterns = [
    path('admin/', admin.site.urls),
    path('book/',views.book_list),
    path('book/detail/',views.book_detail)
]
```
#views.py中
```py
def book_detail(request):
    book_id = request.GET.get("id")
    text = "您输入的书籍id是：%s" % book_id
    return HttpResponse(text)
```
4. URL中包含另一个urls模块#include
实例
```py
#first_project/urls.py文件：
from django.contrib import admin
from django.urls import path,include
urlpatterns = [
    path('admin/', admin.site.urls),
    path('book/',include("book.urls"))
]
```
访问书籍详情页面的url的时候就通过book/detail/<id>来访问。
```py
#book/urls.py文件：
from django.urls import path
from . import views

urlpatterns = [
    path('list/',views.book_list),
    path('detail/<book_id>/',views.book_detail)
]
```
* include(pattern,namespace=None)：直接把其他app的urls包含进来，传递namespace参数来指定一个实例命名空间
实例
```py
#主urls.py文件：
from django.urls import path,include
urlpatterns = [
    path('movie/',include('movie.urls',namespace='movie'))
]
```
movie/urls.py中指定应用命名空间
```py
from django.urls import path
from . import views

#应用命名空间
app_name = 'movie'
urlpatterns = [
    path('',views.movie,name='index'),
    path('list/',views.movie_list,name='list'),
]
```
include((pattern,app_namespace),namespace=None)：在包含某个app的urls的时候，可以指定命名空间，这样做的目的是为了防止不同的app下出现相同的url，这时候就可以通过命名空间进行区分。
实例：
```py
from django.contrib import admin
 from django.urls import path,include
 urlpatterns = [
     path('admin/', admin.site.urls),
     path('book/',include(("book.urls",'book')),namespace='book')
 ]
```
5. path函数
path函数的定义为：path(route,view,name=None,kwargs=None)
* route参数：url的匹配规则。这个参数中可以指定url中需要传递的参数，比如在访问文章详情页的时候，可以传递一个id。传递参数是通过<>尖括号来进行指定的。并且在传递参数的时候，可以指定这个参数的数据类型，比如文章的id都是int类型，那么可以这样写<int:id>，以后匹配的时候，就只会匹配到id为int类型的url，而不会匹配其他的url，并且在视图函数中获取这个参数的时候，就已经被转换成一个int类型了。其中还有几种常用的类型：
    * str：非空的字符串类型。默认的转换器。但是不能包含斜杠。
    * int：匹配任意的零或者正数的整形。到视图函数中就是一个int类型。
    * slug：由英文中的横杠-，或者下划线_连接英文字符或者数字而成的字符串。
    * uuid：匹配uuid字符串。
    * path：匹配非空的英文字符串，可以包含斜杠。
    * view参数：可以为一个视图函数或者是类视图.as_view()或者是django.urls.include()函数的返回值。
* name参数：这个参数是给这个url取个名字的，这在项目比较大，url比较多的时候用处很大。
* kwargs参数：有时候想给视图函数传递一些额外的参数，就可以通过kwargs参数进行传递。这个参数接收一个字典。传到视图函数中的时候，会作为一个关键字参数传过去。
实例
```
 from django.urls import path
 from . import views

 urlpatterns = [
     path('blog/<int:year>/', views.year_archive, {'foo': 'bar'}),
 ]
```
那么以后在访问blog/1991/这个url的时候，会将foo=bar作为关键字参数传给year_archive函数。
6. re_path函数，利用正则表达式解析
实例：
```py
from django.urls import path, re_path
from . import views

    urlpatterns = [
        path('articles/2003/', views.special_case_2003),
        re_path(r'articles/(?P<year>[0-9]{4})/', views.year_archive),
        re_path(r'articles/(?P<year>[0-9]{4})/(?P<month>[0-9]{2})/', views.month_archive),
        re_path(r'articles/(?P<year>[0-9]{4})/(?P<month>[0-9]{2})/(?P<slug>[\w-_]+)/', views.article_detail),
    ]
```
7. 指定默认参数
	``
def page(request, num=1):
   pass
   ``
8.  URL反转
已知视图函数，反转回他的url
实例
``reverse('book:list')``
``> /book/list/``
如果这个url中需要传递参数，那么可以通过kwargs来传递参数。
实例：
```
reverse("book:detail",kwargs={"book_id":1})
> /book/detail/1
```