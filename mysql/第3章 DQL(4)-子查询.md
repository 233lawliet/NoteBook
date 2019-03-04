## 子查询 ##


### 含义 ###

    出现在其他语句内部的select语句，称之为子查询或者内查询
    内部镶嵌者查询语句的select语句，升值为主查询或者外查询

### 分类 ###


#### 按照结果集的行列数分类 ####

- 标量子查询（结果集只有一行一列）
- 列子查询（结果集只有一列多行）
- 行子查询（结果集为一行多列）
- 表子查询（结果集为多行多列）

#### 按照子查询的位置分类 ####

- select后边
    - 仅支持标量子查询
- from后边
    - 仅支持表子查询
- where,having后边
    - 支持行子查询
    - 支持列子查询
    - 支持标量子查询
- exists/not exists后边
    - 用于判定结果是否存在
    - 返回结果1或者0
    - 常伴随where的使用
### 特点 ###

- 子查询放在小括号内
- 子查询一般放在条件的右侧
- 标量子查询，搭配着单行操作符：<,>,=,!=,<=,>=
- 列子查询，搭配着多行操作符使用：in,any/some,all

### *where后边的子查询 ###

#### 1.表量子查询(单行子查询) ####

- 举例
    - 一个单行子查询

    
            select *
            from employees
            where salary>(
              select salry 
              from employees 
              where last_name ="Abel"
            )
    - 多个多行子查询


            select last_name,job_id,salary
            from employees
            where job_id=(
              select job_id
              from employees
              where employee_id = 141
            ) and
            where salary>(
              select salary
              from employees
              where employee_id =143
            )



    - 5个组函数子查询

            select last_name,job_id,salary
            from employees
            where salary={
              select min(salary)
              from employees
            }



            select min(salary),department_id
            from employees
            group by department_id
            having min(salary)>(
              select min(salary)
              from employees
              where department_id = 50
            )


#### 2.列子查询(多行子查询) ####

- 操作符：
    - in/not in 等于表中的任意一个
    - any/some 和表中的任意一个进行比较
        - >any(元素和) 等价于：大于min()
    - all 和表中的所有元素进行比较
        - >all(元素和) 等价于：大于max()


- 举例：
    - in的类型
        
            SELECT last_name,employee_id
            FROM employees
            WHERE department_id IN(
            	SELECT  DISTINCT department_id
            	FROM employees
            	WHERE last_name LIKE '%u%'
            );   

    - any的类型（= any 等价 in）

            SELECT last_name,employee_id
            FROM employees
            WHERE department_id = any(
            	SELECT  DISTINCT department_id
            	FROM employees
            	WHERE last_name LIKE '%u%'
            );   

#### 3.行子查询 ####

- 应用少，不一定存在

- 举例：

        //普通子表查询
        select *
        from employees 
        where employee_id=(
          select min(employee_id)
          from employees
        )and salary(
          select max(salary)
          from employees
        )

        //-->行字查修你
        select *
        from employees
        where (employee_id,salary)=(
          select min(employee_id,max(salary))
          from employees
        )
 
        
### select后边的子查询 ###

- 特点
    - 内部仅仅支持标量子查询
- 举例

        //查询每个部门的员工数
        select distinct d.*,(
          select count(*)
          from employees e
          where e.department_id = d.department_id
        ) from departments d;
    

        //查询员工号=102的部门名
        select(
        select department_name
        from departments d
        inner join employees e
        on d.department_id =e.department_id
        where e.employee_id=102
        );


### *from后边的子查询 ###

- 要求子查询的结果充当一张表，必须起别名
- 例子


        //查询每个部门的平均工资和等级
        select ag_dep.*,g.frade_level
        from (
          select avg(salary) ag, department_id
          from employees 
          group by department_id
        ) ag_dep
        inner join job_grades g
        on ag_dep.ag between lowest_sal and highest_sal



### exists后子查询（相关子查询） ###

- 举例
    - where的结合使用

            select department_name
            from deparments d
            where exists(
              select *
              from employees e
              where d.department_id=e.department_id
            )

    - **in可以完全代替**

            select department_name
            from departments
            where department_id in(
             select department_id 
             from employees
            )


    - not exists的使用

            select bo.*
            from boys bo
            where not exitst(
              select boyfriend_id
              from beauty b
              where bo.id=boyfriend_id
            )