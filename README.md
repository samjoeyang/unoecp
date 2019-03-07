![不丁Logo](https://www.zhenzhidaole.com/media/filer_public_thumbnails/filer_public/19/33/193321a0-48f2-4632-aeb3-4a20ea1e98e6/logo.png__200.0x200.0_q85_subsampling-2.png "不丁")

[不丁官网](https://www.zhenzhidaole.com/ "不丁官网")

---
Base on DjangoCMS and Front-end Themes,include [startbootstrap-sb-admin-2-gh-pages](https://startbootstrap.com/template-categories/all/ "startbootstrap-sb-admin-2-gh-pages") and [clean-blog](https://startbootstrap.com/themes/clean-blog/ "clean-blog")

* 模版配置部分参考[蜗牛博客](http://www.snailtoday.com/archives/7652)
* 想要手动安装的可以参考[这里](https://www.jianshu.com/p/353ac259245c),不过本人没有成功，需要仔细阅读
* [DjangoCMS文档](http://docs.django-cms.org/en/latest/)

|Django Versions|Python Versions|Remarks|
|:-|---|:------:|
|1.8|2.7,3.2,3.3,3.4,3.5||
|1.9,1.10|2.7,3.4,3.5||
|1.11|2.7,3.4,3.5||
|2.0|3.4,3.5,3.6||
|2.1|3.5,3.6,3.7||

---

## **DjangoCMS安装：**
python环境请自己提前安装好，也可以进入虚拟环境，（本人在虚拟环境下出现过多次报错，放弃，直接用系统环境下配置安装）

1. 执行以下命令安装django CMS installer

  `pip install djangocms-installer`

2. 建立项目目录
执行mkdir命令，建立djangocms目录，执行cd命令进入这个目录。

  `mkdir djangocms && cd djangocms`

3. 建立一个项目
执行以下命令，建立一个名为unoecp的新的django项目,这个命令的意思是运行django CMS installer，“-f”表示同时安装Django Filer，“-p .”表示使用当前目录作为新项目的目录。(Django Filer是一个用来管理文件和处理图片的组件，几乎所有的django CMS项目都用到它。)

  `djangocms -f -p . unoecp`

执行上面的命令之后，会出现一个“please wait while i install dependencies”的提示，并且会停留一段时间，此时等待即可。

4. 启动服务，可指定端口

  `python manage.py runserver 8000`

5. 检测是否安装成功
打开浏览器，输入`http://localhost:8000/` 如果正常浏览就表示DjangoCMS安装成功了。

6. 登陆后台
输入用户名：admin，密码：admin，进行登陆。

---
>手动创建root用户,命令行里输入
>
>   `python manage.py createsuperuser`
>
>按照提示步骤输入用户名（默认root）、密码、邮箱地址，然后在运行服务
>
>   `python manage.py runserver 8000`
>
---

## **模版配置**
1. 把模版的css、js、images等文件或目录复制到static文件夹下面,可以创建自己的目录，如“unoecp”

2. 在模版的页面文件头部添加

  `{% load staticfiles i18n cms_tags sekizai_tags menu_tags %}`

3. 修改css文件路径,格式如下

  `<link href="{% static 'unoecp/css/main.min.css' %}" rel="stylesheet"> `

4. 修改js文件路径,格式如下

  `<script src="{% static 'unoecp/js/jqBootstrapValidation.js' %}"></script>`

5. 修改静态图片路径,格式如下

  `<header class="intro-header" style="background-image: url({% static 'unoecp/img/about-bg.jpg' %})">`
  
6. 其他静态资源，格式参考如下

  `{% static 'folder1/folder2/filename.ext' %}`


## **添加django CMS工具栏**

下面几个tags添加到模板中

1. `</head>`之前，引入css

  `{% render_block "css" %}`
  Loads necessary CSS files for django CMS and its addons

2. `<body>`后面第一行
  
  `{% cms_toolbar %}`
  Loads the toolbar itself

3. `</body>`后面，引入js

  `{% render_block "js" %}`
  Loads necessary Javascript files for django CMS and its addons

## **CMS的配置**

### **页面配置**

1. 内容标签
```
<div class="col-lg-8 col-lg-offset-2 col-md-10 col-md-offset-1">
      {% block content %}{% endblock %}
</div>
```
2. 导航条
```
<ul class="nav navbar-nav navbar-right">
   {% show_menu 0 0 100 100 %}
</ul>
```
3. 配置内容区域,主要是添加占位符`{% placeholder "content" %}`,在templates文件夹下面，新建一个content.html文件，将以下内容复制进去
```
{% extends "base.html" %}
{% load cms_tags %}

{% block content %}
    {% placeholder "content" %}
{% endblock content %}
```
4. django CMS占位符的高级使用
`or %}`的标签的意思是，占位符没有内容输出的时候，你可以通过`or %}`来定义显示的内容。如果占位符没有内容输出，我们让它显示about-bg.jpg。
```
<!-- Page Header -->
{% placeholder header or %}
<header>...about-bg.jpg...</header>
```
5. 配置文章标题，使用{% page_attribute "page_title" %}这个模板标签

可以通过Page > Page Settings来修改这个标题。

将以下的代码：
```
<div class="page-heading">
    <h1>About Me</h1>
    <hr class="small">
    <span class="subheading">This is what I do.</span>
</div>
```
修改成这样：
```
<div class="page-heading">
    <h1>{% page_attribute "page_title" %}</h1>
</div>
````
6. 添加内容,不做详细介绍，进入DjangoCMS的编辑模式，点击“结构”节点，进入结构视图模式，视图模式是我们给网站添加内容、设定网站外观时经常用到的，在这里你可以看到我们之前插入的两个占位符。

### **集成博客/新闻模块**
[aldryn-newsblog文档](https://pypi.org/project/aldryn-newsblog/)

1. 安装Aldryn News & Blog
进入我们的项目所在的开发环境，执行命令安装,环境不同安装目录有所不同，请注意

`pip install aldryn-newsblog`

2. 配置Aldryn News & Blog
打开项目的settings.py文件，将以下代码加入到`INSTALLED_APPS`的`'cms'`后面。
```
# you will probably need to add these
'aldryn_apphooks_config',
'aldryn_categories',
'aldryn_common',
'aldryn_newsblog',
'aldryn_people',
'aldryn_translation_tools',
'parler',
'sortedm2m',
'taggit',
```
```
# you'll almost certainly have these installed already
'djangocms_text_ckeditor',
'easy_thumbnails',
'filer',
```

3. 执行命令，进行数据库同步。

`python manage.py migrate`


### **添加博客文章**

### **设定文章页的格式**

### **最近发布文章列表及完善**

### **设置头部动态效果**

## **开发新应用UNOECP**

Django创建项目的命令是`django-admin startproject UNOECP`,不过本项目是基于DjangoCMS的，可以略过项目创建的过程，直接创建应用`app`，命令如下

`python manage.py startapp UNOECP`


***
待补充内容：
* settings文件配置说明
* 多站点多域名情况下的SITE_ID的配置
***


## **Next...**
