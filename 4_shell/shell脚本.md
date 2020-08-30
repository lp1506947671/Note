###  shell脚本

#### 第1章 shell基础

##### 1.1 运维&shell[了解]	

**1.运维基础**

```python
运维定位:技术岗位
工作范围:项目全生命周期			
运维的目标： 自动化运维 --> 方式（工具+脚本）
脚本：shell脚本 /其他脚本					
```

**2.shell简介**

```shell
shell定义:用于程序和操作系统之间的命令解释器
shell分类:图形shell/命令行式shell
				   windows系统中的cmd.exe 
				   Linux中的bash/sh/dash/rbash
```

```shell
shell查看方式：
#手动执行shell
		echo $SHELL		  # 系统默认shell
		cat /etc/shells   # 系统支持的shell种类
#脚本执行shell
		mkdir/data/scripts -p
		cd/data/scipts/
		ls
		vim test.sh
		cat test.sh
		/bin/bash test.sh  #执行shell脚本
#注释
		单行注释 #
		多行注释 :<<! 需要注释的字符 !
```



#####1.2 shell脚本

**1.创建脚本**

```shell
#更改Linux主机用户名
hostnamectl set-hostname Jason
exec /bin/bash
#清屏
crtl + l
```

**2.执行脚本**

2.1执行方式

```shell
#1.适用于任何情况
/bin/bash 脚本的绝对路径
#2.
ll 脚本文件名 #查看是否有可执行权限
chomd +x 脚本文件名 #给脚本添加可执行权限
./脚本的文件名
#3.加载环境变量使用
source 脚本文件名
```

2.2开发规范

```shell
echo $LANG #查看脚本所使用的编码语言 
		   #echo 将命令后面的内容，输出到当前屏幕

1.脚本命令 --有意义
2.脚本首行 --命令解释器
3 注释信息 --尽量全面
4.脚本执行 -- /bin/bash 脚本文件
5.注释 -- 成对内容一次性写完,防止遗漏
6.#! /bin/bash  首行必须是
```

##### 1.3变量

**1.变量是什么**

```shell
1.变量的组成: 变量名(不变的)=变量值(可变的)
2.变量的类型: 
	本地变量:手工定义,作用范围小
        普通变量: 手工|默认方式定义 ,作用范围大的变量
        命令变量: bash中自带一些参数变量
    全局变量:查看 env /export 全局变量
    		删除 unset
    内置变量:
    
    

```

**2.本地变量**

```shell
#普通变量
变量名=变量值   #不能包含特殊值
变量值='变量值' #原字符输出
变量="变量值"   # 先对变量值进行解析然后进行拼接 推荐使用


#命令变量:是一种提前执行的符号
变量名=$(命令)
变量名=`命令`
```

**3.全局变量**

```shell
#定义:当前系统下所有环境变量都生效的变量
#创建全局变量
export name1=Jason1
name2=Jason2
expoort name2
```

**4.变量的查看和删除**

```shell
#1.变量的查看
echo $变量名 #第二推荐
echo "$变量名"
echo ${变量名}
echo "${变量名}" #建议使用

#2.变量的删除
unset 变量名

```

**5,内置变量**

```shell
$0 获取当前执行的shell执行的脚本文件名
$n 获取当前执行的shell脚本的第n个参数值
$# 获取当前shell命令行中参数的总个数
$? 获取上一个指令的返回值



示例

#! /bin/bash
echo "我的脚本文件名称:$0"
echo "第一个位置参数:$1"
echo "当前脚本传入的参数总数量:$#"

echo $? #获取上一个指令的返回值
```

**6.默认值**

```shell
# 精确截取
name="qwertyuiop"
${name:0:5}#从0开始选取5个
${file:0-6:3}#
# 默认值
#! /bin/bash
# 套餐选择演示
a="$1"
echo "你选择的套餐是:套餐${a:-1}"优先选择传入值,没有传入值则选择默认值
echo "你选择的套餐是:套餐${a+22}":强制转换
```

#### 第2章核心知识的应用

##### 2.1 表达式

