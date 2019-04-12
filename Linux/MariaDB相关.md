## 安装及相关配置 ##

### 安装 ###

    yum update 
    yum install mariadb

### 启动和开机自启 ###

    systemctl start mariadb
    systemctl enable mariadb

### 安全设置 ###

    mysql_secure_installation
    //主要取消掉root的远程登录

## 字符集设置 ##

### 查看字符集 ###

    show variables like "%character%";
    

### 修改字符集 ###

    修改/etc/my.cnf.d/server.cnf

    [mysqld]
    character_set_server=utf8mb4
    collation-server=utf8mb4_unicode_ci 
    init_connect='SET NAMES utf8mb4'
    skip-character-set-client-handshake=true

## 用户管理 ##



### 添加用户 ###

    create user '名字'@'(%|localhost|指定ip)' identified by 密码;
    //user+host构成了用户名
    //%通配


### 重命名 ###

    rename user '名字'@'(%|localhost|指定ip)'  to  '新名字'@'(%|localhost|指定ip)';

### 删除用户 ###

    drop user '名字'@'(%|localhost|指定ip)';

### 修改密码 ###

    update mysql.user set password=password('llt19970518')where user ='lilutong';
    //调用了加密函数

### 修改生效 ###

    flush privileges;


## 权限管理 ##


### 权限赋予 ###

    grant all privileges on *.* to 'lutong'@'%';
    //privileges可无

### 部分授权 ###

    grant insert,update,delete,select on *.* to 'lutong'@'%';

### 查看权限 ###

    show grants for 'lutong'@'%';












-