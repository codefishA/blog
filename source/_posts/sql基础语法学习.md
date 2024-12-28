---
title: sql语法学习

author: codefish
date: 2022-6-30 18:14:07
categories: extension
tags: [sql,mysql]
top_img: /img/man-sunglasses.jpg
cover: /img/man-sunglasses.jpg
---

作为一个前端，我认为sql的学习有助于理解业务，当别人告诉你这个表这个表时，不至于一知半解，我不指望一次入门学习能带来很多，但收获远比我想象的多的多。

## 1,基本使用

### 1.删除目录

1.点击安装文件夹，删除profile下面的文件夹

2.环境变量安装到sql下面的bin 文件夹

3.mySql 服务自动启动， 启动才能使用

### 2.命令启用服务

```js
net start MySQL
net stop MySQL
```

### 3.使用客户端连接数据库服务器

```js
 mysql -uroot -p密码 显示密码
 mysql -u root -p 隐藏密码
 exit 退出
```

**查看数据库**

show database

**使用数据库**

use myDatabase

**创建数据库**

create database bjdatabse

****

**数据库的基本单元**

表



## 2.基础知识

### 1.约束

**唯一性约束**

show tables **查看**数据库下面有那些**表**

### 2.sql语句的分类

1. DQL 数据查询语句 select
2. DML 数据操作语言 insert delete update **主要操作表中的数据**
3. DDL  数据定义语言 操作表的结构，删除列 create drop alter
4. RTL 事务控制语言 事务提交 事务回滚
5. DCL 数据控制语言，grant revote

### 3.表操作

- 查看表中数据

select * from dept 查看表 dept（查看所有数据

**只看结构**

select * from dept emp

不见分号不执行-sql 语句

```js
sql 语句以分号结尾
sql语句不区分大小写

```



查询语句

select name from 表名

查询两个字段-用逗号隔开

1.**查询所有的字段**

1. select * form fm_sn

2. select a,b,c fm_sn

3. **注意**：第一种写法可读性差，效率低（将*转换为所有的字段），实际中不建议此种方法

   

2.**起别名**

select name,id as newName from dept;

**注意**：注意只是将显示的表名改为新民，不改变原表

as关键字可以省略吗

select name ,id  newName from fm_sn;

*假设起的别名有空格，怎么办*?

结果： 报错

**'用单引号将别名包起来可以解决'** -双引号也可以

字符串统一使用单引号括起来，在其他数据库不能用（标准单引号

别名如果是中文需要用单引号包起来



**3.条件查询**

定义： 不是将所有数据查出来

```sql
select 
	a,b;
from
	表名
where
	a = 100; //条件
	 
	 ! 不等于 <>
	 between and >= <= 
	 sal >= 100 and sal <=300
	 between 100 and 300 必须左边数值小，右边数值大
	 = 是不行的 必须为 is null 
	 is not null 不为空
	 在数据库中null 不能使用= 进行衡量
```



### 4.单行处理函数

**定义**：一行一行的处理

**1.select  lower(ename) from eap;**

将表eap中的ename转小写

-*起别名处理*

select lower(ename) as ename from eap;

一一对应

**2.upper 转换为大写**

**3.取子串**

select substr(ename,1,1) from eap;

起始下标从1开始，没有0

eg: 找出员工第一个字母为a的信息

```sql
//1.
select ename from emp where ename like "A%";
select ename from where substr(ename , 1, 1) = "A"
//substr 从位置1开始截取，截取一个长度
//concat 字符串拼接
```

**4.str_to_date**

**5.data_format**

**6.format设置千分位**

**7.round四舍五入**

select round(123.567,1 ) from eap;

保留一位小数

select round(123.567,-1 ) from eap;    -> 1240

**8.rand生成随机数**

**9.ifnull 将null 转换为一个具体值**

在数据库中，只要又null 参与的数学计算，最后的结果一定是null

eg:计算员工的年薪

年薪 = （月薪 + 月补助）*12

问题： 如补助null，然后null参与了计算最后的结果一定为null，需要使用到ifNull函数

ifNull (args1,args2)

args1 数据

args2 把数据当那个默认值

eg: ifNull(a, 0)    //如果a为null把他当作0 来进行处理

*即对空值进行处理*

**10.case when then when then else and**

eg： 当岗位为normal时候，工资上调10%,当岗位为CEO时候，工资上调50%

