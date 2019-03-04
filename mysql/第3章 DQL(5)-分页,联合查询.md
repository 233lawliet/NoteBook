## 分页查询 ##


### 语法 ###

- limit offset,size


        select 字段
        from 表1
        连接 join 表2
        on 连接条件
        where 筛选条件
        group by 分组字段
        having 分组后的筛选
        order by 排序字段
        limit offset,size;
        //offset起始索引，size长度
### 举例 ###
    - 查看前五条

            select * from employees limit 0,5
            或select * from employees limit 5
             
    - 查看第11条-25条信息
            
            select * from employees  limit 11,25-11+1;            

### 特点 ###

- limit语句放在查询语句的最后
- 公式：offset,size=(page-1)*size,size
- 语法上：前n条的起始offset可以省略


### 查询顺序 ###


    select 字段                      7
    from 表1                         1
    连接 join 表2                     2
    on 连接条件                       3
    where 筛选条件                    4
    group by 分组字段                 5
    having 分组后的筛选                6
    order by 排序字段                 8
    limit offset,size;               9
  



## union联合查询 ##

### 含义 ###

- 将多个语句查询结果合并成一个新的结果


### 语法结构 ###

    查询语句1
    union
    查询语句2


### 举例： ###


    select * form employees where name like "%a%" and department_id>90;
     //等价于    
    select * from employees where name like "%a%"
    union
    select * from employees where department_id>90


### 应用场景 ###

- 在查询多个表时，其之间没有直接的连接关系，单查询信息一致时
