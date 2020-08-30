# 一.Django框架

## 1.工程搭建

**1.配置文件**

1.Django的国际化

```python
#setting.py文件
USE_I18N = True
USE_L10N = True
USE_TZ = True


LANGUAGES = (
    ('en', ('English')),
    ('zh-hans', ('中文简体')),
    ('zh-hant', ('中文繁體')),
)
```

工程创建/创建子应用/创建视图:

```python
django-admin startproject 工程名
python manage.py runserver
python manage.py startapp users

定义路由url地址
第一种方法:
		1.demo/urls.py中添加子应用的路由数据:
		urlpatterns = [
    	url(r'^admin/', admin.site.urls),
        url(r'^users/', include(users.urls),namespace='users'), 
]
		2.在users/urls.py文件中定义路由信息:
		urlpatterns = [
    		url(r'^index/$', views.index ,name='index')),
		]
		

第二种方法 全部路由信息都定义在主路由文件中，子应用不再设置urls.py:
 		demo/urls.py中添加子应用的路由数据:
		urlpatterns = [
    	url(r'^admin/', admin.site.urls),
     	url(r'^users/index/$', users.views.index)
]

reverse反解析
	 	from django.urls import reverse  # 注意导包路径

```

##2.请求与响应

一:请求Request

0 .利用http协议向服务器传参的几种途径:

```python
"""
    1.提取URL的特定部分 正则表达式
    2.查询字符串 request.GET
    3.请求体 注释掉settings.py文件中CSRF中间件
    		3.1 Form Data:request.POST(能用来获取POST方式的请求体表单数据)
    		3.2 Non-Form Data:request.body
    			(request.body返回bytes类型。)
    4.报文头 request.META

"""
    

```

1. URL路径参数

2. Django中的QueryDict对象

```
1.定义:QueryDict类型的对象用来处理同一个键带有多个值的情况
2.get:获取最后一个值/不存在则返回None值 dict.get('键',默认值)
  getlist():值以列表返回 dict.getlist('键',默认值)
```

3. Django中的Query String

```python
# /qs/?a=1&b=2&a=3
request.GET.get('a')
request.GET.getlist('a')

注意：查询字符串不区分请求方式，即使客户端进行POST方式的请求
```

4.请求体(Form Data/Non-Form Data)(**POST**、**PUT**、**PATCH**、**DELETE**。)

```
注意:Django默认开启了CSRF防护，在测试时可以关闭CSRF防护机制
```

```python
#Form Data
a = request.POST.get('a')
alist = request.POST.getlist('a')
注意:request.POST属性获取，返回QueryDict对象
	request.POST只能用来获取POST方式的请求体表单数据
```

```python 
#Non-Form Data(JSON,XML)
json_str = request.body   #request.body返回bytes类型。
req_data = json.loads(json_str)
```

5.请求头

```
request.META属性获取请求头headers中的数据，request.META为字典类型
```

6.其他常用HttpRequest对象属性

```
    1.request.method
    2.request.user
    3.request.encoding
    4.request.FILES
    
```

二 :响应Response

```
1.HttpResponse
2.HttpResponse子类
3 JsonResponse
```

三:Cookie

1.设置Cookie

```
HttpResponse.set_cookie(cookie名, value=cookie值, max_age=cookie有效期)
```

2.读取cookie

```
cookie1 = request.COOKIES.get('itcast1')
```

四 session

```
1.session的三种存储方式
	1.1数据库
	1.2本地缓存
	1.3混合存储
2.session的操作
	2.1request.session.get('键',默认值)#
	2.2request.session.clear()#清除所有session，在存储中删除值部分。
	2.3request.session.flush()#清除session数据，在存储中删除session的整条数据。
	2.4del request.session['键']#删除某个键及对应的值。
```

## 3.类视图和中间件

类视图

1.定义以及优点

```
定义:以函数的方式定义的视图称为函数视图，用类来定义一个视图，称为类视图。
优点:可读性好复用性高
```

2.类视图如何使用

```
2.1配置路由时，使用类视图的as_view()方法来添加。
```

3.类视图原理是什么

```

```

4.类视图中如何使用装饰器

```python
from django.utils.decorators import method_decorator

1.在URL配置中装饰
urlpatterns = [
    url(r'^demo/$', my_decorate(DemoView.as_view()))
]

2.# 为特定请求方法添加装饰器
@method_decorator(my_decorator, name='get')
class DemoView(View):
    def get(self, request):
        print('get方法')
        return HttpResponse('ok')

    def post(self, request):
        print('post方法')
        return HttpResponse('ok')
        
3.# 为特定请求方法添加装饰器
class DemoView(View):
    @method_decorator(my_decorator)  # 为get方法添加了装饰器
    def get(self, request):
        print('get方法')
        return HttpResponse('ok')
    
    
 
```

中间件

