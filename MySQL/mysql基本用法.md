# 1. 数据库常用命令

## 1.1 SQL分类

1. 数据查询语言(DQL-Data Query Language)  

   代表关键字:select 

2. 数据操纵语言(DML-Data Manipulation Language)

   代表关键字:insert,delete,update 

3. 结构定义语言(DDL-Data Definition Language)

   代表关键字:create ,drop,alter,

4. 事务控制语言(TCL-Transactional Control Language)

   代表关键字:commit ,rollback;

5. 数据控制语言(DCL-Data Control Language)

   代表关键字:grant,revoke.

   

## 1.2 查看mysql版本

`select version();`

结果:

+-----------+

| version() |

+-----------+

| 8.0.22  |

+-----------+

## 1.3 终止一条SQL语句

如果想要终止一条正在编写的语句，可键入\c。

## 1.4 退出MySQL

EXIT;即可退出

# 2. 条件查询

## 2.1 常用的条件运算符

|      运算符      |                             说明                             |
| :--------------: | :----------------------------------------------------------: |
|        =         |                             等于                             |
|      <>或!=      |                            不等于                            |
|        <         |                             小于                             |
|        <=        |                           小于等于                           |
|        >         |                             大于                             |
|        >=        |                           大于等于                           |
| between … and …. |            两个值之间,等同于 >= and <=  左小右大             |
|     is null      |                 为null（is not null 不为空）                 |
|       and        |                             并且                             |
|        or        |                             或者                             |
|        in        |          包含，相当于多个or（not in不在这个范围中）          |
|       not        |                not可以取非，主要用在is 或in中                |
|       like       | like称为模糊查询，支持%或下划线匹配%匹配任意个字符下划线，一个下划线只匹配一个字符 |
|      Concat      |                          拼接字符串                          |

demo:

1. "="

```mysql
select empno, ename, sal from emp where sal=5000;
```

2. "<>或!=null"

```mysql
select empno, ename, sal from emp where sal <> 5000;
select empno, ename, sal from emp where sal != 5000;
```

3. "<、<=、>、>="

```mysql
select empno, ename, sal from emp where sal < 5000;
select empno, ename, sal from emp where sal <= 5000;
select empno, ename, sal from emp where sal > 5000;
select empno, ename, sal from emp where sal >= 5000;
```

4. "between..and..."

```mysql
select empno, ename, sal from emp where sal between 1600 and 3000;
```

**注意:**关于between … and …，它是包含最大值和最小值的范围期间必须是从小到大 不能是从大到小,可以运用到字符方面,但比较的是首字母,并且是左闭右开

5. "is null 跟is not null"

```mysql
select * from emp where comm is null; //条件comm字段必须为空
select * from emp where comm is not null; //条件comm字段必须不为空
```

**注意:**在数据当中null不是一个值,代表什么也没有,为空。不能用等号来衡量

6. "and跟or"

```mysql
select * from emp where job='MANAGER' and sal > 2500; //and是并行关系
select * from emp where job='MANAGER' or job='SALESMAN'; //or是或的关系
```

7."in跟not in"

```mysql
select * from emp where job in ('manager','salesman');//or的升级版本
select * from emp where job not in ('manager','salesman');//取反操作表示不包含
```

**注意:**in表示包含的意思，完全可以采用or来表示，采用in会更简洁一些，并且执行效率跟or没区别,In后面的值不是区间，是具体的值

8. "like"

```mysql
select * from emp where ename like 'M%';
```

**注意:**

-  %x%表示字符中含有字符的
- _A%表示第二个字符为A的
- A%表示以A字符为开头的字符
- %A表示已A字符为结尾的字符
- 代表任意多个字符，_代表任意一个字符
- 如果查询含有下划线的字符那么就得使用转义字符%\ _%,\具有转义作用，能将后面的字符转为普通字符
- Like 中的表达式必须放到单引号中|双引号中

9. "Concat"

```mysql
语法:CONCAT(str1,str2,…) 
su.email like concat('%', #{userName},'%')//经常用于跟like模糊查询进行拼接匹配
```

