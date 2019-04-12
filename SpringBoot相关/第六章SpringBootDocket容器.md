## Docket简介 ##

### 简介 ###

- DOcket是一个开源的应用容器引擎



### 核心概念 ###



- docker主机(Host):安装了Docker程序的机器。
- docker客户端(client):客户端通过命令行或者其他工具使用docker.
- docker仓库(registry):用来保存各种打包好的软件镜像，
- docker镜像(images):软件打包好的镜像。
- docker容器(container):容器是独立运行的一个或者一组应用。


### 使用Docker的步骤 ###

1. 安装docker
2. 取docket仓库找到对应的镜像
3. 使用docket运行这个镜像，这个镜像就会产生一个docket容器。
4. 对容器的启动停止==对软件的启动停止，



### docker安装使用 ###

#### 要求 ####

- 内核版本>3.10
- uname -a

1. 内核版本检查
2. 安装docker
    - yum install docker
3. 启动docker 
    - systemctl start docker
    - 检查版本：docker -v
    
4. 设置开机启动

        [root@lilutong ~]# systemctl enable docker
        Created symlink from /etc/systemd/system/multi-user.target.wants/docker.service to /usr/lib/systemd/system/docker.service.

5. docker的停止
    - systemctl stop docker 




### docker常用操作 ###

#### 镜像操作 ####

- docker镜像搜索
    - 取 docker hub中查找相关的镜像
    - cmd: docker search 镜像名字

- docker镜像拉取(下载)
    - 可以拼取全称，而加以简写  
        - 例如：docker pull docker.io/mysql     ->  docker pull mysql 
    - cmd: docker  pull 镜像名字
    - 指定版本需要去docker hub查找，拉取方式：在镜像名称后边+版本号
- 查看已下载的镜像以及镜像id
    - cmd:docker images  
- 镜像删除
    - 通过镜像id删除
    - cmd:docker rmi   镜像id
    
#### 容器操作 ####

- 根据镜像创建容器和运行容器
    - cmd:docker run --name 容器名字 -d 镜像名字：镜像版本
    - 容器名字自己命名，-d后台运行，镜像版本如果是last可以省略
- 查看运行的容器
    - cmd: docker ps
    - 查看所有的容器：docker ps -a
- 启动容器
    - cmd:docker start 容器id
- 停止运行中的容器
    - cmd:docker stop 容器id 
- 删除容器
    - cmd:docker rm 容器id  
    
- 端口映射
    - -p 外机开放端口：镜像对外使用端口
    - 举例： run -d -p 8888:8080 tomcat  //自动创建容器
    - 或使用： docker run --name tomcat -d -p 8080:8080 tomcat
    - 从而支持多个端口，同时运行相同镜像的容器

- 查看日志
    - cmd:docker logs  容器id


#### mysql正确启动方式 ####

docker run --name mysql01 -p 3306:3306 -e  MYSQL_ROOT_PASSWORD=LLTllt19970518 -d mysql





















- 