```python
"""
中间件与装饰器的区别: 作用域范围不同装饰器只作用某个使用了装饰器的视图函数,而中间件则作用于所用的函数 中间件可以介入Django的请求和响应处理过程，修改Django的输入或输出
"""

def my_middleware(get_response):
    # 1.此处编写的代码仅在Django第一次配置和初始化的时候执行一次。
    def middleware(request):
         # 2.此处编写的代码会在每个请求处理视图前被调用。
         print('before request 被调用')
         response = get_response(request)
         # 3.此处编写的代码会在每个请求处理视图之后被调用。
          print('after response 被调用')
          return response
     return middleware

"""
多个中间件的执行顺序:
				1.配置初始化中间件:先下后上
				2.请求视图前中间件:先上后下
				3.请求视图后中间件:先下后上

"""
```

##4.模板

**1.配置**

**2.定义模板**

**3.模板渲染**

```python 
#第一种
from django.http import HttpResponse
from django.template import loader

def index(request):
    context={'city': '北京'}
    template=loader.get_template('index.html') # 1.获取模板
	data=template.render(context)  # 2.渲染模板
    return HttpResponse(data)
                  
#第二种
from django.shortcuts import render
def index(request):
    context={'city': '北京'}
    data = render(request,'index.html',context)
    return data



#补充
def test(a):
    a = a
    b = 123
    c = "123"
    return locals()

print(test(123))
{'c': '123', 'b': 123, 'a': 123}
注意:返回一个函数中的所有变量时，用Local再好不过，
    但是当你只需要返回某些局部变量时则必须手动添加
```

**4.模板语法**

1.模板变量: {{}}

2.模板语句: for循环/if条件语句

```python
{% for item in 列表 %}
循环逻辑
{{forloop.counter}}表示当前是第几次循环，从1开始
{%empty%} 列表为空或不存在时执行此逻辑
{% endfor %}


#cycle
{% for post in posts%}
{{loop.cycle('odd','even')}} {{post.title}}
{% endfor %}

输出:
odd Post Title
even Second Post

```

| 变量           | 描述                                         |
| -------------- | -------------------------------------------- |
| loop.index     | 当前循环迭代的次数（从 1 开始）              |
| loop.index0    | 当前循环迭代的次数（从 0 开始）              |
| loop.revindex  | 到循环结束需要迭代的次数（从 1 开始）        |
| loop.revindex0 | 到循环结束需要迭代的次数（从 0 开始）        |
| loop.first     | 如果是第⼀次迭代，为 True 。                 |
| loop.last      | 如果是最后⼀次迭代，为 True                  |
| loop.length    | 序列中的项⽬数                               |
| loop.cycle     | 在⼀串序列间期取值的辅助函数。⻅下⾯示例程序 |

```python 
{% if ... %}
逻辑1
{% elif ... %}
逻辑2
{% else %}
逻辑3
{% endif %}

注意：运算符左右两侧不能紧挨变量或常量，必须有空格
{% if a == 1 %}  # 正确
{% if a==1 %}  # 错误
```

3.过滤器

```python 
# 变量|过滤器:参数   
1.字符串操作:
    safe,captialize,lower,upper,title.reverse
    format:
    striptags：渲染之前把值中所有的HTML标签都删掉
    truncate: 字符串截断
    1.<p>{{ '<em>hello</em>' | safe }}</p>
	2.<p>{{ 'hello' | capitalize }}</p>
    3.<p>{{ '%s is %d' | format('name',17) }}</p>
    4.<p>{{ '<em>hello</em>' | striptags }}</p>
    5.<p>{{ 'hello every one' | truncate(9)}}</p>
    
2.列表操作
	first,last,length,sum,sort
    1.<p>{{ [6,2,3,1,5,4] | sort }}</p>

3.语句过滤模块
	{% filter upper %}
    #⼀⼤堆⽂字#
    {% endfilter %}

4.自定义过滤器
#demo.py
@app.template_filter('lireverse')
def do_listreverse(li):
# 通过原列表创建⼀个新列表
temp_li = list(li)
# 将新列表进⾏返转
temp_li.reverse()
return temp_li

# demo.html
<br/> my_array 原内容：{{ my_array }}
<br/> my_array 反转：{{ my_array | lireverse }}
```

4 .注释

```python 
#单行注释语法
{#...#}

#多行注释使用comment标签
{% comment %}
...
{% endcomment %}
```

5 .模板继承 

```python
#父模板
{% block 名称 %}
预留区域，可以编写默认内容，也可以没有默认内容
{% endblock  名称 %}

#子模板
{% extends "父模板路径"%}
{% block 名称 %}
实际填充内容
{{ block.super }}用于获取父模板中block的内容
{% endblock 名称 %}
```

6.模板宏

```python
#1.定义
{% macro input(name,value='',type='text') %}
{% endmacro %}
#2.调用
{{ input('name' value='zs')}}

#macro.html
{% macro function(type='text', name='', value='') %}
{% endmacro %}

#demo.html
{% import 'macro.html' as func %}
{% func.function() %}
```