## 2.2 Linux区分大小写

MySQL在windows下是不区分大小写的，将script文件导入MySQL后表名也会自动转化为小写，结果再 想要将数据库导出放到Linux服务器中使用时就出错了。因为在Linux下表名区分大小写而找不到表，查了很多都是说在`linux下更改MySQL的设置使其也不区分大小写`，但是有没有办法反过来让windows 下大小写敏感呢。其实方法是一样的，相应的更改windows中MySQL的设置就行了:

具体操作:

MySQL在windows下是不区分大小写的，将script文件导入MySQL后表名也会自动转化为小写，结果再 想要将数据库导出放到Linux服务器中使用时就出错了。因为在Linux下表名区分大小写而找不到表，查了很多都是说在Linux下更改MySQL的设置使其也不区分大小写，但是有没有办法反过来让windows 下大小写敏感呢。其实方法是一样的，相应的更改windows中MySQL的设置就行了

# 3. 排序数据

## 3.1 概述

排序采用order by子句，order by后面跟上排序字段，排序字段可以放多个，多个采用逗号间隔，order by默认采用升序，如果存在where子句那么order by必须放到where语句的后面

关键词:`ASC`:升序(默认) `DESC`:降序 放在by后面关键词后面

## 3.2 单一字段排序

```mysql
select * from emp order by sal;// 按照从小到大的排列顺序排列
select * from emp order by sal asc;//上面默认asc 现在这是显式指定排列顺序
select * from emp order by sal desc;//对数据进行降序排列
```

## 3.3 多个字段排序

```mysql
select * from emp order by job desc, sal desc; //如果采用多个字段排序，如果根据第一个字段排序重复了，会根据第二个字段排序
```

**注意:**第一个排序的语句占主导地位，第二个排序语句只要当第一个排序语句相等的情况下才会执行。

> 越靠前的字段越能起到主导作用，只有当前面的字段无法完成排序的时候，才会启用后面的字段。



## 3.4 order by 运行顺序

*order by如果所在的SQL语句中没有条件，那么就可以不用加where ，并且order by如果在where条件一起搭配使用，那么前面不用加and or之类的关键词*

执行顺序：

```mysql
select
	字段		3
from
	表名		1
where
	条件		2
order  by 
	排列		4
```



# 4. 函数

## 4.1 分组函数

| 函数名 |    含义    |
| :----: | :--------: |
| count  | 取得记录数 |
|  sum   |    求和    |
|  avg   |   取平均   |
|  max   | 取最大的数 |
|  min   | 取最小的数 |



**注意:**所有的分组函数都是对"某一组"数据进行操作,意思就是如果查询数据用到分组函数那么就相应伴随着分组Group by

demo:

1. "count"

```mysql
select count(*) from emp;//Count(*)表示取得所有记录，忽略null，为null的值也会取得
select count(comm) from emp;//采用count(字段名称)，不会取得为null的记录
```

### 4.1.1 Count(*)跟Count（具体的某个字段）的区别？语法知识

1. Count（\*）：不是统计某个字段中数据的个数，而是统计总记录条数（忽略NULL，理解为包含NULL，），在统计结果的时候，不会忽略列值为NULL的记录
2. Count（1）：忽略所有列，1表示一个固定值，也可以用count(2)、count(3)代替，在统计结果的时候，不会忽略列值为NULL的记录。
3. Count（字段)：表示统计字段中不为NULL的数据总数量（不包含NULL），在统计结果的时候，会忽略列值为NULL的记录（不包括空字符串和0），即列值为NULL的记录不统计在内。



> 执行效率:
>
> 1.统计列为主键:count(字段)>count(1)
>
> 2.统计列不是主键:count(1)>count(字段)
>
> 3.表存在主键:count(主键)最快
>
> 4.表只有一列:count(*)最快
>
> 5.表有多列且不存在主键:count(1)>count(*)

总结:统计具体字段则count(字段)、统计总记录数则count(主键)



