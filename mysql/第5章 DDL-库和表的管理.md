#DDL 数据定义语言

### 库和表的操作 ###
- 创建： create
- 修改： alter
- 删除： drop


## 库的管理 ##

### 库的创建 ###

- 语法

        create database  [if not exists]库名;



- 举例

        CREATE DATABASE IF NOT EXISTS books ;


### 库的修改 ###

- 语法

        RENAME DATABASE books TO 新库名;

- 举例：更改库的字符集

        ALTER DATABASE books CHARACTER SET gbk;


### 库的删除 ###

- 语法

        DROP DATABASE IF EXISTS books;




##表的管理



### 表的创建 ###

- 语法

        create table 表名(
        	列名 列的类型【(长度) 约束】,
        	列名 列的类型【(长度) 约束】,
        	列名 列的类型【(长度) 约束】,
        	...
        	列名 列的类型【(长度) 约束】
        
        
        )



- 案例
        
        CREATE TABLE book(
        	id INT,#编号
        	bName VARCHAR(20),#图书名
        	price DOUBLE,#价格
        	authorId  INT,#作者编号
        	publishDate DATETIME#出版日期
        );
        
        DESC book;
 



### 2.表的修改


- 语法
     
           alter table 表名 
           add|drop|modify|change column 列名 【列类型 约束】;
        
- 修改表名

        ALTER TABLE author RENAME TO book_author;



- 修改列名

        ALTER TABLE book CHANGE COLUMN publishdate pubDate DATETIME;


- 修改列的类型或约束

        ALTER TABLE book MODIFY COLUMN pubdate TIMESTAMP;

- 添加新列

        ALTER TABLE author ADD COLUMN annual DOUBLE; 

- 删除列

        ALTER TABLE book_author DROP COLUMN  annual;





### 表的删除
    

- 语法

        DROP TABLE IF EXISTS book_author;   
        SHOW TABLES;


- 通用的写法
        
        DROP DATABASE IF EXISTS 旧库名;
        CREATE DATABASE 新库名;
        
        
        DROP TABLE IF EXISTS 旧表名;
        CREATE TABLE  表名();



### 表的复制


- 仅仅复制表的结构
        
        CREATE TABLE copy LIKE author;

- 复制表的结构+数据
     
        CREATE TABLE copy2 
        SELECT * FROM author;

- 只复制部分数据
        
        CREATE TABLE copy3
        SELECT id,au_name
        FROM author 
        WHERE nation='中国';


- 仅仅复制某些字段

        CREATE TABLE copy4 
        SELECT id,au_name
        FROM author
        WHERE 0;