```html
<form>
<label>⽤户名：</label><input type="text" name="username"><br/>
<label>身份证号：</label><input type="text" name="idcard"><br/>
<label>密码：</label><input type="password" name="password"><br/>
<label>确认密码：</label><input type="password" name="password2"><br/>
<input type="submit" value="注册">
</form>

1.macro.html

{% macro input(label="", type="text", name="", value="") %}
<label>{{ label }}</label><input type="{{ type }}" name="{{ name }}" value="{{ valu
e }}">
{% endmacro %}

2.demo.html
{% import 'macro.html' as func %}

<form>
{{ func.input("⽤户名：", name="username") }}<br/>
{{ func.input("身份证号：", name="idcard") }}<br/>
{{ func.input("密码：", type="password", name="password") }}<br/>
{{ func.input("确认密码：", type="password", name="password2") }}<br/>
{{ func.input(type="submit", value="注册") }}
</form>

```

7.包含

```python
{% include 'hello.html' ignore missing %}  #include 的使⽤加上关键字ignore missing
```

**补充**

1.模板中如何导入静态文件

```html
1.
{% extends "父模板路径"%}
{% load static %}
<link rel="stylesheet" href="{% static 'style.css' %}">

2.
那么可以在settings.py中的TEMPLATES/OPTIONS添加’builtins’:[‘django.templatetags.static’]，这样以后在模版中就可以直接使用static标签，而不用手动的load了
```



## 5.数据库

**ORM框架:对象关系映射框架**

​    1.通过类和类对象就能操作它所对应的表格中的数据

​    2.根据我们设计的类自动帮我们生成数据库中的表格



1.使用django进行数据库开发的步骤如下：

```python
"""
1.配置数据库连接信息
2.在models.py中定义模型类
3.迁移
4.通过类和对象完成数据增删改查操作
"""
```

2.配置:工程同级子目录 __ init __.py中添加

```python
from pymysql import install_as_MySQLdb

install_as_MySQLdb()
```

3.定义模型类

```python
from django.db import models

#定义图书模型类BookInfo
class BookInfo(models.Model):
    btitle = models.CharField(max_length=20, verbose_name='名称')
    bpub_date = models.DateField(verbose_name='发布日期')
    bread = models.IntegerField(default=0, verbose_name='阅读量')
    bcomment = models.IntegerField(default=0, verbose_name='评论量')
    is_delete = models.BooleanField(default=False, verbose_name='逻辑删除')

    class Meta:
        db_table = 'tb_books'  # 指明数据库表名
        verbose_name = '图书'  # 在admin站点中显示的名称
        verbose_name_plural = verbose_name  # 显示的复数名称

    def __str__(self):
        """定义每个数据对象的显示信息"""
        return self.btitle

#定义英雄模型类HeroInfo
class HeroInfo(models.Model):
    GENDER_CHOICES = (
        (0, 'male'),
        (1, 'female')
    )
    hname = models.CharField(max_length=20, verbose_name='名称') 
    hgender = models.SmallIntegerField(choices=GENDER_CHOICES, default=0, verbose_name='性别')  
    hcomment = models.CharField(max_length=200, null=True, verbose_name='描述信息') 
    hbook = models.ForeignKey(BookInfo, on_delete=models.CASCADE, verbose_name='图书')  # 外键
    is_delete = models.BooleanField(default=False, verbose_name='逻辑删除')

    class Meta:
        db_table = 'tb_heros'
        verbose_name = '英雄'
        verbose_name_plural = verbose_name

    def __str__(self):
        return self.hname
```

```python
"""
1.数据库表名:默认以 小写app应用名_小写模型类名 
2.主键:默认创建的主键列属性为id，可以使用pk代替，pk全拼为primary key
3.属性命名限制 属性=models.字段类型(选项)
4.字段类型: TextField/DecimalField/DateField/DateField/FileField/ImageField/ForeignKey
5.选项:null如果为True，表示允许为空 blank 如果为True，则该字段允许为空白
	  null是数据库范畴的概念，blank是表单验证范畴的
6.外键 :CASCADE /PROTECT/SET_NULL/SET_DEFAULT

"""
```

4.迁移

```python
1.python manage.py makemigrations #生成迁移文件
2.python manage.py migrate # 同步到数据库中

```

5.演示工具的使用

```python
python manage.py shell
```

6.数据库操作

"1.模型类对象"
"2.模型类"

1. 增加

```python
from datetime import date
"1.模型类对象save" 
hero = HeroInfo(hname='孙悟空',hgender=0,hbook=book)
hero.save()

"2.模型类create"
HeroInfo.objects.create( hname='沙悟净', hgender=0, hbook=book)

```

2. 删除

```python
"1.模型类对象 delete"
hero = HeroInfo.objects.get(id=13)
hero.delete()
"2.模型类 objects.filter().delete()"
HeroInfo.objects.filter(id=14).delete()
```

3. 修改