2. "sum"

```mysql
select sum(字段) from emp;//可以取得某一个列的和，null会被忽略
```

3. "avg"

```mysql
select avg(字段) from emp;//对某一列进行去平均数
```

4. "max"

```mysql
select max(字段) from emp;//对某一列求最大值
```

5. "min"

```mysql
select min(字段) from emp; //对某一列求最小值
```



> 注意：
>
> 1.分组函数不能直接使用在where关键字后面。
>
> 2。特点：输入多行，最终输出的结果是一行
>
> 3.分组函数会自动忽略NULL（不包含NULL）。只取现有的
>
> 4.在数据库中只要跟NULL参与运算那么都会得NULL值。

## 4.2 SQL处理函数

|    函数名    |                       含义                       |
| :----------: | :----------------------------------------------: |
|    ifNull    |                    空处理函数                    |
|  Case When   |            搜索函数(类似if/else用法)             |
|    lower     |                      转小写                      |
|    upper     |                      转大写                      |
|    subStr    |                    截取字符串                    |
|    length    |                    字符串长度                    |
|     trim     |                   祛除首尾空格                   |
|    rand()    |                    生成随机数                    |
|    round     |                     四舍五入                     |
|    exists    |              判断查询子句是否有记录              |
| group_concat |         将分组中的字段值拼接给新的字符串         |
|     cast     |                     类型转换                     |
|    nullIf    |     两个参数一样则返回null,否则取第一个参数      |
| find_in_set  | 判断指定字符串在以逗号分隔的字符串列表中的位置。 |
|      if      |              类似于java中的if-else               |

**注意:**上面分组函数用于多行处理而这个都是属于单行处理函数,并且不依靠group by



dmeo:

1. "ifNull"

```mysql
select ename,(sal+ifnull(comm,0))*12 as year from emp;//顾名思义:如果第一个参数为null,那么就取第二个参数
```

**注意:**如果查询出来为null,可能是一条记录都不存在所以才会为null,ifNull是建立在有数据的基础上的



2. "Case When"

`有两种格式:1.简单函数2.搜索函数`

- case 列名

  when  条件值1  then  选择项1

  when  条件值2   then  选项2.......

  else   默认值    end

```mysql
select 
	case sex
		when '1' then '男'
		when '2' then '女'
	else '其他'  end
from student
```

其中:else可不用放,如果不加else那么没满足条件的都为NUll

- CASE

  WHEN 列名1= 条件值1 THEN 选择项1

  WHEN 列名2= 条件值2 THEN 选择项2

  ELSE 默认值 END 

```mysql
select 
	case 
		when sex='1' then '男'
		when sex='2' then '女'
	else '其他'  end
from student
```



> 通常我们在写Case When的语句的时候,会容易忘记 end 这个结束,一定要记得哟!

工作中大多数用的是第一个格式,因为通常我们会对数据库的字段值来匹配给前端来使用,当然也可以拿到java中处理



3. "lower"

```mysql
select  lower(ename) from emp;//对字段进行小写
```



4. "upper"

```mysql
select * from emp where job=upper('manager');//对字段进行转大写
```



5. "subStr"

```mysql
select * from emp where substr(ename, 1, 1)=upper('m');//substr(字段,截取下标,截取长度) ,这里下标都是从1开始
```



6. "length"

```mysql
select length(ename), ename from emp where length(ename)=5;
```



7. "trim"

```mysql
select * from emp where job=trim(upper('manager'));//trim会去首尾空格，不会去除中间的空格。
```



8. "rand"

```mysql
select round(123.56);//结果:123 可指定保留四舍五入小数精度如:round(123.12,1)
```



9. "rand"

```mysql
select rand();//返回大于等于 0 及小于 1 的均匀分布随机实数，每次计算工作表时都将返回一个新的
```



10. "exists"

> EXISTS 运算符用于判断查询子句是否有记录，如果有一条或多条记录存在返回 True，否则返回 False。

普通SQL查询:

