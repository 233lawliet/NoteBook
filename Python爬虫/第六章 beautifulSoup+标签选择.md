## BeautifulSoup ##


### window国内源 ###

#### window ####

1. 地址栏输入：%appdata%
2. 新建文件夹：pip
3. 添加文件pip.ini
4. 文件内

        [global]
        timeout = 6000
        index-url=https://mirrors.aliyun.com/pypi/simple/
        trusted-host=mirrors.aliyun.com
        

#### linux ####

1. cd ~
2. mkdir /.pip
3. vi ~/.pip/pip.conf


        [global]
        index-url = http://mirrors.aliyun.com/pypi/simple/
        [install]
        trusted-host=mirrors.aliyun.com




### 使用说明 ###

#### 作用 ####
- 选择器(类似jquery)
#### 引用 ####
- from  bs4 import BeautifulSoup
#### 使用方式 ####

- 可以将一个html文档，转化成指定的对象，然后通过对象的方法，找找指定的内容。

#### 引用 ####
1. soup=BeautifulSoup(open("本地文件",encoding='utf8'),"lxml")
2. soup=BeautifulSoup("字节类型 或者 字符类型","lxml")

#### 返回 ####
- <class 'bs4.BeautifulSoup'>
- soup是一个bs.Beautiful类型。对外显示为字符串

## soup查找网页内容 ##

### 1.常用函数 ###

#### 标签获取 ####

- f1：标签方式
- f2：函数方式find()  #**可以添加属性限制条件

        ol=soup.ol
        ol2=soup.find('ol')
        ol2=soup.find('ol',class_="test")


#### 属性获取 ####

- f1:通过标签获取
- f2:通过函数获取  #限制查找


- ['属性']
- .attr获取的是所有属性
- .attr['属性']

        print(soup.ol["class"])
        print(soup.ol.attrs["class"])
        print(soup.ol.attrs)
        
        p=soup.find('p',class_="testClass")
        print(p['class'],p.attrs,p.attrs['class'])

#### 内容获取 ####

- 返回标签内的信息

- .text   //子类还有标签，递归返回所有标签的内容
- .get_text()  //子类还有标签，递归返回所有标签的内容
- .string   //子类还有标签，返回None  没有返回内容


        print(soup.ol.text)
        print(soup.ol.get_text())
        print(soup.ol.string)


 
#### 查找所有函数find_all() ####

- 返回类型：<class 'bs4.element.ResultSet'>
- 可用for遍历
 
- find_all("标签") 
- find_all("标签",属性='属性内容')  //查找具体属性的标签
- find_all("标签",limit=2) 查找的前n个 
        
        ps=soup.find_all('p')
        ps=soup.find_all('p',class_="testClass")
        ps=soup.find_all('p',class_="testClass",limit=1)

### 2.选择器select ###


- 返回的是：list   查找id也是

#### 常见选择器 ####


- id选择器           ##
- 类选择器           #.
- 标签选择器           #标签
- 组合选择器            #逗号
- 层级选择器    （后代  #空格
- 下一级的选择器 （孩子 #>
- 属性选择器       #[属性=‘属性’]


    print(type(soup.select('#feng')))
    print(soup.select("ol>li>p"))




### 3.xpath ###

略
 




