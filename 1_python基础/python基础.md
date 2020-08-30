#### 1. eval

```python
描述:eval() 函数用来执行一个字符串表达式，并返回表达式的值
举例:eval('pow(2,2)')
```

#### 2.format

```python
>>>"{} {}".format("hello", "world")    # 不设置指定位置，按默认顺序
'hello world'
 
>>> "{1} {0} {1}".format("hello", "world")  # 设置指定位置
'world hello world'

print("网站名：{name}, 地址 {url}".format(name="菜鸟教程", url="www.runoob.com"))
 
# 通过字典设置参数
site = {"name": "菜鸟教程", "url": "www.runoob.com"}
print("网站名：{name}, 地址 {url}".format(**site))
 
# 通过列表索引设置参数
my_list = ['菜鸟教程', 'www.runoob.com']
print("网站名：{0[0]}, 地址 {0[1]}".format(my_list))  

注意:setdefault和get的区别
    setdefault对于不存在的键,会显示其默认值,并插入到原先的字典中
    get对于不存在的键,会显示其默认值,但不会影响原先的字典
```

#### 3.setdefault:

```python
描述:根据键读取相应的值,如果键不存在于字典中，将会添加键并将值设为默认值
举例:
dict = {'runoob': '菜鸟教程', 'google': 'Google 搜索'}
 
print "Value : %s" %  dict.setdefault('runoob', None)
print "Value : %s" %  dict.setdefault('Taobao', '淘宝')
#输出:
Value : 菜鸟教程
Value : 淘宝
```

#### 4.斜线与反斜线的区别
```python
web,Linux,除号:斜线/
windows:反斜线\
```
#### 5.loads,dumps,load,dumps
```python
json.loads(json字符)
json.dumps(字典)

json.load(open('json文件','r'))
json.dump(字典,open('json文件','w'))

注意:json字符只支持单引号
```
#### 6.正则
```python
Python中字符串前面加上 r 表示原生字符串，

与大多数编程语言相同，正则表达式里使用"\"作为转义字符，这就可能造成反斜杠困扰。假如你需要匹配文本中的字符"\"，那么使用编程语言表示的正则表达式里将需要4个反斜杠"\\"：前两个和后两个分别用于在编程语言里转义成反斜杠，转换成两个反斜杠后再在正则表达式里转义成一个反斜杠。
```
#### 7. secrets.SystemRandom
```python
import secrets

# 用secrets模块获取systemRandom类实例
random_digit = secrets.SystemRandom()
r1 = random_digit.randint(0, 100)
print("r1",r1)
# 指定范围内按照指定间隔生成随机数
r2 = random_digit.randrange(4,40,4)
print("r2",r2)
# 在指定范围内生成特定的随机数
a= [6,12,24,21]
r3= random_digit.choice(a)
print("r3",r3)
# 在指定的列表中的生成一个随机样本
r4=random_digit.sample(a,2)
print("r4",r4)
# 使用secrets生成一个随机浮点数
r5=random_digit.uniform(2.5,25.5)
print("r5",r4)
```

#### 8.datetime的使用
```Python
datetime.datetime.now()+datetime.timedelta(days=-1).strftime("%Y%m%d")
```

#### 9.os.walk()

```shell
F:\aaa

|--------1.txt
|--------2.txt
|--------3.txt
|--------4

         |-------5.txt

         |-------6.txt

         |-------7.txt
```

```python
import os 
def main():
	fileDir = "F:" + os.sep + "aaa"		# os.sep指代windows下的"\"Linux写的"/"
	for root, dirs, files in os.walk(fileDir):
		print(root)	#第一个为起始路径
		print(dirs) #第二个为起始路径下的文件夹
		print(files) #第三个是起始路径下的文件
	os.system("pause") #防止小黑窗一闪而过
 
if __name__ == '__main__':
	main()
 
```

```
# 输出
# F:\aaa
# ['4']
# ['1.txt', '2.txt', '3.txt']
# F:\aaa\4
# []
# ['5.txt', '6.txt', '7.txt']
```

#### 10.os.path.split()与split()

