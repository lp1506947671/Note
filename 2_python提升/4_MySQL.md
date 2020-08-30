##Mysql

###1.MySQL的基本使用

####1.MySQL的安装

```python
sudo apt-get install mysql-server #安装mysql

ps ajx |grep mysql #ps 是Linux中查看进程的意思 ps aux 查看所有的进程
				   #-a 显示终端上所有进程 -x显示没有控制终端的进程 

sudo service mysql start
sudo service mysql stop
sudo service mysql restart

sudo apt-get install mysql-client
mysql -u root -pmysql      #连接数据库
```

####2.数据的完整性

**注意:使用数据类型的原则是：够用就行，尽量使用取值范围小的，而不用大的，这样可以更多的节省存储空间**

| 数据类型                   | 特别说明                                                     |
| -------------------------- | ------------------------------------------------------------ |
| 整数 int                   |                                                              |
| 小数 decimal               | decimal(5,2)表示共存5位数，小数占2位                         |
| 字符串 varchar char        | 如char(3)，如果填充'ab'时会补一个空格为'ab '/如varchar(3)，填充'ab'时就会存储'ab' |
| 日期   datetime ,date,time |                                                              |
| 枚举   enum                |                                                              |

| 约束            | 含义                                                         |
| --------------- | ------------------------------------------------------------ |
| 主键primary key | 物理上存储的顺序                                             |
| 非空not null    | 此字段不允许填写空值                                         |
| 默认default     | 当不填写此值时会使用默认值，如果填写时以填写为准             |
| 外键foreign key | 对关系字段进行约束，当为关系字段填写值时，会到关联的表中查询此值是否存在，如果存在则填写成功，如果不存在则填写失败并抛出异常 |

####3.数据库设计

```python
#第一范式：强调的是列的原子性，即每一列不能够再分成其他几列

#第二范式：建立在第一范式的基础上，非主键必须完全依赖主键,不能只依赖主键的一部分
       例如: 如果订单表中主键是（OrderID，ProductID）,Quantity完全依赖于主键,而 UnitPrice，ProductName只依赖于ProductID
        
#第三范式：建立在第二范式的基础上，另外非主键列必须直接依赖于主键，不能存在传递依赖,即不能存在：非主键列 A 依赖于非主键列 B，非主键列 B 依赖于主键的情况。
		例如:如果订单表中主键是 OrderID,CustomerID完全依赖于主键,而CustomerName直接依赖的是 CustomerID而不是直接依赖于主键
```



### 2.MySQL-查询

####1.数据库的操作

```mysql
-- 1.创建数据库 create database 数据库名 charset=utf8;   
        create database python charset=utf8

    -- 2.删除数据库 drop database 数据库名;
        drop database python

    -- 3.使用数据库 use 数据库名;
        use python

    -- 4.查看所有数据库
        show databases
```

####2.数据表的操作

```mysql
-- 1.增
        -- 创建学生表
        create table students(
            id int unsigned primary key auto_increment not null,
            name varchar(20) default '',
            age tinyint unsigned default 0,
            height decimal(5,2),
            gender enum('男','女','人妖','保密'),
            cls_id int unsigned default 0
        )

        -- 添加字段 add : alter table 表名 add 列名 类型;
             alter table  students add birthday datetime

-- 2.删除
        -- 删除表 : drop table 表名;
           drop table students
        -- 删除字段: alter table 表名 drop 列名;
           alter table student drop birthday

-- 3.修改表
       -- 修改表-修改字段-重命名版 change : alter table 表名 change 原名 新名 类型及约束;
            alter  table students change birthday birth datetime not null 
       -- 修改表-修改字段-不重命名版 modify: alter table 表名 modify 列名 类型及约束;
            alter  table  students  modify  birth  date not null
            
-- 4.查看
        -- 查看当前数据库中所有表
        show tables
        --  查看表结构
        desc 表名;
        -- 查看表的创建语句
        show create table classes
```

####3:数据的操

