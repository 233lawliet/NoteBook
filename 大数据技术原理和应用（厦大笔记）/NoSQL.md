## 5.1NOSQL概述 ##



Not Only SQL


### 特点 ###


- 灵活的可扩展性
- 灵活的数据模型
- 和云计算的充分结合


### 传统数据的缺点 ###

- 无法满足海量数据的访问
- 无法满足高并发的需求
      ![](https://i.imgur.com/dk2SuNG.png)
- 无法高扩展性
    - 传统数据库
        - 采用了主从复制（写主，从读）
            - 可以同步，可以异步
            - 读写分离
        - 分库
            ![](https://i.imgur.com/xNaxZQW.png)  
        ![](https://i.imgur.com/tmLHKFE.png)

- 高可用性
    - 传统数据库的范式
        - 为了降低冗余，需要大量的连接
        - 连接操作，极为耗时

 
       
## 5.2NoSQL和关系数据库的比较 ##

### 比较 ###

- 数据库原理
    ![](https://i.imgur.com/IBys3R4.png)

- 数据规模扩展性
    ![](https://i.imgur.com/yqDZHir.png)
    - 硬件（cpu,硬盘）


- 数据模型

    ![](https://i.imgur.com/6cXSNT3.png)

- 查询效率

    ![](https://i.imgur.com/Hl6VS1R.png)

- 一致性

    ![](https://i.imgur.com/pTCZaec.png)


- 数据完整性

    ![](https://i.imgur.com/KywmXAL.png) 

- 可扩展性

    ![](https://i.imgur.com/99b5hOd.png)

- 可用性

    ![](https://i.imgur.com/dO2Pwlm.png)

- 标准化方面

    ![](https://i.imgur.com/G3VGw8a.png)

- 技术支持

    ![](https://i.imgur.com/TrlN8iT.png)

- 可维护性

    ![](https://i.imgur.com/mT3D3YQ.png)

- 应用场景

![](https://i.imgur.com/O18Y2ur.png)


### 关系数据库的优势 ###



![](https://i.imgur.com/atu4V2B.png)

### 关系数据库的劣势 ###
![](https://i.imgur.com/MQbW6Ed.png)


### NOSQL数据库的优劣势 ###

- ![](https://i.imgur.com/zOYB4gC.png)



## 5.3NOSQL数据库的四大类型 ##


![](https://i.imgur.com/Hb9IczX.png)


![](https://i.imgur.com/Qevc8ki.png)

### 键值数据库 ###

![](https://i.imgur.com/CCnYlpr.png)

![](https://i.imgur.com/M21qbwU.png)

### 列族数据库 ###

![](https://i.imgur.com/YVu7L3N.png)

### 文档数据库 ###

- 举例
    ![](https://i.imgur.com/4ZJOd6O.png)


![](https://i.imgur.com/gYTL3Go.png)

### 图形数据库 ###


![](https://i.imgur.com/mMiPDpx.png)

### 数据库比喻 ###

![](https://i.imgur.com/OjmB0Fj.png)


## 5.4NoSQL的三大基石 ##

### CAP ###

![](https://i.imgur.com/itGz792.png)
![](https://i.imgur.com/c0frBI6.png)




#### 不同产品设计的原则 ####

![](https://i.imgur.com/G0Uk2Q3.png)

### BASE ###

![](https://i.imgur.com/cEGkK8w.png)

![](https://i.imgur.com/LNletCX.png)

#### 最终一致性分类 ####


![](https://i.imgur.com/ihGH4vP.png)

![](https://i.imgur.com/drTAXfR.png)



## 5.5NOSQL->NewSQL ##

### 数据库的发展 ###

- 原始

![](https://i.imgur.com/AumzBi0.png)


- 现代

![](https://i.imgur.com/3dCXgeO.png)


### 数据库分类 ###

![](https://i.imgur.com/gAwWZEo.png)




## 5.6文档数据库MongoDB ##


### 简介 ###

![](https://i.imgur.com/N70J97S.png)


![](https://i.imgur.com/IeS6bmu.png)






### 特点 ###

![](https://i.imgur.com/wOplWPS.png)


### MongoDB和传统对比 ###

![](https://i.imgur.com/PMhXAGm.png)

- 传统数据库的连表

![](https://i.imgur.com/ypmsXhl.png)


- mongoDB不需要连表

![](https://i.imgur.com/vo9Z9Eu.png)


### 集合 ###

![](https://i.imgur.com/CrI5M6A.png)




- 