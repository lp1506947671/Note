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

**3.注意事项**

main.py

```python
from recv_msg import *
from handle_msg import *


def main():
    # 1. 接收数据
    recv_msg()
    # 2. 测试是否接收完毕
    test_recv_data()
    # 3. 判断如果处理完成，则接收其它数据
    recv_msg_next()
    # 4. 处理数据
    handle_data()
    # 5. 测试是否处理完毕
    test_handle_data()
    # 6. 判断如果处理完成，则接收其它数据
    recv_msg_next()


if __name__ == "__main__":
    main()
```

recv_msg.py

```python
from common import RECV_DATA_LIST
from common import HANDLE_FLAG
# import common


def recv_msg():
    """模拟接收到数据，然后添加到common模块中的列表中"""
    print("--->recv_msg")
    for i in range(5):
        RECV_DATA_LIST.append(i)


def test_recv_data():
    """测试接收到的数据"""
    print("--->test_recv_data")
    print(RECV_DATA_LIST)


def recv_msg_next():
    """已经处理完成后，再接收另外的其他数据"""
    print("--->recv_msg_next")
    if HANDLE_FLAG:
    # if common.HANDLE_FLAG:
        print("------发现之前的数据已经处理完成，这里进行接收其他的数据(模拟过程...)----")
    else:
        print("------发现之前的数据未处理完，等待中....------")
```

handle_msg.py

```python
from common import RECV_DATA_LIST
from common import HANDLE_FLAG
# import common


def handle_data():
    """模拟处理recv_msg模块接收的数据"""
    print("--->handle_data")
    for i in RECV_DATA_LIST:
        print(i)

    # 既然处理完成了，那么将变量HANDLE_FLAG设置为True，意味着处理完成
    global HANDLE_FLAG
    HANDLE_FLAG = True
    # common.HANDLE_FLAG = True


def test_handle_data():
    """测试处理是否完成，变量是否设置为True"""
    print("--->test_handle_data")
    if HANDLE_FLAG:
    # if common.HANDLE_FLAG:
        print("=====已经处理完成====")
    else:
        print("=====未处理完成====")
```

commom.py

```python
RECV_DATA_LIST = list()  # 用来存储数据
HANDLE_FLAG = False  # 用来标记是否处理完成
```

**HANDLE_FLAG为什么没有被修改为Ture?**

在handle_msg.py文件中，HANDLE_FLAG的使用方式为 from common import HANDLE_FLAG ，该导入方式相当于是在handle_msg.py 生成一个叫做HANDLE_FLAG 的变量，并且这个变量指向的是common.py里面HANDLE_FLAG的值（False），当在执行 HANDLE_FLAG =True 这行代码时 其实是将变量handle_msg.py中的 HANDLE_FLAG 重新指向了一个新的值为True（这个过程可以理解为赋值的过程，即修改的是变量的指向而不是变量指向的值），此时common里面的HANDLE_FLAG 值依然是False， 所以在recv_msg.py使用 from common import HANDLE_FLAG 导入时，HANDLE_FLAG这个变量仍指向False。

**如何解决这个问题？**
在handle_msg.py文件中，将HANDLE_FLAG的使用方式改为 import common，再使用common.HANDLE_FLAG 调用即可解决。具体原理可以理解为：在handle_msg.py中生成一个叫做common的变量，这个变量指向的是common文件，而common.HANDLE_FLAG可以理解为指向common文件中的 HANDLE_FLAG 变量名而不是这个变量名的值，所以在handle.py执行 common.HANDLE_FLAG=Ture 让它的指向从False变成了True后，在recv_msg.py使用 common.HANDLE_FLAG可以获取到Ture这个值。

[1]https://www.cnblogs.com/testlearn/p/12364724.html