select ename,jop,sal from emp;

```sql
select 
ename,job,sal, 
(case job when 'normal' then sal*1.1 when 'CEO' then sal*1.5 else sal end) as newsal
from emp;
//else后面的没有进行处理
```



**注意字面量和字面值**

```sql
//
select 'abc' as bleming from emp;
生成若干abc()

select abc from emp;
//将abc当作一个字段的名字，去表里面找abc字段

//字面量：当前列名
//字面值：所有值为当前的字段
```



**11.分组函数**

定义： 输入多行，输出一行

5个：

count 计数

sum 求和

avg 平均值

min 最小

max 最大

注意：分组函数必须对数据进行分组才能使用， 如果没有对数据进行分组，整个表默认为一组

eg1:

1.找出最高工资

 select   max(sal) from emp;

2.找出最低工资

select min(sal) from emp;

3.计算工资和

select sun(sal) from emp;

4.计算平均



### 5.分组函数

```sql
count
sum
avg
max
min
```



分组函数自动忽略null

#### 1.count字段

1. count() 统计字段下不为NULL的元素的总数(某一列null的数量)
2. count(*) 统计所有行数，（只要有一行数据count++)
3. 不存在所有的列都为空的列
4. 1

 

注意点： 分组函数不能直接使用在where子句中

找出比最低工资高的员工信息

```sql
select enamel，sal,from emp where sal >  min(sal)
reason: 分组函数必须分组才能使用，where之后不能使用分组函数

select sum(sal) from emp;

```

**所有的分组函数可以组合起来一起用**

select sum(sal),min(sal),max(sal),avg(sal),count(*) from emp

### 6.分组查询*****

定义： 先对数据进行分组，再进行计算，对每一组的数据进行操作

```sql
select 
...
from 
...
group by
..

eg:计算每个部门的平均和
eg：计算每个岗位的平均薪资
eg:找出每个工作岗位的最高薪资
```

**语句执行顺序**

```sql
select 4
..
from 1
..
where 2
.. 
group by 3
...
order by 5
```

注意： **在一条selecte语句中，如果有group by 语句，select 后面只能有分组的字段**

**按照多个字段进行分组**

找出每个部门不同工作岗位的最高薪资

select ename,job from emp where ename,job

**having子句**-对分组后的数据进行再一步过滤

不能单独使用，需要在group子句后面使用

```sql
select 
deptno,max(sal)
from emp
group by 
	deptno
having 
	max(sal) > 3000
	//以上的sql语句执行效率低-> 先筛选后分组

select 
	xxx
from 
	emp
where 
	sal > 3000
group by 
    deptno;
//能使用where的尽量使用where,优先使用where，实现不了的使用having


```

eg:找出每个部门平均薪资，要求平均薪资高于2500

```sql
select 
	deptno ,avg(sal)
from 
	emp 
group by deptno

=====>
==>必须使用having
select 
	deptno ,avg(sal)
from 
	emp
group by deptno
	sal > 2500
having
	avg(sal) > 2500;
```

**.数据**

```sql
//2.创建数据库
create database bjpowernode;
//3.选择数据库
use bjpowernode;
//4.导入数据
source d:\bjpowernode.sql;
//5.删除数据库
drop database bjpowernode;

**查看表的结构
desc dept;
```

## 7.连接查询

定义：多张表联合起来查询数据-连接查询

SQL92 92年出现的语法

SQL99 99年出现的语法

根据表连接的方式分类

- 内连接
  - 等值连接
  - 非等值连接
  - 自连接
- 外连接
  - 左外连接
  - 右外连接
- 全连接（使用少

**现象**

：两种表进行连接查询时候，没有任何条件限制的时候，会出现什么现象

拿一条表的数据对另一个表所有数据进行查询

```sql
select ename,dname from emp ,dept;
```

**问题：笛卡儿积现象**

查询结果：最终查询结果条数，为两个表数量之积

14*4 = 56

**解决**：连接时加条件，满足此条件的记录被筛选出来

```sql
select 
	ename ,dname
from 
	emp,dept
where emp.deptno = dept.deptno;
```

思考：查询结果提条数为14条，但是匹配的次数没有减少(56次)

问题：**查询效率低**

```sql
select 
	emp.ename,dept.dname
from 
	emp,dept
where 
	emp.deptno = dept.deptno;
```