```mysql
-- 增加
        -- 全列插入：值的顺序与表中字段的顺序对应  ;
            -- insert into 表名 values(...) 
            insert into  students values (0,’郭靖‘,1,'蒙古','2016-1-2')

        -- 全列多行插入：值的顺序与给出的列顺序对应 ;
            -- insert into 表名 values(...),(...)...; 
                insert into classes valuse  (0,'python1'),(0,'python2')
            -- insert into 表名(列1,...) values(值1,...),(值1,...)...;
                insert into  students(name) values ('杨康'),('杨过'),('小龙女')
                

        -- 部分列插入：值的顺序与给出的列顺序对应 insert into 表名(列1,...) values(值1,...) values
            insert into  students(name,hometown,birthday) values ('黄蓉','桃花岛','2016-3-2')

        
-- 删除
        -- 物理删除 delete from 表名 where 条件 
        delete  from students where id=5

        -- 逻辑删除，本质就是修改操作 isdelete=1 id=1
        update  student set isdelete=1 where id=1

-- 修改
        -- update 表名 set 列1=值1,列2=值2... where 条件 id=5
        update student set gender=0,hometown='北京' where id =5

-- 查询
        -- 查询所有列 select * from 表名;
        select * from classes;
        -- 查询指定列 select 列1,列2,... from 表名;
        select id,name from classes;
```

####4.数据的查询

```mysql
-- 总结:
        select distinct * from 表名  where ... group by ... having ... order by... limit start,count
```

```mysql
-- 准备
    -- 使用 as 给字段起别名 /可以通过 as 给表起别名
    -- 消除重复行 select distinct 列1,... from 表名


-- 条件查询 
        -- 1.比较运算符 
        -- 2.逻辑运算符 
        -- 3.模糊查询 like:%表示任意多个任意字符/_表示一个任意字符
            查询姓黄的学生
            select * from students  where name like '黄%'
        -- 范围查询 in/between ... and ...
            查询编号是1或3或8的学生
            select * from students where id and 3 or 8         
            查询编号为3至8的学生
            select * from students where id between 3 and 8 
            
        -- 空判断is not nul注意：null与''是不同的空值是不占用空间的,mysql中的NULL其实是占用空间的，
            查询没有填写身高的学生
            select * from students where height is not null
        
注意: 优先级由高到低的顺序为：小括号，not，比较运算符，逻辑运算符**

-- 排序
	查询未删除男生信息，按学号降序
    select * from students where gender=1 and is_delete=0 order by id desc

	查询未删除学生信息，按名称升序
    select * from students where is_delete=0 order by name ;

	显示所有的学生信息，先按照年龄从大-->小排序，当年龄相同时 按照身高从高-->矮排序
    select * from  students order by age desc, height desc; 


-- 聚合函数 : 总数count /最大值max(列)/最小值min(列)/求和 sum(列)/平均值avg(列) 
      		查询学生总数 count(*) 计算总行数
			select count（*） from students

			查询女生的编号最大值
			select max（id） from students where gender=2

			查询未删除的学生最小编号
			select min（id） from students where is_delete=0
			
 			查询男生的总年龄
			select sum(age) from students where gender=1;

			查询男生的平均年龄
			select sum(age)/count(*) from students where  gender=1;
			
 			查询未删除女生的编号平均值
			select avg(id) from  students  where  is_delete=0 and gender=2                   
            
-- 分组
     -- group by + group_concat()
       	查询每种性别的都有那些人
        select gender,group_concat(name) from students group by gender
     -- group by + 集合函数
         分别统计性别为男/女的人年龄平均值
         select  gender,avg(age) from students group by gender
     -- group by + having  
         select gender,count(*) from students  group by gender having count(*)>2
     -- group by + with rollup 
     	分别统各自性别人数并将人数大于2的进行显示
      	select  gender,count(*) from studnents group by  gender with rollup
      	
      	
      	注意:
      		1.group_concat()用来显示分组后每一组的某字段的值的集合
      		2.having作用和where一样，但having只能用于group by
      		3.with rollup的作用是：在最后新增一行，来记录当前列里所有记录的总和
      		

-- 分页
        查询前3行男生信息
		select * from students where gender=1 limit 0,3;

		已知:每页显示m条数据，当前显示第n页,求第n页数据
		select * from students is_delete=0 limit(n-1)*m,m;
            
-- 连接查询
            -- 使用内连接查询班级表与学生表
                select  * from students as s inner join classes as c on s.cls_id=c.id 

            -- 使用左连接查询班级表与学生表
                select *  from students as s left join classes as c on s.cls_id=c.id 

            -- 使用右连接查询班级表与学生表
                select *  from students as s right join classes as c on s.cls_id=c.id 

-- 自关联 :
		表中的某一列，关联了这个表中的另外一列，但是它们的业务逻辑含义是不一样的，城市信息的pid引用的是省信息的id,因为省没有所属的省份，所以可以填写为null,城市所属的省份pid，填写省所对应的编号id
	 查询一共有多少个省
	  select count(*) from areas where pid is null;
	 查询省的名称为“山西省”的所有城市
	  select city.* from areas as city inner join as province on city.pid =province.aid where province.atitle='山西省'
	  查询市的名称为“广州市”的所有区县
select dis.* from areas as dis inner join areas as city on city.aid =dis.pid
where city.atitle='广州市';


-- 子查询
    -- 标量子查询 
        查询班级学生平均年龄 /查询大于平均年龄的学生
            select * from student  where  age > (select avg(age) from student)
        
    -- 列表子查询
       找出学生表中所有的班级 id/找出班级表中对应的名字
            select name from student where id in (select cls_id from student )
    -- 行级子查询
       查找班级年龄最大,身高最高的学生
   		select * from student where (height,age)=(select max(height),max(age) from students);

```