```python

"1.模型类对象 save"
hero = HeroInfo.objects.get(hname='猪八戒')
hero.hname = '猪悟能'
hero.save()
"2.模型类 update"
HeroInfo.objects.filter(hname='沙悟净').update(hname='沙僧')
```

4. 查

   4.1 基本查询

```python
"""
1.all : BookInfo.objects.all()
2.get : BookInfo.objects.get(id=3)
3.count : BookInfo.oobjects.count()

"""
```

​		4.2 过滤查询

```python
一:"""
1. get:过滤单一结果
2. filter:过滤出多个结果
3. exclude :排除掉符合条件剩下的结果

"""

二属性与常量值的比较运算符:"""
语法格式:属性名称__比较运算符=值
比较运算符:
1.exact：相等。
		查询编号为1的图书:  
						BookInfo.objects.filter(id_eact=1)
						BookInfo.objects.filter(id=1)
						
2.模糊查询:
  contains：是否包含  
  				  查询书名包含'传'的图书:
  				  				BookInfo.objects.filter(btitle__contians='传')
  startswith、endswith：以指定值开头或结尾
  				  查询书名以'部'结尾的图书:
  				  				BookInfo.objeccts.filter(btitle__endswith='部)
  				  
3.isnull：是否为null。
				查询书名不为空的图书:
								BookInfo.objects.filter(btitle__isnull=False)
				
				
4.in：是否包含在范围内
				查询编号为1或3或5的图书:
								BookInfo.objects.filter(id__in=[1,3,5])
5.比较查询
  	gt 大于 (greater then)
  			查询编号大于3的图书:
  				BookInfo.objects.filter(id__gt=3)
                
    gte 大于等于 (greater then equal)
    lt 小于 (less then)
    lte 小于等于 (less then equal)
    
6.exclude:不等于
		  查询编号不等于3的图书
		  					BookInfo.objects.exclude(id=3)
7.__year、month、day、week_day、hour、minute、second
		  查询1980年后发表的图书:
		  					BookInfo.objects.filter(bpub_date__gt=date(1980))
"""

三 F对象和Q对象:"""
1.两个属性比较使用F对象: 
	From django.models imort F
	查询阅读量大于2倍评论量的图书:
						BookInfo.objects.filter(bread_gte=F('bcomment')*2)
2.实现逻辑或or的查询，需要使用Q()对象结合|运算符
	查询阅读量大于20，或编号小于3的图书，只能使用Q对象实现
						BookInfo.objects.filter(Q(bread__gt=20)|Q(pk_lt=3))
	查询编号不等于3的图书。
						BookInfo.objects.filter(~Q(pk=3))

"""
    
四 聚合函数:"""
Avg 平均，Count 数量，Max 最大，Min 最小，Sum 
	查询图书的平均阅读量:
					BookInfo.objects.aggregate(Sum('bread'))
	
注意:注意aggregate的返回值是一个字典类型,
	count函数的返回值是一个数字,
	使用count时一般不使用aggregate()过滤器。
"""
```

​		4.3排序

```python
"""
'bread': 升序
'-bread':降序

所有的书按照升序排序
				BookInfo.objects.all().order_by('bread')
"""

   	

```

​		4.4 关联查询

```python
"""
1. 基本关联查询:
1.一对多:
    	查询id为1的书里的所有英雄:
    	语法:一对应的模型类对象.多对应的模型类名小写_set 
    						b = BookInfo.objects.get(id=1)
    						b.heroinfo_set.all()
2.多对一:
    	查询id为1的英雄所对应的的书籍:
    	语法:多对应的模型类对象.多对应的模型类中的关系类属性名
    						h=HeroInfo.objects.get(id=1)
    						h.hbook
    						
    	语法:多对应的模型类对象.关联类属性_id
        	h = HeroInfo.objects.get(id=1)
			h.hbook_id





2. 关联过滤查询:
1.多模型类条件查询一模型类数据:
语法:关联模型类名小写__属性名__条件运算符=值
				查询图书，要求图书中英雄的描述包含"八":
				BookInfo.objects.filter(heroinfo__hcomment__contains='八')
                
2.由一模型类条件查询多模型类数据:
语法:一模型类关联属性名__一模型类属性名__条件运算符=值
				查询英雄,查询图书阅读量大于30的所有英雄
				HeroInfo.objects.filter(hbook__bread__gt=30)
"""
```

7.查询集 QuerySet

```
1.惰性执行:创建查询集不会访问数据库，直到调用数据时，才会访问数据库
2.缓存 :使用缓存的数据，减少了数据库的查询次数
```

8.管理器

```python
1.Django应用的每个模型类都拥有至少一个管理器,默认每一个模型类生成一个名为objects的管理器
2.自定义管理器
		
```

## 6.Admin站点

1.使用Django的管理模块

