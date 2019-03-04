# TCL(Transaction Controller Language) #




## 事务 ##

### 含义 ###

- 一组或者一个sql语句构成的执行单元
- 要么都执行，要么都不执行


### 特性(ACID) ###

- 原子性(Atomicity):要么发生，要么不发生
- 一致性(Consistency):从一个状态进入另一个状态
- 隔离性(Isolation):事务之间不进行干扰
- 持久性(Durability):事务提交是永久性的，直接改变数据库。


### 事务的创建 ###

#### 隐式的事务 ####

- 含义
    - 没有明显的开启和结束的标记
- 举例
    - insert,delete,update

#### 显式的事务 ####

- 含义
    - 有明显的开启和结束的标记
- 使用前提
    - 设置自动提交为关闭
    - set autocommit =0;
      
### 事务的流程 ###
- 流程

    1. 开启事务
        - set autocommit =0
        - start transaction ;(可选) 
    2. 编写sql语句（select,insert,delete,update）
    3. 结束事务
        - 提交事务:commit;
        - 回滚事务：rollback;

- 举例

        #开启事务
        SET autocommit=0;
        START TRANSACTION;
        #编写一组事务的语句
        UPDATE account SET balance = 1000 WHERE username='张无忌';
        UPDATE account SET balance = 1000 WHERE username='赵敏';
        
        #结束事务
        ROLLBACK;
        #commit;


### 并行事务的问题 ###

#### 原因 ####

- 同时运行多个事务，当这些事务访问数据库中相同的数据时，容易八楼一下问题。

#### 问题 ####

- 脏读：事务1读取了事务2更新但是还未提交的事务，事务2撤销，则事务1读取的数据则是无效的。
- 不可重复读：事务读取了事务2更新前和更新后的字段，字段数值不同。
- 幻读：事务2对数据库进行插入，删除，导致信息条数改变，使前后数据条数不同。

- 问题解决方案（事务隔离级别）
    - oracle支持两种
        - read comitted(默认)
        - serializablle(序列化)
    - mysql支持的级别（四种）
        - 默认是：repeatable read



### 事务的隔离级别 ###


- 查看默认的隔离级别 

    select @@tx_isolation;

- 设置隔离级别
    - read umcommitted
     
            set session transaction isolation level read umcommitted
  
    - read commmitted
    
            set session transaction isolation level read committed

   - repeatable read
   
            set session transaction isolation level repeatable read
   - serializable
    
            set session transaction isolation level serializable

- 设置系统的全局隔离级别

        set global transaction isolation level 级别(四个)

- 对应级别的作用

![](https://i.imgur.com/aRo2r5X.png)


- 隔离级别总结

        事务的隔离级别：
        		  脏读		不可重复读	幻读
        read uncommitted：√		√		√
        read committed：  ×		√		√
        repeatable read： ×		×		√
        serializable	  ×     ×       ×


- 默认的隔离级别
    - mysql中默认 第三个隔离级别 repeatable read
    - oracle中默认第二个隔离级别 read committed




### 关于事务回滚 ###


- savepoint

- 作用
- 使事务能够回滚，不进行断点之后事务内容。

- savepoint 的使用

        SET autocommit=0;
        START TRANSACTION;
        DELETE FROM account WHERE id=25;
        SAVEPOINT a;#设置保存点
        DELETE FROM account WHERE id=28;
        ROLLBACK TO a;#回滚到保存点
        





### 扩展：存储引擎相关 ###

#### 概念 ####

- 在mysql中的数据通过各种不同的技术存储在文件中，


#### 查看数据的存储引擎 ####





- 操作命令

        show engines

- 数据默认的存储引擎：InnoDB
- InnoDB支持事务



























- 