**别名-效率**

```sql
select 
	e.ename ,d.dname
from 
	emp e,dept d
where 
	e.ename = d.dname 
```

e.ename = d.dname  **SQL92语法**

*减少表的连接次数-加快速率*-提升程序执行速率

**注意**：笛卡尔积-表的连接次数越多效率越低

## 8.内连接

**特点**：完全能够匹配这个条件的数据查询出来

### 1.等值连接

eg:查询每个员工所在的部门名称，显示员工名和部门名

```sql
SQL 92 内连接-等值连接
select 
	e.ename,d.dname
from 
	emp e,dept d
where 
	e.deptnp = d.deptnp

```



````sql
SQL 99
select 
	e.ename , d.dname
from 
	emp e,
join
	dpart d
where 
	e.dpartno = d.dpartno
````

内连接（inner

```sql
select 
	e.name , d.dname
from 
	emp e,
inner join 
	dpart d
where 
	e.dpartno = d.dpartno
	
```



**非等值连接**

```sql
select 
	e.ename , e.sal, s.grade
from 
	emp e
join 
	salgrade s
where 
	e.sal between s.losal and s.hisal;
	//条件不是一个等量关系，称为非等值连接
```

### 2.内连接之自连接

查询员工的上级领导，要求显示员工名和对应的领导名

![image-20220712085749880](D:\桌面\tools\开发问题\img\image-20220712085749880.png)

**技巧：一张表看作两张表**

```sql
select 
	a.ename as colleage, b ename sa leader
from 
	edpart a
join 
	edpart b
on
	a.mgr = b.empno
	
```

内连接：两表之间没有主次关系

## 9.外连接

eg1

```sql
select 
	e.ename , d.ename 
from 
	emp e
right join 
	dept d
on 
	e.deptno = d.deptno

```

**右外连接**:right代码将join右边的关键字看作主表，捎带关联查询左边表，又叫做右连接

在外连接中：两张表存在这主次关系

左外连接：同右，又叫做左连接，左连接右连接可以实现同一个效果（写法不一样

*外连接查询结果条数一定大于等于内连接*

outer 可以省略，代表外连接

```sql
select 
	e.ename , d.ename 
from emp
	emp e
right outer join 
	dept d
on 
	e.deptno = d.deptno

```

eg:查询每个员工的上级领导，要求显示所有员工的名字和领导名

```sql
select 
	a.ename , b.ename
from 
	emp a
left join
	emp b
on e.mgr = b.empno
```

## 10.多表连接

```sql
select 
	...
from 
	a
join 
	b
on 
	a 和 b 的连接条件
join
	c
on 
	a 和 c 的连接条件
right join 
	d
on 
	a 和 d 的连接条件

```

eg:要求找出每个员工的部门名称以及工资等级，要求显示员工名，部门名，薪资，薪资等级

![image-20220712094430009](D:\桌面\tools\开发问题\img\image-20220712094430009.png)

```SQL
select 
	e.ename , e.sal,d.mname,s.grade
from 
	emp e
join 
	dept d
on 
	e.deptno = d.deptno
join 
	salgrade s
on e.sal between s.losal and s.hisal;
```

**多查一个上级领导怎么办**，显示领导名字

思路：加一个外连接

```sql
left join 
	emp i 
on a.mgr = i.empno

```

## 11.子查询

定义：select中嵌套select语句。被嵌套的select为子查询

可以出现的位置

```sql
select 
	..(select)
from 
	..(select)
where
	..(select)
```

### 1.where 子句中出现子查询

eg1:找出比最低工资高的员工姓名和工资

```sql
select 
	ename,sal
from 
	emp
where
 	sal>min(sal);
 //where后面不能直接使用分组函数
```

实现思路：

- 查询最低工资值
- 查询比最低>800

```sql
select 
	ename
from 
	emp
where
	sal > (select min(sal) from emp);
```

### 2.from子句中的子查询

**注意**：from后面的子查询，可以将子查询的查询结果当作一张临时表

eg:找出每个岗位的平均工资的薪资等级

- 找出每个岗位的平均薪资

  select job,avg(sal) from emp group by job

  ![image-20220712110949665](D:\桌面\tools\开发问题\img\image-20220712110949665.png)

- 把以上查询结果当成已知的表

![image-20220712111109125](D:\桌面\tools\开发问题\img\image-20220712111109125.png)