```python
"""
1.管理界面本地化:
			LANGUAGE_CODE = 'zh-hans' # 使用中国语言
			TIME_ZONE = 'Asia/Shanghai' # 使用中国上海时间
			
2.创建管理员: python manage.py createsuperuser

3.注册模型类:
		 在apps.py文件中
		 AppConfig.verbose_name 属性用于设置该应用的直观可读的名字，此名字在Django提供的Admin管理站点中会显
		 
		1.在 app.py文件中
		 from django.apps import AppConfig
		 class BooktestConfig(AppConfig):
    	 name = 'booktest'
         verbose_name = '图书管理
		 
		2.在admin.py文件中
             from django.contrib import admin
    		 from booktest.models import BookInfo,HeroInfo
			 admin.site.register(BookInfo)
			 
         
4.定义与使用Admin管理类:
		  		4.1定义Admin管理类:继承自admin.ModelAdmin
		  						在admin.py中定义
		  						 class BookInfoAdmin(admin.ModelAdmin)
                                 pass
		  		4.2:
                	admin.site.register(BookInfo,BookInfoAdmin)
                	@admin.register(BookInfo)
                    class BookInfoAdmin(admin.ModelAdmin):
                        pass
"""
```

1.调整表页展示

```python
"""
1.页大小:(在admin.py/管理类) list_per_page=100
2."操作选项"的位置:(在admin.py/管理类) actions_on_top=True/actions_on_bottom=False
3.列表中的列:(在admin.py/管理类)   list_display = ['id','btitle']
4.将方法作为列:(在models.py)

			4.1 在admin站点中指定显示的列名
			def pub_date(self):
        		return self.bpub_date.strftime('%Y年%m月%d日')
       			pub_date.short_description = '发布日期
       		4.2为方法指定排序依据
       			 pub_date.admin_order_field = 'bpub_date'
       			 
5.关联对象 (在models.py)
		无法直接访问关联对象的属性或方法，可以在模型类中封装方法，访问关联对象的成员
		  def read(self):
             return self.hbook.bread
          read.short_description = '图书阅读量'


6.右侧栏过滤器(在admin.py/管理类)
		 list_filter = ['hbook', 'hgender']
7.搜索框(在admin.py/管理类)
		 search_fields = ['hname']
	
       		
"""
注意: strptime:字符串转换成时间对象
     strftime:时间对象转换成字符串
        
```

2.调整编辑页展示

```python
"""
1.显示字段:(在admin.py/管理类)
		  fields = ['btitle', 'bpub_date']
		  
2:分组显示
		fieldset=(
    ('高级', {
            'fields': ['bread', 'bcomment'],
            'classes': ('collapse',)  # 是否折叠显示
        }
    ),
     ('基本', {'fields': ['btitle', 'bpub_date']})
)

3.关联对象
	在admin.py/管理类,创建HeroInfoTabularInline类
		class HeroInfoTabularInline(admin.TabularInline):
    			model = HeroInfo
   			    extra = 1
	
	在admin.py/管理类
		class BookInfoAdmin(admin.ModelAdmin):
    		  inlines = [HeroInfoTabularInline]
	
"""
```

3.调整站点信息

```python
1.在admin.py中添加
        admin.site.site_title='美多商城1'
        admin.site.site_header='美多商城2'
        admin.site.index_title='欢迎使用美多商城3'
```

4.上传图片

```python
1.pip install Pillow
2.配置保存路径:MEDIR_ROOT =os.path.join(BASE_DIR,'static_files/media')
3.为模型类添加ImageField字段:
    image=modles.ImageFiled(upload_to='booktest',verbose_name='图片',null=True)
    
python manage.py makemigrations
python manage.py migrate

```

# 二. Django REST framework

## 1. DRF介绍

1.web应用模式

```python
"""
1.前后端不分离: 前端页面的效果都由后端控制,由后端渲染页面或重定向,前端与后端的耦合度很高
2.前后端分离: 后端仅返回前端所需的数据，不再渲染HTML页面,不再控制前端的效果,前端与后端的耦合度相对较低
"""
```

2.认识RESTful

```
API:Application Programming Interface 应用程序编程接口
REST:Representational State Transfer  表现层状态转化

```

3.RESTful设计方法

