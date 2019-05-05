### yum查看可选择版本 ###


yum search java|grep jdk



### 下载jdk ###
    
    yum install java-1.8.0-openjdk 
    
    //装完之后，默认的安装目录是在: /usr/lib/jvm/
    //需要查看文件的名字  用于环境变量的配置

    

### 设置环境变量 ###

vi /etc/profile 



### 添加配置信息 ###

    
    #set java environment
    JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.212.b04-0.el7_6.x86_64
    JRE_HOME=$JAVA_HOME/jre
    CLASS_PATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar:$JRE_HOME/lib
    PATH=$PATH:$JAVA_HOME/bin:$JRE_HOME/bin
    export JAVA_HOME JRE_HOME CLASS_PATH PATH
    


### 生肖环境变量 ###

 source /etc/profile 



















- 