```sql
select 
	t.* ,s.grade
from 
	t
join 
	salgrade s
on 
	t.avs(sal) betweent s.losal and s.hisal
```

完整写法

```sql
select 
	t.* ,s.grade
from 
	(select job,avg(sal) as avgsal from emp group by job) t
join 
	salgrade s
on 
	t.avs(sal) betweent s.losal and s.hisal
```

### 3.select 后面出现的子查询

eg:找出每个员工的部门名称，要求显示所有的员工名，部门名

```sql
select 
	e.ename, e.dpartno,(select d.dname from dept d where e.deptno 		=d.deptno) as dname 
from 
	emp e;
```

**注意：对于select**后面的子查询，只能返回一条结果，多余一条就报错***

## 12.union 合并查询结果集

eg:查询工作岗位是MANAGER和SALESMAN的员工

```sql
select 
	ename ,job 
from 
	emp 
where 
	job = 'MANAGER' or job = 'SALESMAN'
-------------------------------------------
select 
	ename , job 
from 
	emp 
where 
	job in ('MANAGER','SALESMAN')
```

**union 效率更高**

ps：对于表连连接来说，每连接一次新表，则匹配的次数满足笛卡尔积，成倍的翻。。。

union 可以减少匹配的次数，在减少匹配次数的情况下，还可以完成两个结果的拼接。。。

a-b-c = 10 - 10 -10 /1000

**如果使用union**

```sql
a-b 10*10
a-c 10*10
总次数 200
```

Oracle要求，union列数相同，，并且数据的类型必须相同

## 13.limit

使用场景：分页查询

定义：将查询结果的一部分取出来

作用：提高用户的体验，一次都查询出来体验差

**eg1:**按照薪资降序，取出排名在前五名的员工

```sql
select 
	ename ,sal
from 
	emp
order by
	sal desc
limit 5;
----------------------------
完整用法
limit 0,5; startIndex起始下标（默认为0） ,length
缺省用法
limit 5


```

注意：**limit在order by 之后执行**

**eg2** ：取出第3-5名的员工

```sql
select 
	ename ,sal
from 
	emp
order by
	sal desc
limit 2,3
```

eg3:取出工资排名在5-9名的员工

```sql
select 
	ename,sal
from 
	emp
order by 
	sal desc
limit 4,5

```



## 14.分页

假设：每页显示三条数据

```sql
limit 0,3
limit 3,3
limit 6,3
limit 9,3
```

ps：每页显示pageSizes条记录，第pageNo页：limit ? ,pageSize

 第pageNo页：limit (pageNo - 1) * pageSize , pageSIze

**通用写法**

limit (pageNo - 1)*pageSize , pageSize

## DQL 总结

```sql
select 		5.
	...
from        1.
	...
where 		2.
	....    
group by	3.
	...
having		4.
	...
order by	6.
	...
limit 		7.
	...
```

**多表联查**



## 15.表的创建

属于DDL语句

create ,drap , alter

```sql
1.
create table 表名（字段名1，数据类型,字段名2，数据类型
2.
create table 表名（
	字段名1，数据类型,
	字段名2，数据类型
）
```

**数据类型**- 关于sql

表名：建议见表知名，字段名同样。fm_part

- varchar
- char
- int
- bigint
- float
- double
- string
- datetime
- date
- clob
- blob

### 1.varchar

ps：可变长度字符串（255） 

​		name varchar(10)

新增一个字段name,动态分配长度，根据数据动态判断，实际动态长度，比较智能，节省空间，会根据实际的数据长度动态分配空间

### 2.char（255

ps：定长字符串

name char(10)

name = jack 

不管实际的数据长度是多少，分配固定长度的空间去存储数据，使用不当的情况下，可能造成数据空间的浪费

- varchar  节省空间但是慢
- char        快，使用不当可以造成空间浪费

eg1 ： 性别->char

eg3 ： 姓名->varchar   (variable)

### 3.int（11位数

ps：数字中的整数型

### 4.bigInt 

ps: 等同于java中的long

### 5.date 

ps：短日期类型

### 6.datetime

ps:长日期类型

### 7.clob

ps: 字符大的对象，最大可以存储4G的字符串

比如:存储超过255个字符的都要使用CLOB字符大对象来存储

Character Large Object :

### 8.blob

二进制大对象

Binary Large Object