```python
1.域名:应该尽量将API部署在专用域名之下
	  如果确定API很简单，不会有进一步扩展，可以考虑放在主域名下
2.版本:应该将API的版本号放入URL
3.路径
		3.1:网址只能有名词不能有动词,名词往往与数据库的表名对应
		3.2: API中的名词应该使用复数。无论子资源或者所有资源
4.HTTP动词
 四个常见:get(select)/post(create)/put(update完全更新)/delete(delete)
 三个不常用:options/head/patch(update 部分更新)
 
 5.过滤信息（Filtering）
 	1.?limit=10：指定返回记录的数量
 	2.?offset=10：指定返回记录的开始位置
 	3.?page=2&per_page=100：指定第几页，以及每页的记录数。
 	4.?sortby=name&order=asc：指定返回结果按照哪个属性排序，以及排序顺序。
 6.状态码:
          2* : 成功,操作被成功接收并处理 
            	200:OK
                201:已创建。成功请求并创建了新的资源
                202:已接受。已经接受请求，但未处理完成
          3** :重定向,需要进一步的操作以完成请求 
            	300:
                301:永久性重定向
                302:临时性重定向
                    
          4** : 客户端错误，请求包含语法错误或无法完成请求 
                400:客户端请求的语法错误，服务器无法理解
                401:未认证
                403:权限被禁止

          5**: 服务器错误，服务器在处理请求的过程中发生了错误
                500:服务器内部错误，无法完成请求
                501:服务器不支持请求的功能，无法完成请求
                502:
                #200 
                请求成功
                #201 
                请求成功并创建新的资源
                #202
                已经接受请求,但为处理完成

                #301
                永久重定向
                #302
                临时重定向

                #400 
                客户端的请求语法错误,服务器无法理解
                #401
                请求要求用户的身份认证
                #403:
                用户的身份得到认证,但是权限是被禁止的,往往是因为在做post的请求时所提交的内容没有携带cookie所导致,需要把django的setting文件中的csrf的中间件注释掉django.middleware.csrf.CsrfViewMiddleware
                CSRF（Cross-site request forgery）跨站请求伪造
                #404:
                请求资源不存在

                #502:
                在运行项目时Nginx的配置出现问题
                网关在执行请求时,从服务器端得到一个无效的请求
 7.错误处理（Error handling）
 8. 返回结果
 9. 超媒体（Hypermedia API）
 
    
    


    
  
```

4.使用Django开发REST接口

```
使用runserver启动Django项目
```

5.明确REST接口开发的核心任务

```python
"""
三件事:
1.序列化:将模型类对象转换为响应的数据（如JSON格式 ）(instance参数)
2.操作数据库
3.反序列化:将请求的数据（如JSON格式）转换为模型类对象(data参数)

"""
```

6.Django REST framework 简介

```
1.提供了定义序列化器Serializer的方法
2.提供了丰富的类视图、Mixin扩展类
3.丰富的定制层级
4.多种身份认证和权限认证方式的支持
5.内置了限流系统
6.直观的 API web 界面
```



## 2.DRF工程搭建

1.环境安装与配置

```
1.pip install djangorestframework
2.在settings.py的INSTALLED_APPS中添加'rest_framework'。
```

2.DRF框架的使用

```python
"""
1. 创建序列化器
	class BookInfoSerializer(serializers.ModelSerializer):
    """图书数据序列化器"""
    class Meta:
        model = BookInfo #数据字段从模型类BookInfo参考生成
        fields = '__all__' #含模型类中的哪些字段
        
2. 编写视图
			class BookInfoViewSet(ModelViewSet):
    				queryset = BookInfo.objects.all() #查询数据时使用的查询集
    				serializer_class = BookInfoSerializer #指明该视图在进行序列化或反序列化时使用的序列化器
 
3.定义路由
		from rest_framework.routers import DefaultRouter
		urlpatterns = [
    ...
]

		router = DefaultRouter()  # 可以处理视图的路由器
		router.register(r'books', views.BookInfoViewSet)  # 向路由器中注册视图集
		urlpatterns += router.urls  # 将路由器中的所以路由信息追到到django的路由列表中
				
	
    				
"""
```



## 3. Serializer 序列化器

序列化器的作用：进行数据的校验/对数据对象进行转换

1. 定义序列化器

```python
"""
1.定义方法

"""
class BookInfo(models.Model):
    btitle = models.CharField(max_length=20, verbose_name='名称')
    bpub_date = models.DateField(verbose_name='发布日期', null=True)
    bread = models.IntegerField(default=0, verbose_name='阅读量')
    bcomment = models.IntegerField(default=0, verbose_name='评论量')
    image = models.ImageField(upload_to='booktest', verbose_name='图片', null=True)
 
class BookInfoSerializer(serializers.Serializer):
    """图书数据序列化器"""
    id = serializers.IntegerField(label='ID', read_only=True)
    btitle = serializers.CharField(label='名称', max_length=20)
    bpub_date = serializers.DateField(label='发布日期', required=False)
    bread = serializers.IntegerField(label='阅读量', required=False)
    bcomment = serializers.IntegerField(label='评论量', required=False)
    image = serializers.ImageField(label='图片', required=False)
    
    
"""
2.字段选项 
"""
1.read_only 序列化
2.write_only 反序列化
3.required 反序列化必须输入 默认True
4.default 反序列化使用的默认追


"""
3.创建Serializer对象
"""
1.用于序列化时 将模型类对象传入instance
2.用于反序列化时 将要被反序列化的数据传入data参数
3.通过context参数额外添加数据


```

2.序列化的使用

