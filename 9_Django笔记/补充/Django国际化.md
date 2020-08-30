## Django国际化

所谓的国际化，是指使用不同语言的用户在访问同一个网站页面时能够看到符合其自身语言的文本页面。

setting.py

```python
# system time zone.
TIME_ZONE = 'UTC'
# Language code for this installation. All choices can be found here:
# http://www.i18nguy.com/unicode/language-identifiers.html
LANGUAGE_CODE = 'zh_hans'

# Internationalization
# https://docs.djangoproject.com/en/1.11/topics/i18n/
USE_I18N = True
USE_L10N = True
USE_TZ = True

LANGUAGES = (
    ('en', ('English')),
    ('zh-hans', ('中文简体')),
    ('zh-hant', ('中文繁體')),
)

# 翻译文件所在目录，需要手工创建
LOCALE_PATHS = (
    os.path.join(BASE_DIR, 'locale'),
)

MIDDLEWARE = [
    'django.middleware.security.SecurityMiddleware',
    'django.contrib.sessions.middleware.SessionMiddleware',
    'django.middleware.common.CommonMiddleware',
    'django.middleware.csrf.CsrfViewMiddleware',
    'django.contrib.auth.middleware.AuthenticationMiddleware',
    'django.contrib.messages.middleware.MessageMiddleware',
    'django.middleware.clickjacking.XFrameOptionsMiddleware',
    'django.middleware.locale.LocaleMiddleware',   # 添加此行
]123456789101112131415161718192021222324252627282930313233
```

### 一、在视图中标识需要翻译的文本

在视图中和HTML模板中都可以标识要翻译的文本。在视图中，通过_()或ugettext()函数，指定某个变量需要翻译，如下所示：

```python
from django.utils.translation import ugettext as _
from django.http import HttpResponse

def my_view(request):
    output = _("Welcome to my site.")
    return HttpResponse(output)123456
```

### 二、在模板中表示需要翻译的文本

在模版文件中，要标识一个待翻译的文本，需要使用{% trans %}模板标签，但首先你要在模版的顶部加载{% load i18n %}。比如：

```python
{% load i18n %}
<title>{% trans "This is the title." %}</title>12
```

### 三、blocktrans模板标签

与{% trans %}模板标签不同，blocktrans标签允许你通过使用占位符来标记由文字和可变内容组成的复杂句子进行翻译，如下例所示：

```python
{% blocktrans %}This string will have {{ value }} inside.{% endblocktrans %}1
```

### 四、本地化

建立语言文件是通过django-admin makemessages命令完成的。
在项目的根目录下，也就是包含manage.py的目录下，运行下面的命令：

```
python manage.py makemessages -l zh_hans
python manage.py makemessages -l en
python manage.py makemessages -l zh_hant123
```

#### 注意：在Windows下，需要提前安装GNU gettext工具！[安装链接](https://mlocati.github.io/articles/gettext-iconv-windows.html)

每个.po文件首先包含一小部分元数据，例如翻译维护者的联系信息，但文件的大部分是翻译对照：被翻译字符串和特定语言的实际翻译文本之间的简单映射。
例如，有一个像下面这样的待翻译字符串：
`_("Welcome to my site.")`
在.po文件中将包含一条下面样子的条目：
`#: path/to/python/module.py:23msgid "Welcome to my site."msgstr ""`
这三行内容各自代表下面的意思：

- 第一行通过注释表达该条要翻译的字符串在视图或模版中的位置；
- msgid：要翻译的字符串。不要修改它。
- msgstr：翻译后的文本。一开始它是空的，需要翻译人员逐条填写。
  这是一个文本文件，需要专业的翻译人员将所有的msgstr空白‘填写’齐全。如果你的项目比较大，这可能是个磨人的事。

### 五、编译语言文件

```python
django-admin compilemessages
```

Django将自动搜索所有的.po文件，将它们都翻译成.mo文件