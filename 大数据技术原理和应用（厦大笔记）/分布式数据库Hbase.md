## 4.1Hbase简介 ##

### 背景 ###

- BigTable（gogole存储爬虫网页）的开源实现

    ![](https://i.imgur.com/8WyMfL0.png)



### 架构基础GFS ###

  ![](https://i.imgur.com/VvS9kvI.png)


### 优势 ###

- 扩展性好
- 良好的性能
- 高可靠
- 可伸缩

    ![](https://i.imgur.com/UfsSwtf.png)
    ![](https://i.imgur.com/3luOjm8.png)


### HBase和BigTable对应关系 ###

![](https://i.imgur.com/w4Zb5GT.png)

### Hbase设计原因 ###


![](https://i.imgur.com/djx2zeS.png)
    
- 传统关系数据库优化方案（仍满足不了）
    - 主从复制
    - 分库
    - 分表


### Hbase和传统数据库的关系数据 ###

  ![](https://i.imgur.com/leYEqE4.png)

- Hbase存储都是字符串
- Hbase不需要连表
- Hbase不要复杂索引
     ![](https://i.imgur.com/ToKeI1W.png)
- Hbase保留旧的版本
- 水平可扩展


### 访问方式 ###

![](https://i.imgur.com/99Q2tkB.png)