```python
"1.基本使用 "
    queryse = BookInfo.objects.all()
    serializer = BookInfoSerializer(queryse, many=True)
    serializer.data
    "注意:如果要被序列化的是包含多条数据的查询集QuerySet，可以通过添加many=True参数补充说明"
"2.关联对象_嵌套序列化"

  " 2.1PrimaryKeyRelatedField:序列化为关联对象的主键"
	hbook=serializers.PrimaryKeyRelatedField(label='图书')
    
  "2.2StringRelatedField:序列化为关联对象的字符串表示方式 "
    hbook=serializers.StringRelatedFiled(label='图书')
    
   "2.3HyperlinkedRelatedField:序列化为获取关联对象数据的接口链接"
	hbook=serializers.HyperLinkedRelatedField(label='图书',read_only=True,view_name='books-detail')
    """必须指明view_name参数，以便DRF根据视图名称寻找路由，进而拼接成完整UR"""
    
    "2.4 SlugRelatedField"
    hbook=serializers.SlugRelatedField(label='图书',read_only=True,slug_field='bpub_date')
    
    "2.5使用关联对象的序列"
    hbook=BookInfoSerializer()
    
    " 2.6重写to_representation方法"
    		class BookRelateField(serializers.RelatedField):
    				"""自定义用于处理图书的字段"""
    			def to_representation(self, value):
        				return 'Book: %d %s' % (value.id, value.btitle)
      hbook = BookRelateField(read_only=True)
    "many参数:关联的对象数据包含多个数据,只是在声明关联字段时，多补充一个many=True参数即可"
	
```

3.反序列化的使用

```python
反序列化的步骤:数据校验(is_valid(raise_exception=True),)--->数据的保存(validated_data获取数据)
    
    
"""
校验
"""
serializer = BookInfoSerializer(data=data)
serializer.is_valid(raise_exception=True)
serializer.errors 

 1.validate_<field_name>
 2.validate
 3.validators: UniqueValidator/UniqueTogetherValidation/
        
        
"""
保存:基于validated_data完成数据对象的创建，可以通过实现create()和update()两个方法来实现。
"""
		class BookInfoSerializer(serializers.Serializer):
1.create:
		  def create(self, validated_data):
        """新建"""
        return BookInfo.objects.create(**validated_data)
   		 
2.update:
    	def update(self, instance, validated_data):
        """更新，instance为要更新的对象实例"""
        instance.btitle = validated_data.get('btitle', instance.btitle)
         instance.save()
    	

```

4.模型类序列化器 ModelSerializer

| 类型                                         | 序列化                                                       | 反序列化                                                     | update/create                      |
| -------------------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ | ---------------------------------- |
| 序列化器(serializers.Serializers)            | id=serializers.IntegerField(label='ID', read_only=True)      | serializer=BookInfoSerializer(data=data) serializer.is_valid() |                                    |
| 模型类序列化器(serializers.ModelSerializers) | 基于模型类自动生成一系列字段   class Meta:                                             model = BookInfo                                 fields = '__ all __' | 基于模型类自动为Serializer生成validators，比如unique_together | 包含默认的create()和update()的实现 |



```python
"
1.定义
"
class BookInfoSerializer(serializers.ModelSerializer):
    """图书数据序列化器"""
    class Meta:
        model = BookInfo
        fields = '__all__'
"
2. 指定字段
"
1.fields来明确字段
2.exclude可以明确排除掉哪些字段
3.depth表明嵌套的层级数量
4.显示指明字段
5.read_only_fields指明只读字段

"
3. 添加额外参数
"
 extra_kwargs = {
            'bread': {'min_value': 0, 'required': True},
            'bcomment': {'min_value': 0, 'required': True},
        }
```

## 4.视图

1.Request 与 Response

| 方式     | REST framework                                               | Django                       |
| -------- | ------------------------------------------------------------ | ---------------------------- |
| Request  | Request(REST framework 提供了**Parser**解析器)               | HttpRequest                  |
|          | `request.data`                                               | `request.POST/request.FILES` |
|          | `request.query_params`                                       | `request.GET`                |
| Response | rest_framework.response.Response(REST framework提供了`Renderer` 渲染器，) | HttpResponse                 |
|          | `Response(data, status=None, template_name=None, headers=None, content_type=None)` |                              |

2.视图说明

View-----> APIView -----> GenericAPIView

| APIView                                                      | View |      |
| ------------------------------------------------------------ | ---- | ---- |
| 1.REST框架所有视图的基类                                     |      |      |
| 2.Request/Response                                           |      |      |
| 3.dispatch()之前会进行身份认证,权限检查,流量控制(身份认证:authentication_classes/权限检查:permission_classes/流量控制:throttle_classes) |      |      |

|       GenericAPIView       | 属性                                                         | 方法                                                         |
| :------------------------: | ------------------------------------------------------------ | ------------------------------------------------------------ |
| **通用(列表详情都可以用)** | query_set:查询集                                             | get_queryset:返回查询集                                      |
|                            | serializer_class:序列化器                                    | get_serializer_class:返回序列化器类                              get_serializer:返回序列化器对象                                                                                                    ***注意:***在提供序列化器对象的时候，REST framework会向对象的context属性补充三个数据：**request、format、view，这三个数据对象可以在定义序列化器时使用。** |
|        **详情视图**        | lookup_filed:查询单一数据库对象时使用的条件字段              | get_object:返回详情视图所需的模型类数据对象                           该方法会默认使用APIView提供的check_object_permissions:方法检查当前对象是否有权限被访问 |
|                            | lookup_url_kwarg:               查询单一数据时URL中的参数关键字名称 |                                                              |
|        **列表视图**        | pagination_class:分页控制类                                  |                                                              |
|                            | filter_backend:过滤控制后端                                  |                                                              |

