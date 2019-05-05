### 安装vsftpd ###

systemctl start vsftpd.service


### 相关操作 ###



systemctl start vsftpd

systemctl stop vsftpd

systemctl enable vsftpd


systemctl status vsftpd



### 检测vsftpd是否开启了 ###

ps -e|grep vsftpd 


### 配置vsftpd ###



        #打开配置文件
        vim /etc/vsftpd/vsftpd.conf
  
        #显示行号
        :set number
        
        #修改配置 12 行
        anonymous_enable=NO
        
        #修改配置 33 行
        anon_mkdir_write_enable=YES
        
        #修改配置48行
        chown_uploads=YES
        
        #修改配置72行
        async_abor_enable=YES
        
        #修改配置82行
        ascii_upload_enable=YES
        
        #修改配置83行
        ascii_download_enable=YES
        
        #修改配置86行
        ftpd_banner=Welcome to blah FTP service.
        
        #修改配置100行
        chroot_local_user=YES
        
        #添加下列内容到vsftpd.conf末尾
        use_localtime=YES
        listen_port=21
        idle_session_timeout=300
        guest_enable=YES
        guest_username=vsftpd
        user_config_dir=/etc/vsftpd/vconf
        data_connection_timeout=1
        virtual_use_local_privs=YES
        pasv_min_port=40000
        pasv_max_port=40010
        accept_timeout=5
        connect_timeout=1
        allow_writeable_chroot=YES





### 创建用户列表 ###


    
    #创建编辑用户文件
    vim /etc/vsftpd/virtusers
    #第一行为用户名，第二行为密码。不能使用root作为用户名 
    
    vsftpuser
    123456


### 生成用户数据 ###

    db_load -T -t hash -f /etc/vsftpd/virtusers /etc/vsftpd/virtusers.db
    
    #设定PAM验证文件，并指定对虚拟用户数据库文件进行读取
    
    chmod 600 /etc/vsftpd/virtusers.db 


### 修改 /etc/pam.d/vsftpd 文件 ###


    # 修改前先备份 

    cp /etc/pam.d/vsftpd /etc/pam.d/vsftpd.bak
    
    vi /etc/pam.d/vsftpd
    #先将配置文件中原有的 auth 及 account 的所有配置行均注释掉
    auth sufficient /lib64/security/pam_userdb.so db=/etc/vsftpd/virtusers 
    account sufficient /lib64/security/pam_userdb.so db=/etc/vsftpd/virtusers 
    
    # 如果系统为32位，上面改为lib





### 新建系统用户vsftpd，用户目录为/home/vsftpd ###


    #用户登录终端设为/bin/false(即：使之不能登录系统)
    useradd vsftpd -d /home/vsftpd -s /bin/false
    chown -R vsftpd:vsftpd /home/vsftpd



### 建立虚拟用户个人配置文件 ###




    
    mkdir /etc/vsftpd/vconf
    cd /etc/vsftpd/vconf
    
    #这里建立虚拟用户leo配置文件
    touch vsftpuser
    
    #编辑leo用户配置文件，内容如下，其他用户类似
    vim vsftpuser
    
    local_root=/home/vsftpd/vsftpuser/
    write_enable=YES
    anon_world_readable_only=NO
    anon_upload_enable=YES
    anon_mkdir_write_enable=YES
    anon_other_write_enable=YES
    
    
    #建立leo用户根目录
    mkdir -p /home/vsftpd/vsftpuser/



### 重启vsftpd ###
    systemctl restart vsftpd.service








-