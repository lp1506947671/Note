#### 1.Linux

**1.文件和目录**

| 1     | 2                                   |
| ----- | ----------------------------------- |
| /bin  | 可执行二进制文件的目录              |
| /boot | 放置 linux 系统启动时用到的一些文件 |
| /dev  | 存放linux系统下的设备文件           |
| /home | 系统默认的用户家目录                |
| /lib  | 系统使用的函数库的目录              |
| /usr  | 应用程序存放目录                    |
| /var  | 放置系统执行过程中经常变化的文件    |

**2.基本命令**

| 序号 | 命令     | 补充                                  | 注意                                                         |
| ---- | -------- | ------------------------------------- | ------------------------------------------------------------ |
| 1.   | ls-alh   | a:all(包括隐藏文件) l:list h:文件大小 | 如果要使通配符作为普通字符使用，可以在其前面加上转义字符,列如:`ls \*a` |
| 2.   | Ctrl + l | 清屏                                  |                                                              |
| 3.   | cd       | "-":上一次目录,"~":/home/用户目录,""  |                                                              |
| 4.   | rm-rf    | -r:递归删除 -f:强制删除               |                                                              |
| 5.   |          | -r:递归复制                           |                                                              |

**3.高级命令**

| 序号 | 命令                                                         | 作用                                                         |
| ---- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 1    | ls > test.txt和 ls >>test.txt                                | 命令执行结果输出重定向某个文件: "覆盖原来的内容" 和'追加到文件尾部' |
| 2    | cat test.txt test2.txt >test3.txt                            | **查看或者合并文件内容**                                     |
| 3    | more demo.py                                                 | 分屏显示                                                     |
| 4    | `ls -lh |more `                                              | 一个命令的输出可以通过管道做为另一个命令的输入。             |
| 5    | ln                                                           | 建立链接文件 /ln 源文件 链接文件/ln -s 源文件 链接文件       |
|      | ln demo.txt demo_soft_link.txt                               | 软链接：软链接不占用磁盘空间，源文件删除则软链接失效。eg:(软连接就是快捷方式) |
|      | ln -s demo.txt  demo_solid_link(                             | 硬链接：硬链接只能链接普通文件，不能链接目录   硬链接是相同的内容,不同的文件) |
| 6    | grep -niv 正则表达式 文本名                                  | -n:number  -i:忽略大小写 -v:取反                             |
| 7    | find 路径 -name 文件名的正则表达式                                                                    find  路径 -size  +文件的最大值 -size 文件的最小值                                      find   路径  -perm  777 | 查找文件                                                     |
| 8    | tar -cvf  demo.tar                                           | c:打包文件 v:显示进度 f:文件后缀一定是.tar                   |
|      | tar -xvf   demo.tar                                          | x:解包文件                                                   |
|      | tar -zcvf  demo.tar.gz                                       | z:压缩                                                       |
|      | tar -zxvf  demo.tar.gz  -C  home/                            | 解压到指定路径下                                             |
| 10   | chmod                                                        | [ u:user/g:group/o:other/a :all]                             |
|      | chmod u=rw ,g=x,o=r demo.txt                                 | [+:增加,-:减少,=:设定]                                       |
|      |                                                              | [rwx] [4,2,1]  (7,6,5,2,1)                                   |
| 11   | which                                                        | 查看命令位置                                                 |
| 12   | whoami                                                       | 查看当前用户                                                 |
| 13   | who                                                          | 查看所有的登录用户                                           |

**4.远程登录和远程拷贝**

```shell
1.scp -r 目标用户名@目标主机IP地址：/所要复制的文件的绝对路径  /目标文件的绝对路径
eg;scp -r root@192.168.17.1: /home/Jason/demo.py  /home/Jason2/
```

**5.编辑器vim**

| 模式              | 进入    | 命令                                   |                                    |
| ----------------- | ------- | -------------------------------------- | ---------------------------------- |
| 插入模式          | i       |                                        |                                    |
|                   |         |                                        |                                    |
| 命令模式          | ESC     | u                                      | 一步一步撤销                       |
|                   |         | Ctr-r                                  | 反撤销                             |
| 编辑模式/末行模式 | shift+; | -wq! w:存盘 q:退出 !:强制   /word 搜索 |                                    |
|                   |         | :%s/abc/123/g                          | 将当前文件中的所有abc替换成123     |
|                   |         | :1, 10s/abc/123/g                      | 将第一行至第10行之间的abc替换成123 |
|                   |         | G                                      | 跳转到当前文档末尾行的首字符       |
|                   |         | g                                      | 跳转到当前文档首行的首字符         |
|                   |         | ^                                      | 跳转到当前文档光标所在行的首字符   |
|                   |         | $                                      | 跳转到当前文档光标所在行的末尾字符 |

```shell
#常用的基本命令
ps -aux | grep 响应的进程id #查看所有进程 
# ps:查看系统进程和线程 a:all u:特定的user

top  #查看cpu的占有率

netstat -nlup #查看端口的占用情况和网络链接情况
# a:all u:udp t:tcp p:程序名 l:listen n:将别名转为数字
tail -n 100 log.txt #查看日志文件末尾的100行
tail -f log.txt #持续输出新增的内容
apt  #Ubuntu的软件管理器
```

**补充:**