**1.测试语句**

```shell
#应用场景: 判断条件是否成立
test 条件表达式 #1.
[ 条件表达式 ]  #2.优先推荐 [] 和条件表达式应该用空格隔开
```

**2.条件表达式**

```shell
#1.逻辑表达式
#&&
[ 1 = 1 ] && echo='条件不成立'
#||
[ 1 = 2] || echo='条件不成立'
echo $?

-a 　 　　　　　 与 
-o　　　　　　　 或 
!　　　　　　　　非

#2.文件表达式
[ -f 文件名 ] && echo "条件成立"
[ -d 目录 ] && echo "条件成立"
[ -x 文件名 ]&& echo"条件成立"

#3.数值表达式
n1 -gt n2
n1 -lt n2
n1 -eq n2
n1 -ne n2

#4.字符串表达式
[ str1 == str2 ]
[ str1 != str2 ]

```

**3.计算表达式**

```shell
$(( 计算表达式 )) #1.
let 计算表达式    #2.
注意:表达式必须是一个整体，中间不能出现空格等特殊字符
```

**4.数组的操作**

```shell
#定义数组
array_name=(a,b,c)
array_name[0]=a

#查看数组内容
echo ${array_name[1]}
echo ${array_name[@]}
echo ${array_name[*]}

#查看数组索引
echo ${!array_name[1]}
echo ${!array_name[*]}
echo ${!array_name[@]}

#获取长度
echo ${#array_name[1]}
echo ${#array_name[@]}
echo ${#array_name[*]}


#增删改查
#增
array_name[index]=value
#删
单个元素	unset array_name[index]
整体		  unset array_name
#改
整体 	   array_name[index]=value
模拟替换  ${array3[index]/原内容/修改后内容}
注意：    因为是模拟，所以内容不会变
#查
整体	echo ${array3[index]}
部分	echo ${array3[index]:起始位置:获取长度}
```

##### 2.2Linux常见命令符号

```shell
#2.2.1 重定向
			作用：
				数据的传输
			种类；
				> 		覆盖形式
				>> 		追加的形式
			示例：
				echo nihao > file.txt
				ls
				cat file.txt 
				echo nihao1 > file.txt
				cat file.txt 
				echo nihao2 >> file.txt 
				cat file.txt 
				echo nihao2 >> file.txt 
				cat file.txt	
				
#2.2.2 管道符 |	
			作用：信息的传输
			特点：从左向右
			示例：
				ls | grep test
				env | grep SHELL
				
#2.2.3 其他符号
		1.后台展示:&
                sleep 6 &
                ps aux | grep sleep
		2.信息符号:
        		1 表示正确输出的信息
				2 表示错误输出的信息
				2>&1 代表所有输出的信息
                                    /bin/bash cuowu.sh  1>> ceshi-ok 2>> ceshi-nook #1.
                                    /bin/bash cuowu.sh >> ceshi-all 2>&1			#2.
			
		3.特殊设备:	/dev/null是linux下的一个设备文件,这个文件类似于一个垃圾桶，特点是：容量无限大
                   python manage.py runserver >> /dev/null 2>&1 &
```

##### 2.3 简单流程控制

学习目标
了解 3种if语句和case的格式
掌握 双分支if语句和case语句的应用
掌握 3种循环语句的格式和应用
掌握 4种循环退出语

**1.if 语句**

```shell
#单分支
if [ 条件 ]
then 
  指令
fi

#双分支
if [条件]
then 
   指令1
else
   指令2
fi

#多分支
if [条件1]
then
	指令1
elif [条件2]
then
    指令2
else
	指令3
fi

```

```shell
#!/bin/bash
# 多if语句的服务使用场景
arg="$1"
if [ "${arg}" == "start" ]
then
echo "服务启动中..."
elif [ "${arg}" == "stop" ]
then
echo "服务关闭中..."
elif [ "${arg}" == "restart" ]
then
echo "服务重启中..."
else
echo "脚本 $0 的使用方式: /bin/bash $0 [ start|stop|restart ]"
fi




注意事项: 
	1.[]和== 用空格与包含的符号隔离开来
```