```mysql
select 姓名 from 学生表 where 学号 in(select 学号 from 选课表 where 课程号='C1')
```

带exists的SQL查询:

```sql
select 姓名 from 学生表 where exists ( select * from 选课表 where 学生表.学号=选课表.学号 and 课程号='C1')
```

### 4.2.1 exists查询流程:

先在外层查询中取“学生表”的第一行记录，用该记录的相关的属性值（在内层WHERE子句中给定的）处理内层查询，若外层的WHERE子句返回“true”值，则这条记录放入结果表中。然后再取下一行记录；重复上述过程直到外层表的记录全部遍历一次为止。

区别:

- in 和 exists的区别: 如果子查询得出的结果集记录较少，主查询中的表较大且又有索引时应该用in,  反之如果外层的主查询记录较少，子查询中的表大，又有索引时使用exists。其实我们区分in和exists主要是造成了驱动顺序的改变(这是性能变化的关键)，如果是exists，那么以外层表为驱动表，先被访问，如果是IN，那么先执行子查询，所以我们会以驱动表的快速返回为目标，那么就会考虑到索引及结果集的关系了 ，另外IN时不对NULL进行处理。
- EXISTS语句不关心子查询的具体内容，因此用“SELECT *”，“Exists + 子查询”用来判断该子查询是否返回记录。



11. "group_concat"

```mysql
select ww_task_id,GROUP_CONCAT(DISTINCT dept_name ORDER BY dept_id separator ';' ) as dept_name from  xtcz_ww_task_dept  GROUP BY ww_task_id
```

**注意:**默认分隔符是"," 如果想指定分隔符则使用separator来指定



11. "cast"

```mysql
SELECT (1 + CAST('1' AS UNSIGNED))/2;//虽然这里mysql会隐式转换但有些特殊情况是需要我们显式指定类型,比如order by,如果类型是字符串类型那么排列排序将会错位
```



12. "nullIf"

```mysql
SELECT NULLIF(1,1); //一样返回null 如果第一个参数等于第二个参数，则NULLIF函数返回NULL，否则返回第一个参数。
```



13. "find_in_set"

```mysql
SELECT FIND_IN_SET('y','x,y,z');//判断是否包含 第一个参数:要查找的字符串 第二个参数:以逗号分隔的字符串列表
```

**注意:**如果第一个参数为null,那么则返回null。此函数经常用于where条件中。



14."if"

```mysql
想要获取类似于java中的if-else效果则可以这样处理
demo:
if(x=1,y,z) //判断x是否等于1 ，如果等于1则取y，不然就取z
```



> 注意：尽量在SQL中少用函数，因为会造成执行效率降低运行速度低，并且有些函数放在SQL中不好掌控



# 5. 分组查询

## 5.1 group by

出现在select后面的字段 要么是是聚合函数中的,要么就是group by 中的.

```mysql
select job, sum(sal) from emp group by job;
```

在SQL语句中若有group by 语句，那么在select语句后面只能跟分组函数+参与分组的字段。否则会报sql的严格模式错

### 5.1.1多字段分组

```mysql
select empno,deptno,avg(sal) from emp group by deptno,empno; 
```

**注意:** 将x,y看成一个组合字段,如果x+y的值能分一组 则分为同一组,如果不能,那么新建一组



## 5.2 having

```mysql
select job, avg(sal) from emp group by job having avg(sal) >2000; //如果想对分组数据再进行过滤需要使用having子句
```



执行顺序:

select 字段  5

from 表名 1

where ……. 2 

group by …….. 3

having …….(就是为了过滤分组后的数据而存在的—不可以单独的出现) 4

order by …….. 6



> 能在where中过滤的数据，尽量在where中过滤，效率较高。having的过滤是专门对分组之后的数据进行过滤的。



## 5.3 distinct

对查询结果进行去重

```mysql
select distinct job from emp;
```

**注意:**distinct关键字只能出现在所有字段的最前面(含义:是对后面的字段联合起来去除重复记录),只对查询结果去重,不对表数据进行修改