#### 5.数据备份和恢复

```mysql
-- 备份
        mysqldump –uroot –p 数据库名 > python.sql;
-- 恢复
        mysql -uroot –p 新数据库名 < python.sql
-- 自关联
-- 从sql文件中导入数据
         source areas.sql:
```

####6.视图

| 1          |                              2                               |
| ---------- | :----------------------------------------------------------: |
| 定义:      |              一条SELECT语句执行后返回的结果集。              |
| 创建视图   |             create view 视图名称 as select语句;              |
| 查看视图   |                         show tables;                         |
| 删除视图   |                     drop view 视图名称;                      |
| 视图的作用 | 提高了重用性，就像一个函数/对数据库重构，却不影响程序的运行/提高了安全性能，可以对不同的用户/让数据更加清晰 |

####7.事务

| 1                      |                              2                               |
| ---------------------- | :----------------------------------------------------------: |
| **定义**               |                       它是一个操作序列                       |
| **四大特性(简称ACID)** | 原子性(Atomicity)/一致性(Consistency)/隔离性(Isolation)/持久性(Durability)  **注意:**一致性指的是事务的执行的前后数据的完整性保持一致,如转账业务，无论事务执行成功与否，参与转账的两个账号余额之和应该是不变的。 |
| **应用场景**           |                      订单系统,银行系统                       |
| **事务命令**           | ```   0.use meiduo;1.begin; #开启事务 2.commit ;#提交事务 3.rollback #回滚事务 |

####8:索引

| 1        |                              2                               |                              3                               |
| -------- | :----------------------------------------------------------: | :----------------------------------------------------------: |
| 定义:    | 索引是一种特殊的文件 它们包含着对数据表里所有记录的引用指针。(InnoDB数据表上的索引是表空间的一个组成部分) |            例:为表title_index的title列创建索引：             |
| 创建索引 |        create index 索引名称 on 表名(字段名称(长度))         |      create index title_index on test_index(title(10));      |
| 删除索引 |                 drop index 索引名称 on 表名;                 |             drop index title_index on test_index             |
| 查看索引 |                    show index from 表名;                     |                  show index from test_index                  |
| 注意     |            建立太多的索引将会影响更新和插入的速度            | 1.对于一个经常需要更新和插入的表/2.对于比较小的表，排序的开销不会很大，都没有必要建立另外的索引。 |

#### 9.账户管理

1.查看所有用户

```mysql
1.desc user;#查看user表的结构 所有用户及权限信息存储在mysql数据库的user表中
2.select host,user,authentication_string from user; #Host表示允许访问的主机/User表示用户名/authentication_string表示密码，为加密后的值
```

2.创建账户、授权

```mysql
需要使用实例级账户登录后操作，以root为例
常用权限主要包括：create、alter、drop、insert、update、delete、select
如果分配所有权限，可以使用all privileges
```

示例1:

```mysql
-- grant 权限列表 on 数据库 to '用户名'@'访问主机' identified by '密码';
grant select on jing_dong.* to 'laowang'@'localhost' identified by '123456';
注意: jing_dong.* 可以操作python数据库的所有表 /访问主机通常使用百分号%表示此账户可以使用任何ip的主机登录访问此数据库

