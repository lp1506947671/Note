### Git

#### 1.Git的简介

​		Git 是目前世界上最先进的分布式版本控制系统

#### 2.Git的组成

 		Git区由工作区,暂存区和仓库区三者组成从而与外部服务器连接

#### 3.Git单人本地仓库操作

```python
问题:git出现 *** Please tell me who you are. Run...... 错误
0:git clone
1. git init
2.git config user.name "XiaoPawnYe"
3.git config user.email "xiaopawnye@163.com"
4.git add .
5.git commit -m "some init msg"
6.git push
7.git checkout master (切换到主分支)
```



| 工作区                        | 暂存区                          | 仓库区            |                                                       |
| ----------------------------- | ------------------------------- | ----------------- | ----------------------------------------------------- |
| **将工作区文件添加到暂存区:** | **将暂存区文件提交到仓库区:**   | **参看文件状态:** | **标签操作:**                                         |
| git add .                     | git commit -m '版本描述'        | git status        | git tag -a 标签名 -m '标签描述'                       |
| **撤销修改:**                 | **回退版本:**                   | **查看历史版本**  | git push origin 标签名                                |
| git checkout 文件名           | git reset --hard HEAD^^         | git log           | git tag -d 标签名(删除本地标签)                       |
|                               | git reset --hard HEAD~2         | git reflog        | git push origin --delete tag 标签名(删除远程仓库标签) |
| **工作区与版本库对比:**       | **版本库与版本库对比:**         | **代码更迭:**     | **分支操作:**                                         |
| git diff HEAD -- login.py     | git diff HEAD HEAD^ -- login.py | git clone         | git branch                                            |
| **误删处理:**                 | **确定删除处理:**               | git pull          | git checkout -b 分支名(创建并切换分支)                |
| rm 文件名                     | rm 文件名                       | git push          | git push -u origin 分支名(提交分支)                   |
| git checkout -- 文件名        | git rm 文件名                   |                   | git checkout master                                   |
|                               | git commit -m '删除描述'        |                   | git merge dev                                         |

### 虚拟环境

####1.安装虚拟环境

```shell
sudo pip install virtualenv
sudo pip install virtualenvwrapper

#安装完虚拟环境后，如果提示找不到mkvirtualenv命令，须配置环境变量
mkdir $HOME/.virtualenvs#创建虚拟环境存放文件夹
vim  ~/.bashrc#打开~/.bashrc文件添加如下命令
export WORKON_HOME=$HOME/.virtualenvs
source /usr/local/bin/virtualenvwrapper.sh
source ~/.bashrc#运行.bashrc文件
```

#### 2.创建虚拟环境

```shell 
mkvirtualenv 虚拟环境名称  #创建虚拟环境
mkvirtualenv -p python3 虚拟环境名称
workon 虚拟环境名称   #选择虚拟环境
deactivate			#退出虚拟环境
rmvirtualenv 虚拟环境名称
```

```shell
#requirements文件
pip freeze #查看虚拟环境中安装的包 
pip freeze > requirements.txt
pip install -r requirements.txt
```

###Ngnix+Gunicorn

####1.Ngnix具体操作

```shell
sudo apt-get install nginx#安装
/etc/init.d/nginx start #启动
/etc/init.d/nginx stop  #停止
```

####2.Gunicorn具体操作

```shell
pip install gunicorn #安装
gunicorn -w 2 -b 127.0.0.1:5000 运行文件名称:Flask程序实例名

# -w: 表示进程（worker） 
#-b：表示绑定ip地址和端口号（bind）
#Gunicorn相关配置：https://blog.csdn.net/y472360651/article/details/78538188
```

```shell
scp -r 本地文件路径 root@127.0.0.1:远程保存路径
```

###状态保持

浏览器请求服务器是通过http请求进行连接的,http 是一种无状态协议

**无状态**：指一次用户请求时，浏览器、服务器无法知道之前这个用户做过什么，每次请求都是一次新的请求

**无状态原因**：浏览器与服务器是使用 socket 套接字进行通信的，服务器将请求结果返回给浏览器之后，会关闭当前的 socket 连接，而且服务器也会在处理页面完毕之后销毁页面对象