**2.case语句**

```shell
#! /bin/bash
arg="$1" #不要出现空格
case "${arg}" in
"start")
	echo "服务启动中..."
	;;
"stop")
	echo "服务停止中..."
"restart")
	echo "服务重启中..."
*)
	echo"脚本 $0的使用方式:/bin/bash $0[start|stop|restart]"
esac

关键点：	
		1 框架： case-esac
		2 选项： )
		3 语句： ;;

		* 两侧不允许出现双引号
		* 不允许是第一个匹配选项
		case语句是linux下所有服务默认的流程控制语句
```

**3.循环语句**

```shell
#for
#! /bin/bash
for i in $(ls /data/script)
do
	echo"/data/scripts的文件有${i}"
done

#while 满足条件一直执行
a=1
while [ "${a}" -lt 5]
do
	echo "${a}"
	let a=a+1
	sleep 1
done


#util 不满足条件则一直执行
b=1
until [ "${b}" -eq 5 ]
do
	echo "${a}"
	a=$((b+1))
	sleep 0.5
done



注意:
	{1..5}生成1,5之间的数组
```

4.循环退出

```shell
break 跳出所有循环
break n 跳出第n个循环(由内向外)
continue 跳出当前循环
exit 退出程序
```

```shell
1.break示例
# !/bin/bash
while :
do
	echo -n "echo -n "输入你的数字,最好在1~5:" #echo -n 表示不换行输出
	read aNum
	case "${aNum}" in
        1|2|3|4|5)
        echo "你的数字是 $aNum!"
        ;;
        *)
        echo "你选择的数字没在1~5,退出!"
        break
        ;;
	esac
	
done
```

```shell
2.break n:跳出第n层循环
#/bin/bash

for var1 in {1...5}
do
	for var2 in {0...4}
	do
		if [ $var1 -eq 2 -a $var2 -eq 0 ]
		then
			break 2
		else
			echo "$var1 $var2"
		fi
	done
	
done
```

```shell
3.continue: 跳出当前循环

#! /bin/bash
while :  #死循环
do
	echo -n "输入你的数字,最好在1~5:" #echo -n 表示不换行输出
	read aNum
	case $aNum in
	 1|2|3|4|5|)
	 	echo "你的数字是 $aNum!"
	 ;;
	 *)
	 	echo "你选择的数字没在 1~5,退出!"
		continue
	 ;;
	esac
	
	
done

```

```shell
4.exit 退出当前程序

#! /bin/bash
for var1 in{ 1...5 }
do
for var2 in {0...4}
do
    if[$var1 -eq 3 -a $var2 -eq 2]
    then
    	exit
    else
    	echo "$var1 $var2"
    fi

```



##### 2.4 复杂流程控制

**1.函数的基础知识**

```shell
#1.简单函数的定义和调用
#!/bin/bash
dayin(){
		 echo "hello my name is aaa"
		}
dayin

#2.传参函数定义和调用
dayin(){
		  # 合理的使用本地变量，避免参数传参歧义
		  echo "hello my name is $1" 
		}
dayin aaa

#3.脚本传参函数调用
dayin()
{
echo "hello my name is $1"

}
dayin $1



#4.脚本传参函数调用(生产用)
canshu="$1" #定义本地变量
dayin()
{
echo "hello my name is $1"

}
dayin "${canshu}"
```

##### 2.5常见命令的详解

**1.grep命令详解**

```shell
# grep: 文本搜索命令

grep [参数] [关键字] <文件名>
参数详解
-c：只输出匹配行的计数。
-n：显示匹配行及行号。
-v：显示不包含匹配文本的所有行。
-r   递归查找
-i   忽略大小写敏感
egrep  基于正则符号进行精确过滤

touch find.txt
ihao aaa
nihao AAA
NiHao bbb
nihao CCC
```

