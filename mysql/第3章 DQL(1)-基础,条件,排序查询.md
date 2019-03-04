



# DQL(Data Query Language) #


## 基础查询 ##

- select 查询列表 from 表名;
    - 查询的列表可以是：表中的字段，常量值，表达式，函数
    - 查询的结果是一个虚拟的表格

### 分类 ###


#### 字段 ####

- 查询单个字段：select id from employees;
- 查询多个字段：select id,name from employees;
- 查询所有字段：
    - select * from employees;
    - select 逐个字段 from employees;

#### 常量 ####

- select 常量值。
    - select 100;
    - select "demo";

#### 表达式 ####

- select 表达式；   （等同于计算结果）
    - 举例：select 100%51;  结果：49

#### 起别名 ####

- F1:as。select 字段名 as 新字段名  from 表名;
    - 举例：select 100%51 as 结果;
- F2:空格。select 字段名 新字段名 from表名。




**注释：着重号可以加给字段名。举例：select `name`from employees;**



#### 去重 ####

distinct

- select  distinct * from 表名;
- 去重只可以对单个字段去重，否则报错


#### +的作用 ####


只有加法的作用

- 为数字直接运算。select  20+10;    结果：30
- 为字符转换成数字直接运算。 select '20'+10;   结果：30
- 为不可解析的字符，转化成0。  select  'x'+10;  结果：10
- 如果出现一个null是，结果为null。  select  null+100;  结果：null


#### 相关函数 ####


concat(str…)字符连接函数

- select concat(str1,str2) as str from  user;

isnull(str)判定为空函数

- select isnull(str) from user;
- str字段为空返回0，不为空返回1

ifnull(str,默认值) 如果是null设置默认值


## 条件查询 ##

- select 字段名 from 表名 where 条件


### 条件分类 ###

#### 条件表达 ####

- 五种：< > =  != (等同<>)  <= >= 
    - 举例：select * from employees where salary >12000;
#### 逻辑表达 ####

- 三种：&& , ||, ！   (等同于 and  or not  (推荐))
    - select * from employees where salary <20000 and salary>12000  
#### 模糊查询表达 ####

- like 
    - 使用%和_通配符
    - 还支持数值型
    - select * from  employees where name like "%大%";  结果：返回包含大的数据
    - select * from  employees where name like "__a_b%";  结果：返回第三个字符为a,第五个字符为b的数据
- between and和  not between  and
    - 包含临界值
    - 前后的值的顺序由小到大
    - select * from employees where id <=10 and id>=1的等价转化 
    - 举例：select * from employees where id between 1 and 10; 
- in
    - 使用in提高代码的整洁度
    - in ()的内容必须或者兼容  ('123',123)
    - 不可以使用like的%和_通配符 
    - select *from employees where job ="A"  or "B"  的等价转化
    - 举例：select * from employees where job in ("A","B");

- is null和is not null
    - 专门判定null值
    - is只可以和null 或者 not null组合，子判定null类型 

- <=>安全等于
    - 可以判定null值和普通类型的值 
    - 可读性比较低

**注释：匹配的转译字符**

- F1： \+字符  例如：like  "\_%"  表示第一个字符为_的名字
- F2: like "字符组成" escapt "字符"  例如：like "$%" escapt "$"


## 排序查询 ##



### 基本语法 ###


- select 字段 from 表名  where 条件  order by 字段 asc或者desc
- asc升序，desc降序
- 不写asc或者desc表示默认升序


### 排序条件 ###

- 单字段排序： order by 字段
- 多字段排序： order by 字段1，字段2
- 表达式排序：order by 表达式
- 别名排序：order by 别名
- 函数排序 ：order by 函数

        select *,salary*12 as yearSalary from emploees order by id,name asc;
        select *,salary*12 as yearSalary from emploees order by yearSalary asc;
        select *,salary*12 as yearSalary from emploees order by salary*12 asc;
        select *,salary*12 as yearSalary from emploees order by length(name) asc;


### 多字段排序 ###

- select 字段 from 表  order by 字段1 方式,order by 字段2 方式2；
- 用于第一字段一样，第二字段进行排序，等多字段