**cookie的应用**:

- 最典型的应用是判定注册用户是否已经登录网站，用户可能会得到提示，是否在下一次进入此网站时保留用户信息以便简化登录手续，这些都是Cookie的功用。
- 网站的广告推送，经常遇到访问某个网站时，会弹出小窗口，展示我们曾经在购物网站上看过的商品信息。
- 购物车，用户可能会在一段时间内在同一家网站的不同页面中选择不同的商品，这些信息都会写入Cookie，以便在最后付款时提取信息。

###模板

####1.定义:

视图函数有两个作用：处理业务逻辑和返回响应内容

在大型应用中，把业务逻辑和表现内容放在一起，会增加代码的复杂度和维护成本,模板的作用即是承担视图函数的另一个作用，即返回响应内容。

渲染:使用真实值替换变量，再返回最终得到的字符串的过程

#### 2.模板语言

**1.基本使用**

| 1     | 2          | 3                                                            |
| ----- | ---------- | ------------------------------------------------------------ |
| {{}}  | 变量名     | <h1>{{ post.title }}</h1>                                    |
| {%%}  | 控制代码块 | {% if user %}<br/>    {{ user }}<br/>{% else %}<br/>    hello! |
| {# #} | 注释       | {# {{ name }} #}                                             |

**2.过滤器**

| 常见内建过滤器                                       |                                                     |
| ---------------------------------------------------- | --------------------------------------------------- |
| safe：禁用转义                                       | <p>{{ '<em>hello</em>' \| safe }}</p>               |
| capitalize：把变量值的首字母转成大写，其余字母转小写 | <p>{{ 'hello' \| capitalize }}</p>                  |
| lower：把值转成小写                                  | <p>{{ 'HELLO' \| lower }}</p>                       |
| upper：把值转成大写                                  | <p>{{ 'hello' \| upper }}</p>                       |
| title：把值中的每个单词的首字母都转成大写            | <p>{{ 'hello' \| title }}</p>                       |
| reverse：字符串反转                                  | <p>{{ 'olleh' \| reverse }}</p>                     |
| format：格式化输出                                   | <p>{{ '%s is %d' \| format('name',17) }}</p>        |
| striptags：渲染之前把值中所有的HTML标签都删掉        | `<p>{{ '<em>hello</em>'|striptags }}</p>`           |
| truncate: 字符串截断                                 | <p>{{ 'hello every one' \| truncate(9)}}</p>        |
| first：取第一个元素                                  | <p>{{ [1,2,3,4,5,6] \| first }}</p>                 |
| last：取最后一个元素                                 | <p>{{ [1,2,3,4,5,6] \| last }}</p>                  |
| length：获取列表长度                                 | <p>{{ [1,2,3,4,5,6] \| length }}</p>                |
| sum：列表求和                                        | <p>{{ [1,2,3,4,5,6] \| sum }}</p>                   |
| sort：列表排序                                       | <p>{{ [6,2,3,1,5,4] \| sort }}</p>                  |
| 语句块过滤                                           | {% filter upper %}     #一大堆文字# {% endfilter %} |

```python
#自定义过滤器
def do_listreverse(li):
    # 通过原列表创建一个新列表
    temp_li = list(li)
    # 将新列表进行返转
    temp_li.reverse()
    return temp_li

app.add_template_filter(do_listreverse,'lireverse')

<br/> my_array 原内容：{{ my_array }}
<br/> my_array 反转：{{ my_array | lireverse }}


#my_array 原内容：[3, 4, 2, 1, 7, 9] 
#my_array 反转：[9, 7, 1, 2, 4, 3]
```

**控制代码块**

```html
-  if /else / endif

{%if user.is_logged_in() %}
    <a href='/logout'>Logout</a>
{% else %}
    <a href='/login'>Login</a>
{% endif %}

- for / endfor

{% for post in posts %}
    <div>
        <h1>{{ post.title }}</h1>
        <p>{{ post.text | safe }}</p>
    </div>
{% endfor %}
```