```python
#os.path.splitext()将文件名和扩展名分开
fname,fename=os.path.splitext('/home/ubuntu/python_coding/split_func/split_function.py')
print 'fname is:',fname
print 'fename is:',fename
#输出为：
# fname is:/home/ubuntu/python_coding/split_func/split_function
#fename is:.py
 
#os.path.split（）返回文件的路径和文件名
dirname,filename=os.path.split('/home/ubuntu/python_coding/split_func/split_function.py')
print dirname
print filename
#输出为：
# /home/ubuntu/python_coding/split_func
#split_function.py
 
#split（）函数
#string.split(str="", num=string.count(str))[n]
#str - - 分隔符，默认为所有的空字符，包括空格、换行(\n)、制表符(\t)等。
#num - - 分割次数。
#[n] - - 选取的第n个分片
string = "hello.world.python"
print string.split('.')#输出为：['hello', 'world', 'python']
print(string.split('.',1))#输出为：['hello', 'world.python']
print(string.split('.',1)[0])#输出为：hello
print(string.split('.',1)[1])#输出为：world.python
string2="hello<python.world>and<c++>end"
print(string2.split("<",2)[2].split(">")[0])#输出为：c+

```

#### 11.os.chir()

```python
#!/usr/bin/python
# -*- coding: UTF-8 -*-

import os, sys
path = "/tmp"

# 查看当前工作目录
retval = os.getcwd()
print "当前工作目录为 %s" % retval

# 修改当前工作目录
os.chdir( path )

# 查看修改后的工作目录
retval = os.getcwd()

print "目录修改成功 %s" % retval
```

#### 12.ziplife

```python
import zipfile
z= zipfile.ZipFile(r'..\test_file.zip','rw')
z.extract('test_file/a.txt', path=None, pwd=None)
#'C:\\Users\\BruceWong\\Documents\\test_file\\a.txt'
for f in z.namelist():
    print(f)
    
z.extractall()
os.listdir(r'test_file') 
# ['a.txt', 'b.txt', 'c.txt', 'd.txt']

z.write('out.log')

z.close()
```

#### 13.traceback

```python
#test_traceback.py
import traceback
try:
    1/0
except Exception,e:
    traceback.print_exc() #则直接给打印出来
    raceback.format_exc() #返回字符串
```

```python
File "test_traceback.py", line 3, in <module>
告知程序错在哪里
```

#### 14.os.path.dirname,os.path.abspath(

```python
#C:demo\demo.py
import os
a=os.path.dirname(__file__) # C:demo/  返还目录
b=os.path.abspath(__file__) # C:demo\demo.py 返还绝对路径
```

#### 15.quote/urlencode

```python
from urllib.parse import quote urlencode
#特殊符号：汉字、&、=等特殊符号编码为%xx 
KEYWORD = '苹果'
url1= 'https://s.taobao.com/search?q=' + quote(KEYWORD)
print(url)
params2 = {
            'name':"王二",
            'extra':"/",
            'special':'&',
            'equal':'='
        }
url1 = base_url + urlencode(params2)
print(urlencode(params2))
print(url2)



#url标准符号：数字字母 
url2= 'https://s.taobao.com/search?q=' + quote(KEYWORD)
print(url) 
params2 = {
            "value":"english",
            'page':1
        }
url1 = base_url + urlencode(params1)
print(urlencode(params1))
print(url1)

```

#### 16.Configparser

```ini
#config.ini
[logging]
level = 20
path =
server =

[mysql]
host=127.0.0.1
port=3306
user=root
password=123456

```

```python 
#demo.py 
import configparser
from until.file_system import get_init_path

conf = configparser.ConfigParser()
file_path = get_init_path()
print('file_path :',file_path)
conf.read(file_path)

sections = conf.sections()
print('获取配置文件所有的section', sections)

options = conf.options('mysql')
print('获取指定section下所有option', options)


items = conf.items('mysql')
print('获取指定section下所有的键值对', items)


value = conf.get('mysql', 'host')
print('获取指定的section下的option', type(value), value)
```

```python 
#输出
file_path : /Users/xxx/Desktop/xxx/xxx/xxx.ini
获取配置文件所有的section ['logging', 'mysql']
获取指定section下所有option ['host', 'port', 'user', 'password']
获取指定section下所有的键值对 [('host', '127.0.0.1'), ('port', '3306'), ('user', 'root'), ('password', '123456')]
获取指定的section下的option <class 'str'> 127.0.0.1
```