-- 查看用户哪些权限
   show grants for laowang@localhost;
   
-- 退出登录
   quit
   
-- 使用laowang账号登录
   mysql -ulaowang -p
```

示例2:

```mysql
-- 创建一个laoli的账号，密码为12345678，可以任意电脑进行链接访问, 并且对jing_dong数据库中的所有表拥有所有权限
grant all privileges onjing_dong.* to "laoli"@"%" identified by "12345678"
```

3.修改权限

```mysql
-- grant 权限名称 on 数据库 to 账户@主机 with grant option;
   grant select,insert on jing_dong.* to laowang@localhost with grant option
-- 刷新权限
   flush privilege
```

4.修改密码

```mysql
-- update user set authentication_string=password('新密码') where user='用户名';

update user set authentication_string =password ('1,2,3')where user ='laowang';
```

5.删除账户

```mysql
-- 语法1 drop user '用户名'@'主机'
   drop user 'laowang'@'%';
-- 使用root登录，删除mysql数据库的user表中数据 delete from user where user='用户名';
    delete from user where user='laowang';
-- 刷新权限
   flush privilege 
```



### 3.MySQL与Python进行交互

####1.Python 中操作 MySQL 步骤

```python 
from pymysql import *    #2.之后在程序中调用
conn=connect(localhost,port,database,user,password,charset)#3.创建connect连接对象
cs1=conn.cursor() #4.获取Cursor对象：
count = cs1.execute('select id,name from goods where id>=4')
print(count)

for i in range(count):
    result = cs1.fetchone() #获取一行的查询的结果
    print(result)# 打印查询的结果
                
result = cs1.fetchall()#查询多行数据
print(result)

