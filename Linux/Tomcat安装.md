### yum查看tomcat的所有版本 ###

    yum search tomcat
    //只有一个版本

    //最重要的Tomcat的文件将位于/usr/share/tomcat 。 
    //如果你已经有了，你想运行一个Tomcat应用程序，你可以将它放在/usr/share/tomcat/webapps的目录，配置Tomcat。
    //并重新启动Tomcat服务。但在本教程中，我们将安装一些其他软件包，帮助您管理Tomcat应用程序和虚拟主机。

### 安装浏览器管理界面 ###

yum install tomcat-webapps tomcat-admin-webapps


### 相关的操作 ###

- systemctl enable tomcat
- systemctl start tomcat
- systemctl stop tomcat






























-