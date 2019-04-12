
## 安装anaconda ##
### 下载Anaconda ###



wget https://repo.continuum.io/archive/Anaconda3-5.3.0-Linux-x86_64.sh


bash ./Anaconda3-5.3.0-Linux-x86_64.sh

（过程报错缺少zip2包:yum install -y bzip2）

### Anaconda生效 ###


source ~/.bashrc


## 安装conda的python ##


### 创建3.6的空间（并且安装相应的python3.6） ###

conda create --name python36 python=3.6

### 激活3.6的空间 ###

source activate python36

### 返回系统的Python ###

source deactivate python36



### 删除对应的空间 ###

conda remove --name python36

## 安装jupyter nootbook ##

### 下载pip ###

yum -y install python-pip


### 安装多种开发工具+python相关开发 ###

yum -y groupinstall "Development Tools"

yum -y install python-devel

### 安装虚拟开发环境+创建环境+激活环境 ###

pip install virtualenv

virtualenv venv

source venv/bin/activate

### 安装jupyter ###

pip install jupyter


### 创建相关存储目录 ###

mkdir  -p  /data/jupyter/root


### 获取设置密码的加密信息 ###

python -c "import IPython;print( IPython.lib.passwd());"


（自己的加密：sha1:add970c333db:44a4cd78451386b0a7924b2901cb89ee52a1d50d


### 生成默认的配置 ###

jupyter notebook --generate-config --allow-root



### 添加用户的配置信息 ###


root/.jupyter_notebook_config.py下


#自己添加
c = get_config()
c.NotebookApp.ip='0.0.0.0'
c.NotebookApp.allow_root = True
c.NotebookApp.open_browser = False
c.NotebookApp.port = 8888
c.NotebookApp.password = u'sha1:add970c333db:44a4cd78451386b0a7924b2901cb89ee52a1d50d'
c.ContentsManager.root_dir = '/data/jupyter/root'


### 启动测试 ###

jupyter notebook




### 设置后台启动+log输入日志 ###


nohup jupyter notebook > /data/jupyter/jupyter.log 2>&1 &








-