## 分组查询 ##


### 语法标准结构 ###

    select 字段,分组函数,分组字段 
    from 表
    where 筛选条件
    group by  分组字段
    having 过滤条件（包含聚合函数）
    order  by 字段  asc/desc;



#### 普通分组 ####

        select max(salary) job_id 
        from employees
        froup by job_id



#### having分组 ####

        //分组后筛选
        select count(*) ,department_id
        from employees
        group by  department_id
        having count(*)>2  and  name is not null
        

        //分组后筛选
        select count(*) ,department_id
        from employees
        where name is not null
        group by  department_id
        having count(*)>2  


#### 可以通过表达式或者函数分组 ####


        select count(*) , LENGTH(last_name) 
        from employees
        WHERE  last_name is not null
        GROUP BY LENGTH(last_name)
        HAVING count(*) >5


### 关于having和where的区别 ###

- 区别一：
    - having是用于分组后--结果筛选
    - where是用于分组前--条件查找
- 区别二：
    - having可以添加分组函数
    - where不可以添加分组函数


#### 按照多个条件分组 ####

**考虑到性能问题：优先考虑分组前筛选**



## 连接查询（多表查询） ##


### 错误查询：笛卡尔乘积现象 ###

- 实现了每一项匹配另一张表的每一项（m*n）

        select  name ,boyFriend
        from beauty,boys.

### 正确查询 ###

        select name,boyFriend
        from beauty,boys
        where beauty.name_id=boyFriend.name_id;







### 匹配过程 ###

- 笛卡尔积全部匹配
- where进行筛选




### 分类 ###

#### 92sql语法 （不推荐） ####

- 分类

    - 等值连接
    - 非等值连接
    - 自连接
- 基本语法

        select 字段1，字段2
        from 表1，表2
        where 表1表2连接
        【group by  分组字段 
        having 筛选条件
        order by 字段】
### 1.sql92-内连接-等值连接 ###

#### 等值连接查询举例 ####

- 表名过长起别名（起别名后不可用原来的名）

- 两个表的前后位置可以更换（原理：笛卡尔积每个都*）

        SELECT   department_name,city
        from departments d,locations
        WHERE d.location_id = locations.location_id
        and city like "_o%"

- 连接+统计函数

        select job_title,count(*)
        from employees e,jobs j
        where e.job_id=j.job_id
        groups by job_title
        order by count(*) desc;

- 多表连接

        select last_name,department_id,city
        from emploees e,departments d,locations l
        where e.department_id = d.department_id
        and l.location_id=d.department_id
        order by last_name desc;



#### 等值连接特点 ####

- 查询结果为两个表的交集部分
- n个表则右n-1个连接条件
- 表的先后顺序无关
- 一般需要起别名
- 可以搭配：排序，分组，筛选


### 2.sql92-内连接-非等值连接 ###


#### 非等值连接举例 ####

    select salary,grade_level 
    from employees e,job_grade j
    where salary between  g.lowest_sal and g.highest_sal
 

    select country_id,count(*)
    from departments d,lcoations l,
    where d.location_id=l.location_id
    group by country_id
    having count(*)>2

### 3.sql92-内连接-自连接 ###

#### 含义 ####

- 自表连接自表

### 自连接举例 ###

    select e.employee_id,e.employee_name,e2,employee_id,employee_name
    from employees e,employees e2
    where e.manager_id=e2.employee_id



#### 99sql语法（推荐） ####

- 内连接
    - 分类
        - 等值连接
        - 非等值连接
        - 自连接
    - 基本语法

            select 字段
            from 表名1
            inner join 表名2
            on 连接条件
            【group by  分组字段 
            having 筛选条件
            order by 字段】

- 外连接
    - 特点
        - 存在主从表
        - 主表和附表存在一致的，则显示相应的数,主表和附表数据不对应，则显示null
        - 其结果=内连接结果+主表中没有的记录 
        - 记录个数=主表的条数+多匹配的个数,（一个主表匹配多个附表）
    - 分类
        - 左外连接:left (outer) join  on
            - 左边的的是主表，右边是附表
        - 右外连接:right(outer) join  on
            - 右边的是主表，左边的是附表
        - 全外连接:full(outer) join  on
            - 显示结果=内连接结果+主表有，附表没有+主表没有，附表有
            - (不支持)
    - 语法

            select 字段1，字段2
            from 表1
            inner join 表2
            on 连接条件
            【where  筛选条件
            group by  分组字段 
            having 筛选条件
            order by 字段】


- 交叉连接
    - 基本语法：
   

            select 字段1，字段2
            from 表1
            cross join 表2 
    - 含义：
        - 两个表的笛卡尔乘积
        - 结果个数=m*n



### 1.sql99-内连接-等值连接 ###

- 基本语法

        select 字段
        from 表名1
        (inner) join 表名2
        on 连接条件
        【group by  分组字段 
        having 筛选条件
        order by 字段】

- 举例
    - 普通等值连接
    - 添加筛选

            select last_name,job_titile
            from employees e,
            inner join jobs j
            where e.last_name like "_o%"

    - 分组+筛选+排序

            select city,count(*)
            from departments d
            inner join  locations l
            on d.location_id=l.location_id
            group by city 
            having count(*)>3
            order by count(*)

    - 多表连接 

            select last_name,deparment_name,job_title
            from employees e
            inner join departments d  
            on e.deparment_id=d.department_id
            inner join jobs j 
            on d.job_id = d.job_id
            order by department_name desc;
- sql99的内连接特点
    - 可以添加排序，分组，筛选
    - inner可以省略
    - 筛选条件放在where后边，连接条件放在on后边：提高分离性，便于阅读
    - 96sql的inner join on内连接和92sql的where内连接效果一样，都是求交集

**注释：inner join多个表进行连接时，需要一定的顺序性（连接条件）** 

### 2.sql99-内连接-非等值连接 ###

- 举例子

        select count(*),grade_level
        from employees e
        inner join job_grades j
        on e.salaty between  g.lowest_sal  and  g.highest_sal
        group by grade_level



### 3.sql99-内连接-自连接连接 ###

- 举例子

        select
        e.employee_id,e.employee_name,e2,employee_id,employee_name
        from employees e 
        join employees e2
        on e.manager_id=e2.employee_id


### 4.sql99-外连接-左右连接 ###

#### 举例子 ####

- 左外连接
    
        select g.name ,b.*
        from beauty g
        left join boys b 
        on g.boyfriend_id=b.id
        where b.id is not null

- 右外连接

        select g.name ,b.*
        from join boys b
        right beauty g 
        on g.boyfriend_id=b.id
        where b.id is not null



### 内外连接查询总结 ###


![](https://i.imgur.com/kxFs89q.png)


![](https://i.imgur.com/whpE3Oj.png)