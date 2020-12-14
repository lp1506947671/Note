### 1.GIL

​	又称全局解释器锁,当多线程运行遇到IO阻塞会自动释放GIL锁

### 2.深拷贝和浅拷贝

**1.浅拷贝和深拷贝的区别**

|          | 浅拷贝                                                       | 深拷贝                                   |
| -------- | ------------------------------------------------------------ | ---------------------------------------- |
| **使用** | b=copy.copy(a)                                               | b=copy.deepcopy(a)                       |
| **定义** | 浅拷贝是对于一个对象的顶层拷贝                               | 深拷贝是对于一个对象所有层次的拷贝(递归) |
| **注意** | copy.copy对于可变类型，会进行浅拷贝 <br>copy.copy对于不可变类型，不会拷贝，仅仅是指向 |                                          |

**2.浅拷贝对于可变类型**

```python
alist = [1, 2, 3, ["a", "b"]]
b = copy.copy(alist)
print("alist:", alist)
print("b", b)
alist.append(5)
print("alist:", alist)
print("b", b)
alist[3].append("ccc")
print("alist:", alist)
print("b", b)
```

**3浅拷贝对于不可变类型**

```python
import copy
a = (11, 22, 33)
b = copy.copy(a)
print(id(a))
print(id(b))
```

### 3.私有化

|   名称   |                              _x                              |                             __xx                             |
| :------: | :----------------------------------------------------------: | :----------------------------------------------------------: |
| **定义** |                   半保护:私有化属性或方法                    |   全保护:避免与子类中的属性命名冲突(名字重整所以访问不到)    |
| **访问** |               类对象和子类可以访问,也可以继承                |             类对象和子类不可以访问,也不可以继承              |
| **注意** |               from somemodule import *禁止导入               |               from somemodule import *禁止导入               |
|          | 当父类中有单下划线变量时,即能在创建对象进行初始化时改变父类的单下划线变量<br/>,也能通过子类的方法来改变单下划线变量 | 当父类中有双下划线变量时,继承父类的子类只能在创建对象进行初始化时才能赋值给父类的双下划线线变量<br/>,不能通过子类的方法来改变双下划线变量 |
|          |                                                              | 双下划线前缀会导致Python解释器重写属性名称，以避免子类中的命名冲突。这叫作名称修饰（name mangling）,假如类名是Test，那么__ foo在对象属性列表中会变为_Test _ _foo，解释器这么做的目的，是防止变量在子类中被重写,所以访问的格式为:对象._类名.__私有成员变量 |

**获取双下划线变量的方法**

```python
class A:
    def __init__(self):
        self.__name = "text"

a = A()
print(a.__name)  # 报错
print(a._A__name)  # 输出text(对象._类名.__私有成员变量)
```

### 4.import导入模块

**1.程序执行时添加新的模块路径**

```python
sys.path.append('/home/itcast/xxx')
sys.path.insert(0, '/home/itcast/xxx')  # 可以确保先搜索这个路径
```

**2.重新导入模块**

```python
import reload_test
reload_test.test()
from imp import reload
reload(reload_test)
```