常见的五中扩展类

| 扩展类             | 英文名             | 功能                                             |
| ------------------ | ------------------ | ------------------------------------------------ |
| **列表视图扩展类** | ListModelMixin     | 快速实现列表视图list方法会对数据进行过滤和分页。 |
| **创建视图扩展类** | CreateModelMixin   | 创建资源的视图 成功则返回201                     |
| **详情视图扩展类** | RetrieveModelMixin | 返回一个存在的数据对象                           |
| **更新视图扩展类** | UpdateModelMixin   | 更新一个存在的数据对象                           |
| **删除视图扩展类** | DestroyModelMixin  | 删除一个存在的数据对象                           |

子类视图

| 1.CreateAPIView | 2.ListAPIView | 3.RetireveAPIView | 4.UpdateAPIView | 5.DestoryAPIView | 6.RetrieveUpdateAPIView | 7.RetrieveUpdateDestoryAPIView |
| --------------- | ------------- | ----------------- | --------------- | ---------------- | ----------------------- | ------------------------------ |
| POST            | GET           | GET               | PUT/PATCH       |                  | GET/PUT/PATCH           | GET /PUT/PATCH/DELETE          |

##5.视图集ViewSet

1.定义

```python
1.ViewSet视图集实现动作 action 如 create() ,destroy(),update(),list(),retrieve()
2.设置路由时:
urlpatterns = [
    url(r'^books/$', BookInfoViewSet.as_view({'get':'list'}),
    url(r'^books/(?P<pk>\d+)/$', BookInfoViewSet.as_view({'get': 'retrieve'})
]
3.在视图集中，我们可以通过action对象属性来获取当前请求视图集时的action动作是哪个
  def get_serializer_class(self):
    if self.action == 'create':
        return OrderCommitSerializer
    else:
        return OrderDataSerializer
        
```

2.常用视图集父类

| 视图集父类           |                                                              |
| -------------------- | ------------------------------------------------------------ |
| ViewSet              | 继承自`APIView `  (身份认证、权限校验、流量管理)             |
| GenericViewSet       | 继承自`GenericAPIView`                                       |
| ModelViewSet         | 继承自`GenericAPIVIew` 五个常见的子类视图(增删改查)          |
| ReadOnlyModelViewSet | 继承自`GenericAPIVIew`，同时包括了ListModelMixin、RetrieveModelMixin。(两个查) |

3.视图集中定义附加的action动作

```python

from rest_framework.decorators import action
from rest_framework import  mixins
class AddressViewSet(mixins.CreateModelMixin, mixins.UpdateModelMixin, GenericViewSet):
    #methods: 该action支持的请求方式，列表传递
    #detail: 表示是action中要处理的是否是视图资源的对象(是否通过url路径获取主键)
    # put /addresses/pk/status/
    @action(methods=['put'], detail=True)
    def status(self, request, pk=None):
        """ 设置默认地址 """

        address = self.get_object()
        request.user.default_address = address
        request.user.save()
        return Response({'message': 'OK'}, status=status.HTTP_200_OK)

```



##6.路由Routers

​	**定义:通过SimpleRouter,DefaultRouter为视图集ViewSet快速实现路由信息**

1. 使用方法:

```python
#1.创建视图
from rest_framework import routers
router = routers.SimpleRouter()
router.register(r'books', BookInfoViewSet, base_name='book')

#2.添加路由
urlpatterns = [
    ...
]
urlpatterns += router.urls

#3.总路由中添加
urlpatterns = [
    ...
    url(r'^', include(router.urls))
]

```

```
   
   2. 视图集中包含附加action的
   
      ```python
      def get_queryset(self):
      	if self.action == 'list':
      		return Area.objects.filter(parent=None)
      	else:
      		return Area.objects.all()
```

​      

   3. 路由router形成URL的方式

## 7.分页

```python
#1.
REST_FRAMEWORK = {
    'DEFAULT_PAGINATION_CLASS':  'rest_framework.pagination.PageNumberPagination',
    'PAGE_SIZE': 100  # 每页数目
}

#2.
class LargeResultsSetPagination(PageNumberPagination):
    page_size = 1000 #每页数目
    page_size_query_param = 'page_size' #前端发送的页数关键字名，默认为"page"
    max_page_size = 10000 #前端最多能设置的每页数量

class BookDetailView(RetrieveAPIView):
    queryset = BookInfo.objects.all()
    serializer_class = BookInfoSerializer
    pagination_class = LargeResultsSetPagination

#如果在视图内关闭分页功能，只需在视图内设置
pagination_class = None
```