| 作用 | 参数  | 含义                                      | 示例                    |
| ---- | ----- | ----------------------------------------- | ----------------------- |
| grep | -c    | 输出匹配aaa的个数                         | grep -c aaa find.txt    |
|      | -n    | 输出匹配aaa的匹配内容并显示其行号         | grep -n aaa find.txt    |
|      | -v    | 输出与aaa不相匹配的所有内容               | grep -v find.txt        |
|      | -r    | 输出当前文件下文件内容包含 AAA的内容      | grep -nr AAA /data/*    |
|      | -i    | 输出匹配aaa和AAA的内容                    | grep -nri AAA /data/*   |
|      | egrep | 用正则表达式搜索以N开头的内容并显示其行号 | egrep -n  '^N' grep.txt |



**2.sed命令详解**

```shell
sed 行文件编辑工具。编辑文件时以行为单位。
#命令格式：
		sed [参数] '<匹配条件> [动作]' [文件名]
		sed -n '3p' sed.txt
		sed -n '2,3p' sed.txt
#参数详解：
-n 取消默认全文输出 (默认情况下先进行全文输出,然后再输出匹配的内容)
-i 表示对文件进行编辑

#匹配条件：
匹配条件分为两种：数值和内容 #内容匹配符号： / # @ ! 

#动作
p 打印
s 替换 '行号s#原内容#新内容#列号' #g:指代替换所有 默认替换查找到的第一个
a 在匹配到的内容下一行增加内容
i 在匹配到的内容当前行增加内容
d 删除匹配到的内容

#示例
touch sed.txt
nihao sed1 sed2 sed3
nihao sed4 sed5 sed6
nihao sed7 sed8 sed9

```

| 作用 | 参数/动作                | 含义                      |                                |
| ---- | ------------------------ | ------------------------- | ------------------------------ |
| 查看 | -n                       | 查看第2行和第3行的内容    | sed -n '2,3p' sed.txt          |
| 替换 | -i                       | 替换首每行的第1个sed为SED | sed -i 's#sed#SED#'            |
|      | s :替换                  | 替换全部sed为des          | sed -i 's#sed#SED#g'  sed.txt  |
|      |                          | 替换每行的第二个SED为sed  | sed -i 's#SED#sed#2' sed.txt   |
|      |                          | 替换第3行的第2个SED为sed  | sed -i '3s#SED#sed#2'          |
| 增加 | a:在指定行号后面增加内容 | 在第2行后面增加zengjia-2  | sed -i '2a\zengjia-2' sed.txt  |
|      |                          | 指定1~3每行都增加内容     | sed -i '1,3\tongshi-2' sed.txt |
|      | i:在指定行号前面增加内容 | 指定1~3每行都增加内容     | sed -i '1,3i\insert-1' sed.txt |
| 删除 | d:删除                   | 删除3,5行之间的内容       | sed -i '3,5d'  sed.txt         |

**3.awk命令详解**

```shell
#定义
	awk:文档编辑工具，它不仅能以行为单位还能以列为单位处理文件
#格式
	awk [参数] '[ 动作]' [文件名]

#常见参数：
-F 指定列的分隔符
-f 调用脚本
-v 定义变量

#常见动作：
	print:显示内容   $0 显示当前行所有内容 $n 显示当前行的第n列内容
	BEGIN{ 命令 } 	初始代码块，主要和变量相关
	END{ 命令 }       结束代码块,主要信息和输出有关
	/pattern/{命令}  	 匹配、执行代码块

#内置变量
	FILENAME 当前输入文件的文件名，该变量是只读的
	NR 指定显示行的行号
	NF 输出 最后一列的内容
	OFS 输出格式的列分隔符，缺省是空格
	FS 输入文件的列分融符，缺省是连续的空格和Tab

#示例
touch awk.txt
nihao awk1 awk2 awk3
nihao awk4 awk5 awk6
```



| 参数                               | 动作                  | 含义                                    | 示例                                                         |
| ---------------------------------- | --------------------- | --------------------------------------- | ------------------------------------------------------------ |
|                                    | /文本中的内容/        | 过滤包含nihao的内容                     | awk '/nihao/' awk.txt                                        |
|                                    | {print}               | 打印第1列的内容                         | awk '{print $1}' awk.txt                                     |
|                                    | NR:表示行号           | 打印第1列的内容,并显示其行号            | awk '{print NR,$1}' awk.txt                                  |
|                                    |                       | 打印第1行第1和第3列内容                 | `awk 'NR==1 {print $1,$3}' awk.txt`                          |
| -F:指定隔离分隔符，查看内容        |                       | 指定隔离分隔符，查看内容                | `awk -F ':' '{print $1,$7}' linshi.txt`                      |
| -f参数 加载脚本，按格式输出信息    |                       | 加载脚本,按格式化输出                   | awk -f filename awk.txt                                      |
| -v参数 传入多值                    |                       | 传入两个值                              | echo \|awk '{print v1,v2}' v1=100 v2=200                     |
| OFS 输出格式的列分隔符，缺省是空格 | BEGIN{ 命令 }         | 设置显示分隔符，显示内容                | awk 'BEGIN{OFS=":"} {print NR,$0} awk.txt'                   |
|                                    | END{ 命令 }           | 设置输出样式                            | awk -F : 'BEGIN {print "---开始了----"}'  {print NR,$0} END {print "-----结 束了"--}' awk.txt |
|                                    | { if语句 print语句 }  | 列出当前目录中大于200字节的信息         | `ls -lawk '{if ($5>=200) print "文件名:" $9 "\t" "文件大小": $5 "B"}'` |
|                                    | (( 条件1 && 条件2 ))  | 列出当前目录中大于500字节的普通文件信息 | `ls -l | awk '{ if (($5>=500 && /^-/)) print "文件名称: " $9 "\t" "文件大 小:" $5 "B" }'` |
| for                                | { for语句 print语句 } | 按顺序输出所有的内容                    | echo 'abcdefg'  \| awk -F '' '{ for(i=1;i<=NF;i++) print $i }' |
|                                    | for( 循环表达式 )     |                                         | echo 'abcdefg'\|awk -F '' '{ for(i=NF;i<=1;i++) print $i }'  |

**4.find 命令 **

```shell
#定义
 	 find:文件的查找
#格式
	find [路径] [参数] [关键字] [动作]
#参数
-name 			 文件名
-tpye  			 文件类型 (f:普通文件 d:目录)
-mtime(-|+)n 	 查找规定时间内修改的文件  
-prune   		 取反
-perm(/|-)       执行权限

#动作：
    -print 				默认属性，打印信息
    -ls 				列出文件详细属性
    -exec 命令 \;		对过滤的文件进行下一步操作
    注意：
    {} 就相当于过滤出来的文件
    操作类似于 | xargs 命令


```

| 作用     | 参数                            | 含义                                     | 示例                                                         |
| -------- | ------------------------------- | ---------------------------------------- | ------------------------------------------------------------ |
| 文件查找 | -name                           | 在当前系统中查找一个叫awk的文件          | find /home/admin-1/name  "awk.txt"                           |
|          | -type                           | 在当前系统中查找文件类型为普通文件的文件 | find /tmp -type f                                            |
|          | -mtime                          | 根目录下查找5日以内更改的文件            | find / -mtime -5                                             |
|          |                                 | 在/tmp/目录下查找3日以前更改的文件       | find /tmp/ -mtime +3                                         |
|          | -prune -o print(取反 或者 输出) | 在目录下查找不包含backup子目录           | find /data -path "/data/backup" -prune -o print              |
|          |                                 | 忽略多个文件夹                           | find .\ (-path "./backup" -o -path "./backup2" -prue -o print) |
|          | -perm                           |                                          | find /etc -perm -640 -ls                                     |
|          | -exec                           | 对查找到的文件进行改名                   | find ./-perm -002 -exec mv{} {}.old \                        |
|          |                                 | 对查找到的文件进行删除                   | find . -name .svn \|xargs rm -rf                             |
|          |                                 | 查找文件中大于3M的文件                   | find .size +300k -exec ls -ld {};                            |

#### 4.补充

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

#### 