```
linux常见命令：
	文件相关
		创建和删除
			touch	创建一个普通文件
			mkdir	创建一个目录文件
			ln		创建一个链接文件
		
		查看和搜索
			echo 	将命令后面的内容，输出到当前屏幕
			zcat	查看压缩包文件内容
		
		传输和压缩
			wget	下载文件
			tar		文件的压缩和解压缩
			unzip	解压文件
		
			
	用户相关
		useradd		创建用户
		passwd		给用户创建密码或更改密码
		su -		切换用户
		
	权限相关
		chmod		更改文件的操作权限
		chown		更改文件的用户属性
	
	网络相关
		free		查看当前主机的内存信息
		netstat 	查看当前系统开启的端口信息
		lsof		查看进程相关信息
		
	计算相关：
		$((算数表达式))
		let 算数表达式
		
	其他命令：
		sed			行编辑工具
		awk			文档编辑工具
		
```

#### 2.网络

**1.私网IP地址**

|             私网             |
| :--------------------------: |
|   10.0.0.0～10.255.255.255   |
|  172.16.0.0～172.31.255.255  |
| 192.168.0.0～192.168.255.255 |

**2.常见端口**

| 常见端口   | Telnet | ftp   | http | smtp | dns  | tftp | snmp |
| ---------- | ------ | ----- | ---- | ---- | ---- | ---- | ---- |
| 2^16=65536 | 23     | 20/21 | 80   | 25   | 53   | 69   | 161  |

```shell
netstat -ntulp |grep 8000:#查看程序所使用的端口号
netstat -app | grep 5000
lsof -i:8000 #查看端口号对应的应用程序
    
#注意:
-t (tcp) 仅显示tcp相关选项
-u (udp)仅显示udp相关选项
-n 拒绝显示别名，能显示数字的全部转化为数字
-l 仅列出在Listen(监听)的服务状态
-p 显示建立相关链接的程序名
```

**3.udp和tcp**

1.简述TCP和UDP的区别以及优缺点?

| 协议 | TCP(Transmission Control Protocol) | UDP                   |
| ---- | ---------------------------------- | --------------------- |
| 1    | 协议面向连接的                     | 面向无连接            |
| 2    | 基于字节流的                       | 数据报文模式          |
| 3    | 安全可靠                           |                       |
| 4    | TCP对系统资源要求较多              | UDP对系统资源要求较少 |
| 5    | 有序/去重/阻塞/流量控制            |                       |

2.udp

```python
import socket
def send_msg(udp_socket):
    dest_ip = input("\n请输入对方的ip地址:")
    dest_port = int(input("\n请输入对方的port:"))
    msg = input("\n请输入要发送的数据:")
    udp_socket.sendto(msg.encode("utf-8"), (dest_ip, dest_port))
    
def recv_msg(udp_socket):    
    
    recv_data=s.recvfrom(1024)
    recv_msg = recv_msg[0].decode("utf-8")
    recv_ip = recv_msg[1]
    print(">>>%s:%s" % (str(recv_ip), recv_msg))

def main():
    udp_socket = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
    udp_socket.bind(("", 7890))
     while True:
            print("="*30)
            op_num = input("请输入要操作的功能序号(1:发送消息2:接收消息):")
            if op_num == "1":
                send_msg(udp_socket)
            elif op_num == "2":
                recv_msg(udp_socket)
            else:
                print("输入有误，请重新输入...")
```

4.tcp

![三次挥手和四次握手](D:\Dpan\Document\笔记\note\招聘\面试题\picture\三次挥手和四次握手.png)

三次握手

```
第一次握手：Client将标志位SYN置为1，随机产生一个值seq(sequence Number)=x，并将该数据包发送给Server，Client进入SYN_SENT状态，等待Server确认。 
第二次握手：Server收到数据包后由标志位SYN=1知道Client请求建立连接，Server将标志位SYN置为1，ACk=x+1，随机产生一个值seq=y，并将该数据包发送给Client以确认连接请求，Server进入SYN_RCVD状态。 
第三次握手：Client收到确认后，检查ack是否为x+1，如果正确则将标志位ack=y+1，并将该数据包发送给Server，Server检查ACK是否为y+1，如果正确则连接建立成功，Client和Server进入ESTABLISHED状态，完成三次握手，随后Client与Server之间可以开始传输数据了。
```

四次挥手

```python
1客户端将FIN标为1,seq为x的报文给服务端，告诉服务器不在向他发送消息
2服务端收到信息后，回复ACK为y+1的报文给客户端告知确认断开连接
3服务端将FIN标为1,seq为y的报文给客户端端，告诉客户端不在向他发送消息
2客户端收到信息后，回复ACK为y+1的报文给服务端确认断开连接
```

客户端:

```python
import socket 
tcp_client_socket=socket(AF_INIT,SOCKT_STRAEM)
server_ip,server_port=input("请输入服务器IP,服务器端口号:")
tcp_client_socket.connect((server_ip,server_port))

send_data=input("请输入要发送的数据:")
tcp_client_socket.send(send_data.encode("gbk"))

rcv_data=tcp_client_socket.recv(1024)
print('接收到的数据:'recvdata.decode('gbk'))

tcp_client_socket.close()
```

服务端:

```python
import socket

tcp_server_socket=socket(AF_INET,SOCKET_STRAEM)
address=('',7788)
tcp_server_socket.bind(address)

tcp_server_socket.listen(128)
client_socket,clientAddr=tcp_server_socket.accept()

recv_data=client_socket.recv(1024)
print('接收到的数据:'recv_data.decode('gbk'))

client_socket.send("thank you!".encode('gbk'))

client_socket.close()
```

长连接/短连接

```python
长连接:适合操作频繁,点对点的通讯,如数据库的连接
短连接:web网站的http服务连接
```

七层和四层

```
OSI:应,表,会,传.网,数,物
TCP:应      运,网,网络接口层
```

#### 3.多任务



