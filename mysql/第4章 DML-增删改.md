# 数据操作语言 #

### Data Manipulation Language ###

- 插入：insert
- 修改：update
- 删除：delete


## 插入语句 ##

### 语法 ###

- 方式1： insert into 表名(字段1,……) values(值1……)
- 方式2： insert into 表名 set 字段1=值1，字段2=值2……

### 特点： ###

- 插入数据和原类型一致或者兼容
- 不可以为null的必须插入数值，可以为null的必须插入数值
- 可以省略列名，自动补充默认的null
- 字段的顺序可以颠倒，但是必须一一配置
- 字段可以完全省略，但是必须一一匹配
- 举例：
    - 方式1；

            insert into beauty(id,name,email,phone,photo) values(1,'lutong','12312@gmail.com','1234235',null)
            
    - 方式2：
            
            insert into beauty(id,name,email,phone) values(1,'lutong','12312@gmail.com','1234235')

    - 方式3：
    
            insert into beauty values (1,'lutong','12321@gmail.com','1232',null);
    - 方式4：
    
            insert into beauty 
            select 1,'lutong','12321@gmail.com','1232',null;



### 方式比较 ###

- values方式支持多行插入,set不支持



        insert into beauty values
        (1,'lutong','123213@gmail.com',null,bull) 
        ,(2,'lutong2','12a3213@gmail.com',null,bull)
        ,(3,'lutong3','12ad3s213@gmail.com',null,bull);

- values方式支持子查询，set不支持  （数据插入则返回该数据）

        insert into beauty(id,name,phone)
        select 26,'宋茜','12321312';
        //不加（）
 



## 修改语句 ##


### 语法结构 ###

- 修改单表的记录
    - update 表名 字段1=值1，字段2=值2 where  条件
- 修改多表的记录
    - 92语法：
      
            update 表1 表2 
            set 表n的字段1=值1，表n的字段2=值2 
            where 条件
    - 99语法：
    
            update 表1 
            inner|left|right join 表2 
            set 字段=值 
            where 条件 


## 删除语句 ##

### 语法结构 ###

- 单表的删除

        delete  from 表名
        where 条件 
- 多表的删除
    - 92的删除
    
            delete 表1的别名，表2的别名
            from 表1 表1的别名，表2的别名
            where 连接 and 筛选条件
    - 99的删除

            delete 表1别名，表2别名
            from 表1 别名
            inner|left|right 表2 别名
            on 连接条件
            where 筛选条件
- 清空表的数据

        truncate table 表名
        



### 举例： ###
    
- 普通删除
    
        delete from beauty where phone like "%4136"


- 多表删除
    - delete后边的表名标志了将要删除的表的字段
            
            delete beauty ,boys
            from beauty join boys
            on beauty.boyfriend_id = boys.id
            where boys.boyName='张无忌'


- 整个表的数据清空
    - truncate 不可以添加where

            truncate table boys;


### delete和truncate比较 ###

- delete可以添加where,truncate不可以
- truncate删除效率更高
- 加入删除的字段中有自增字段，delete删除依旧断点开始，truncate则是从头开始
- truncate删除没有返回值，delete删除返回行数
- truncate删除不能回滚，delete删除可以回滚