conn.commit()
cs1.close() #关闭Cursor对象
conn.close()#关闭Connection对象
```

####2.参数化

1.sql注入

```python
程序开发过程中不注意规范书写sql语句和没有对特殊字符进行过滤，导致客户端在提交web表单,输入请求页面请求的查询字符串时能把sql命令插入其中,最终达到欺骗服务器执行恶意的SQL命令
```

2.如何防止

```sql
1.过滤掉一些常见的数据库操作关键字
2.在PHP配置文件中将Register_globals=off;设置为关闭状态
3.SQL语句书写的时候尽量不要省略小引号(tab键上面那个)和单引号
4.提高数据库命名技巧，对于一些重要的字段根据程序的特点命名，取不易被猜到的
5.对于常用的方法加以封装，避免直接暴漏SQL语句
6.开启PHP安全模式：Safe_mode=on;
7.打开magic_quotes_gpc来防止SQL注入
8.控制错误信息：关闭错误提示信息，将错误信息写到系统日志。
9. 使用mysqli或pdo预处理。
```

###4.数据库的优化

1.怎么优化数据库查询效率？

| 优化方法                                                     |
| :----------------------------------------------------------- |
| 1.储存引擎选择                                               |
| 2.对查询进行优化，要尽量避免全表扫描，首先应考虑在 where 及 order by 涉及的列上建立索引 |
| 3.尽量避免不规范的书写导致引擎放弃索引而进行全局扫描,(索引应尽量避免在 where 子句中使用: 1.or 来连接条件，如果一个字段有索引，一个字段没有索引/2. != 或 <> 操作符/3.进行null值判断) |
| 4.使用复合索引中的字段作为条件时,那么必须使用到该索引中的第一个字段作为条件时才能保证系统使用该索引分表分库, |
| 5.主从分离读写；采用主从复制把数据库的读操作和写入操作分离开来 |
| 6.对于多张大数据量的表的JOIN，要先分页再JOIN，否则逻辑读会很高，性能很差 |
| 7.尽可能的使用 可变长度的字符串varchar                       |
| 8.Update 语句，如果只要更改1,2个字段就不要Update全部字段,否则频繁调用会引起明显的性能消耗,同时带来大量日志 |
| 9.很多时候可考虑用 exists 代替 in                            |
| 10.尽量使用数字型字段                                        |

2.数据库的优化？

| 解决办法                                                     |
| :----------------------------------------------------------- |
| 1.选择合适的数据库引擎优化                                   |
| 2.不采用全文索引,避免不规范的书写,导致引擎放弃索引进行全局搜索 |
| 3.主从分离读写；采用主从复制把数据库的读操作和写入操作分离开来 |
| 4.设计表的时候严格根据数据库的设计范式来设计数据库           |
| 5.使用缓存，把经常访问到的数据而且不需要经常变化的数据放在缓存中,能节约磁盘IO |
| 6.优化硬件,采用SSD,使用磁盘队列技术(RAID0,RAID1,RDID5)等     |
| 7.分库分表分机器（数据量特别大),主要的的原理就是数据路由；   |
| 8.采用MySQL 内部自带的表分区技术，把数据分层不同的文件，能够提高磁盘的读取效率 |
| 9.进行架构级别的缓存，静态化和分布式                         |
| 10.垂直分表；把一些不经常读的数据放在一张表里，节约磁盘I/O； |
| 11.采用更快的存储方式，例如 NoSQL存储经常访问的数据          |

### 5.题目

```mysql
查出最贵的一件商品的商品信息。
select *  from  product  where price= (select  max(price)  from  product);
查出最大(最新)的商品的编号（ID）。 
select  pro_id  from product where pro_name like "%60%"; 
查出最便宜商品的价格。
select min(price) from product;
取出最小(最旧)的商品编号。
Select  min(pro_id) from product;  
查出所有商品的总数量。
select protype_id,count(*) from product group by protype_id with rollup;
查出所有商品的平均价格。
select protype_id,avg(price) from product group by protype_id;
查出联想品牌的所有商品的平均价格。
select avg(price) from (select * from product where pro_name like “%联想%”); 
按价格由高到低排序。
select * from product order by price desc
取出价格最高的前三个商品。
select  
查出每个产地各有多少数量的商品
select chandi,count(*) from product group by chandi;
查出每个品种(商品类别)各有多少个商品
select pinpai,count(*) from pinpai group by pinpai;

```

###注意

1.count(*)  count(字段) count(1) count(主键)

```
count(*):行数,不会忽略列值为NULL  
count(1):包括了忽略所有列，用1代表代码行，在统计结果的时候，不会忽略列值为NULL
count(列名):只包括列名那一列，在统计结果的时候，会忽略列值为空（这里的空不是只空字符串或者0，而是表示null）的计数，即某个字段值为NULL时，不统计。
```

2.SQL的case when then else end语句的用法

```sql
 --简单case函数

            case sex

            when '1' then '男'

            when '2' then '女'

            else '其他' end

--case搜索函数

            case when sex = '1' then '男'

            when sex = '2' then '女'

            else '其他' end
```

3.SQL中UNION和UNION ALL的详细用法

```sql
UNION:用于合并两个或多个SELECT语句的结果集，这里需要注意的是：UNION内部的SELECT语句必须拥有相同数量的列,列也必须拥有相似的数据类型，同时每条SELECT语句中列的顺序必须相同。
--注意:
	union操作符合并的结果集，不会允许重复值，如果允许有重复值的话，使用UNION ALL
```

4.SQL中的ROUND的使用

```sql
ROUND(AVG(score),2) --将平均成绩保持两位数
```

Oracle数据库的连接

```sql
su - oracle --从root切换到oracle用户进入：
sqlplus /nolog --进入sqlplus环境，nolog参数表示不登录：
sqlplus / as sysdba -- 以管理员模式登录：
conn 用户名/密码
```