专门用来存储图片，声音，视频等流媒体数据

往BOLB类型的字段上插入数据的时候，例如插入一个图片，视频等，你需要使用IO流才行。

```js
t_table 表
编号 no(bigint)
名字 name(varchar)
描述信息 description(clob)
上映时间 playtime(date)
时长 time(double)
海报 image(blob)
类型 type(int)
```





eg1:创建一个学生表

```sql
create table t_student{
	no int,
	name varchar(32),
	sex char(1) default 1,
	age int(3),
	email varchar(255)
}
```

删除表

​	drop table t_student;

注意：但他不存在的时候会报错

​	drop  tablet if exists t_studen ;   -> 表存在才删除

## 16.数据操作

语法：

​	insert into 表名（字段名1，字段名2，字段名3...) values (值1，值2，值3)

注意：字段名和值要一一对应，数量要对应，数据类型要对应。

```sql
insert into t_student (no ,name, sex,age,email) values (1, 'zhangsan','m','20','zhangsan','12534652@163.com')

------------------------值与字段一一对应---------------------------
insert into t_student (no ,name, sex,age,email) values (1, 'zhangsan','m','20','zhangsan','12534652@163.com')
```

ps:可以只添加部分字段，未赋值的字段，值为null

tips:insert 语句但凡执行成功，必然会多一条记录，其他值为null,不指定默认值。默认值为null

insert 语句中的字段名 可以省略吗

```sql
insert into t_student values(2)
--缺省表示写上所有字段
```

### 1.插入日期

1

```sql
```

### 2.格式化数字

```sql
select ename ,format(sal,'$999,999') as sal from emp;
```

加入千分位

### 3.str_to_date

将字符串varchar类型转换为date类型

通常使用在插入insert方面，因为插入的时候需要一个日期类型的数据，通常需要改函数将字符串转换为date

```sql
insert into t_user(id,name,birth) value(1, 'zhangsan'， str_to_date('01-10-1990'),'%d-%m-%Y')
```

如果提供的日期时1990-10-01，str_to_data函数就不需要了

```sql
insert into t_user(id,name,birth) values('1','list','1990-10-01')
```

查询的时候可以使用特定的日期格式转换吗

```sql
select id，name,date_format(birth,'%m/%d/%Y') as birth from t_user
```





### 4.date_format

将date类型转换为具有一定格式的varchar字符串类型

```sql
date_format(日期类型数据，‘日期格式’)
这个函数通常使用在查询日期方面，设置展示的日期格式
select id,name,birth from t_user
以上sql语句实际上进行了默认的日期格式化，
自动将数据库中的date类型转换为varchar字符串
采用的时mysql默认的日期格式，‘%Y-%m-%d'
```



### 5，数据库命名规范

所有的标识符全部小写，单词之间使用下划线连接

### 6.mysql的日期格式

- %Y 年
- %m 月
- %d 日
- %h 时
- %m 分
- %s 秒

### 7.date和datetime区别

datetime包含年月日时分秒信息

```sql
drop table if exists t_user;
create table t_user (
	id int,
    name varchar(32),
    birth date,
    create_time datetime
)
```

短日期默认格式： %Y-%m-%d

长日期默认格式： %Y-%m-%d %h:%i:%s

```sql
insert into t_user(id,name,birth,create_time) values('1','zhangsan','1990-10-10','2022-07-13 19:29:29')
```

#### now函数

定义：获取系统当前时间，带有时分秒信息

## 17.修改update(DML)

语法：update 表名 set 字段名1 = 值1 ，字段名2 = 值2，字段名3 = 值3...where 条件

注意：没有条件限制会导致所有的数据全部更新

```sql
update t_user set name = 'jack',birth = '2000-10-11' where id = 2
```

## 18.删除语句

delete from 表名 where 条件

注意: 没有条件整张表的数据都会被删除

```sql
delete from t_user where id = 2;
insert into t_user(id) value(2);
delete from t_user; //删除所有
```

**去除重复记录**

*只能出现在所有字段的最前面*，出现在job,deptno两个字段之前，表示两个字段联合起来去重

```sql
select distinct job from emp;
```

错误写法：select ename , distinct fob from emp;

正确写法：select fistinct job,deptno from emp;

eg1:统计一下岗位总数-技巧

```sql
select count(distinct  job) from emp;
```





## 19.连接查询





















































