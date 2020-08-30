# 1.shell随笔

## 1.shell的调试

```shell
cat -n test.sh #检查脚本语法错误
/bin/bash -x test.sh # 进入跟踪方式，显示所执行的每一条命令
sh -c 'a=1;b=2;let c=$a+$b;echo "c=$c"' #"string" 从strings中读取命令
```

## 2.shell的前后台

```shell
fg(前台执行)  frontground
bg(后台执行) background   &  daemon

按Ctrl+z可以挂起前台运行的程序
挂起的程序可以用fg恢复到前台，或者用bg恢复到后台
一般命令在前台执行（fg）
在命令后面加上&，它会在后台执行（bg），并将特殊的环境变量$!设置为该任务的进程ID。这时shell可以并发执行其他命令

链接:https://blog.csdn.net/weixin_42167759/article/details/80700719?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1.nonecase&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1.nonecase
```

## 3.vi命令(编辑模式)

```shell
set num #显示行号
dd #删除光标这一行
dG #删除所有
u  #撤销上一步
/字符 #查找的字符 
```

## 4.shell数组

```shell
# 数组的定义
array_name=(a b)
# 传递所有的数组值
函数名 ${array_name[@]}
# 查看所有数组的索引
${!array_name[@]}
# 查看
${#array_name[@]}
```

## 5.shell数值

```shell
# 数值的定义
i=0 #不要有空格
# 数值的计算
let i=i+1
```

## 6 awk命令

```shell
awk -F [:|] # 正则匹配分割符号
```

## 7.字符串的分割

```shell
${parameter/parttern/string} 
用string来替换第一个匹配的pattern
${parameter//pattern/string} 
用string来替换parameter变量中所有匹配的pattern

string="hello,shell,split,test"  
array=(${string//,/ })  
 
for var in ${array[@]}
do
   echo $var
done 
```

