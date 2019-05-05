
## Nginx服务器安装 ##

### 环境准备 ###


yum -y install gcc pcre pcre-devel zlib zlib-devel openssl openssl-devel



### 下载nginx ###


wget https://nginx.org/download/nginx-1.15.9.tar.gz


tar -zxvf nginx-1.15.9.tar.gz

cd nginx-1.15.9

### 设置配置参数 ###




./configure \
--prefix=/usr/local/nginx \
--pid-path=/var/run/nginx/nginx.pid \
--lock-path=/var/lock/nginx.lock \
--error-log-path=/var/log/nginx/error.log \
--http-log-path=/var/log/nginx/access.log \
--with-http_gzip_static_module \
--http-client-body-temp-path=/var/temp/nginx/client \
--http-proxy-temp-path=/var/temp/nginx/proxy \
--http-fastcgi-temp-path=/var/temp/nginx/fastcgi \
--http-uwsgi-temp-path=/var/temp/nginx/uwsgi \
--http-scgi-temp-path=/var/temp/nginx/scgi


### 编译安装 ###

make  
make install



### 启动 ###

cd /usr/local/nginx/sbin

./nginx
(报错缺少文件：mkdir -p /var/temp/nginx）


### 关闭 ###


./nginx -s stop





## nginx服务配置 ##

### 添加用户 ###

useradd  ftpuser

psword ftpuser



### 映射文件 ###
vim  /usr/local/nginx/conf/nginx.conf



location{
    /访问路径
    root /home/ftpuser
}

### 修改pid ###

配置文件（/sur/local/nginx/config）下

将注释放开，并修改为：pid    /usr/local/nginx/logs/nginx.pid;

在 /usr/local/nginx 目录下创建 logs 目录：mkdir /usr/local/nginx/logs



### 添加到系统变量 ###


在/etc/profile 中加入：

export NGINX_HOME=/usr/local/nginx

export PATH=$PATH:$NGINX_HOME/sbin

执行 source /etc/profile ，使配置文件生效。


### nginx启动 ###

nginx

### nginx关闭 ###
ps -ef | grep nginx


　　从容停止   kill -QUIT 主进程号

　　快速停止   kill -TERM 主进程号

　　强制停止   kill -9 nginx

-