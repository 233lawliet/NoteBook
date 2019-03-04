## 约束 ##


### 含义 ###

- 对数据进行的一种约束，用于保证数据的准确性和可靠性。

### 分类 ###

- 非空：not null  
- 默认：default
- 主键约束：primary key （具有唯一性（但是可以多字段构造一个主键），不允许为空）
- 外键约束：foreign key (保证字段的准确性)
- 唯一约束：unique  (保证字段的唯一性，允许为空)
- 检查约束：check (mysql不支持)


### 约束的分类 ###

- 列级约束
    - 六大约束都支持
    - 外键约束支持，无效果
- 表级约束
    - 除了非空和默认，其他都不支持


### 添加约束 ###


- 语法结构
    
        create table 表名(
          字段  类型  列级约束
          ……
          表级约束
        )

- 举例
    - 列级约束

            create table stuinfo(
             id int primary key,
             stuName varchar(20) not null,
             gender char check(gender="男" or gender ="女"),
             seat int unique,
             age int default 18,
             majorid int references major(id)
            
            )

    - 表级约束

            /*
            
            语法：在各个字段的最下面
             【constraint 约束名】 约束类型(字段名) 
            */

            DROP TABLE IF EXISTS stuinfo;
            CREATE TABLE stuinfo(
            	id INT,
            	stuname VARCHAR(20),
            	gender CHAR(1),
            	seat INT,
            	age INT,
            	majorid INT,
            	
            	CONSTRAINT pk PRIMARY KEY(id),#主键
            	CONSTRAINT uq UNIQUE(seat),#唯一键
            	CONSTRAINT ck CHECK(gender ='男' OR gender  = '女'),#检查
            	CONSTRAINT fk_stuinfo_major FOREIGN KEY(majorid) REFERENCES major(id)#外键
            	
            );

  
- *通用写法

        CREATE TABLE IF NOT EXISTS stuinfo(
        	id INT PRIMARY KEY,
        	stuname VARCHAR(20),
        	sex CHAR(1),
        	age INT DEFAULT 18,
        	seat INT UNIQUE,
        	majorid INT,
            FOREIGN KEY(majorid) REFERENCES major(id)
        
        );



- 查看约束


        desc 表名;



### primary key 和union的对比 ###

- union和primary key可以组合使用，但是不推荐
        
        union(字段1,字段2)
        primary key(字段1,字段2)

- 都具有唯一性
- primary key要求不可以为空，union要求可以为null
- 一个表一个primary key，union必须唯一

### 关于外键 ###

- 引用：foreign key(majorid) references major(id)
- 要求在从表设置外键关系
- 从表的外键列的类型和主表的关联列的类型一致或者兼容，名字可以不同
- 主表一般是一个独立的key (主键或者union)
- 插入数据时，必须先插入主表，再插入从表；删除时必须先删除从表，在删除主表


### 修改表时添加约束 ###



- 添加列约束

        alter table 表名 modify 字段 字段类型  约束；
        
        约束=not null |primary key|default 18

- 添加表级约束

        alter table 表名 add 约束
        
        约束=union(字段)|主键|外键引用

### 删除表的约束 ###

- 原理：重新更改字段或者表的约束

- 删除列约束

        alter table 表名 modify 字段 字段类型  ；
 
- 删除表级约束

        alter table 表名 drop 约束
        
        约束=index|primary key|foreign key 字段


## 标识列 ##

- 又称作自增长列


### 创建和插入 ###

- 创建表示添加自增 

        create table if not exists 表名；
        create table 表名(
          id  int  primary key auto_increment 
          ……
        )
        

- 数据插入
    - F1

            insert into 表(id,name) values(null,‘lutong’);

    - F2

            insert into 表(name) values("路通");


- 标识列的修改

        alter table 表名 modify 表名 类型 约束 自增；

### 特点 ###

- 字段不一定为primary key，但必须时key。例如：union
- 一个表最多一个标识列（自增）
- 表示列的类型只能是数值


### 修改增加步长 ###
        
        set auto_increment_increment =3
        //所有的自增表：自增步长=3




1