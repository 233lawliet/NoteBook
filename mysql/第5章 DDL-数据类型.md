# DDL(Data Definition Language) #

### 常见的数据类型 ###


- 数值型
    - 整型
    - 小数
        - 定点型
        - 浮点型
- 字符型
    - 较短的文本：char,varchar
    - 较长的文本：text,blob(较长的二进制文本)
- 日期型    



## 数值 ##

### 整型 ###

- 类型分类

![](https://i.imgur.com/HHCnlhc.png)


#### 特点 ####

- 默认是，有符号的
    - 设置为无负号类型:unsigned

            create table 表名(
              字段名  int unsigned
            )

- 插入的数值超出临界值，会报out of range 错误，并且插入临界值
- 如果不设置默认的长度，会有默认的长度
- 插入的长度由类型决定，int()的参数表示显示的宽度，由0填充，可以通过zerofill显示


### 浮点型和定点型###


- 类型分类

![](https://i.imgur.com/6Q43hpp.png)
    - 较长的文本
        - text和blob（二进制格式）
- 写法
    - float(M,D)
    - double(M,D)
    - decimal(M,D)


- 特点
    - M,D前者表示小数+整数的长度，D表示小数点后的长度
    - 插入长度超过范围，则插入临界值
    - decimal默认的M,D=(10,0)  float和double则是根据插入值设定
    - 定点型应用于对精度要求高的地方，如：货币运算


### 字符型 ###

- 类型分类

![](https://i.imgur.com/li0fkj5.png)
![](https://i.imgur.com/YMetjDT.png)
![](https://i.imgur.com/4EyqlL4.png)
![](https://i.imgur.com/X6qw6cH.png)
#### char和varchar的区别 ####

- char(M)的M可以省略，默认为1，varchar(M)的M不可以省略
- char空间耗费较高，varchar节省空间
- char的效率更高


### 日期型 ###

- 类型分类

![](https://i.imgur.com/B3hJlQ9.png)

- 特点
    - timestamp受时区的影响
