---
layout: post
title: "生物信息学软件安装与管理"
date:   2019-05-22
tags: [生物信息学]
comments: true
author: Si Yangming
toc : true
---

> 软件安装分成以下几类：
针对不同平台（windows、Linux、Mac）编译好的二进制程序；
各种编程语言（perl,R,python,java,matlab,ruby,C）的源代码，主要是安装程序依赖的模块；
专门用于包管理的命令apt-get，yum，bioconda，cpan，cran，pip。

## Linux系统环境配置
```shell
## CentOS系统软件管理命令
sudo yum search	#查询gcc/java/python
sudo yum install	#安装软件
sudo yum -y install	#非交互式安装
sudo yum	remove	#移除软件
sudo yum update	#升级软件
sudo yum list	(all installed recent updates)
sudo yum infor 
```
## 命令行模式生物软件安装
下载软件安装包，进行解压缩：
1、运行./configure检查配置及依赖关系，生成Makefile
2、make check编译前检查
3、make 编译
4、make install安装
注意阅读软件包内README INSTALL文件说明！！
## bioconda
Anaconda 安装包
https://mirrors.ustc.edu.cn/anaconda/archive/
https://bioconda.github.io/
https://conda.io/miniconda.html
https://anaconda.org/
查询bioconda可以安装的软件列表：
https://anaconda.org/bioconda/repo
https://bioconda.github.io/conda-recipe_index.html
```shell
## conda的安装
wget  https://repo.continuum.io/miniconda/Miniconda2-latest-Linux-x86_64.sh
bash Miniconda2-latest-Linux-x86_64.sh

export PATH="$PATH:$HOME/miniconda2/bin"
source  ~/.bashrc

## bioconda channel配置
conda config --add channels https://mirrors.ustc.edu.cn/anaconda/pkgs/r/
conda config --add channels https://mirrors.ustc.edu.cn/anaconda/pkgs/main/
conda config --add channels  https://mirrors.ustc.edu.cn/anaconda/pkgs/free/
conda config --add channels https://mirrors.ustc.edu.cn/anaconda/cloud/conda-forge
conda config --add channels https://mirrors.ustc.edu.cn/anaconda/cloud/bioconda/
conda config --set show_channel_urls yes
## 显示当前所有channel
conda config --get
```
查看当前镜像配置文件
```shell
cat ~/.condarc
```
condarc内容如下：
```markdown
channels:
  - https://mirrors.ustc.edu.cn/anaconda/cloud/peterjc123/
  - https://mirrors.ustc.edu.cn/anaconda/cloud/pytorch/
  - https://mirrors.ustc.edu.cn/anaconda/cloud/menpo/
  - https://mirrors.ustc.edu.cn/anaconda/cloud/bioconda/
  - https://mirrors.ustc.edu.cn/anaconda/cloud/msys2/
  - https://mirrors.ustc.edu.cn/anaconda/cloud/conda-forge/
  - https://mirrors.ustc.edu.cn/anaconda/pkgs/main/
  - https://mirrors.ustc.edu.cn/anaconda/pkgs/free/
  - https://mirrors.ustc.edu.cn/anaconda/pkgs/mro/
  - https://mirrors.ustc.edu.cn/anaconda/pkgs/pro/
  - https://mirrors.ustc.edu.cn/anaconda/pkgs/r/
  - bioconda
  - r
  - defaults
  - conda-forge
show_channel_urls: true
```
conda的包管理命令
```shell
# 搜索特定软件包
conda search package-name  
# 新建一个叫py3的环境，这个环境里面我们需要指定使用python3 
conda create -n py3 python=3.5.3
# 激活虚拟环境 
source activate py3   
# 关闭虚拟环境
source deactivate  
# 列出已经创建的虚拟环境  
conda info --evns

## 安装star软件
conda install star
# 卸载star
conda uninstall star
conda remove star

# 以下代码可以列出$HOME/miniconda2中的安装的所有程序
conda list
# 新环境建成后需使用以下代码激活该环境：
source activate py3
# 注意：这时会在命令行提示符前有一个,表示已经在这个环境下
# 再使用conda install在这个新环境下装软件
conda install samtools=0.1.19
source deactivate
```

除了以上简单的安装外，还可以建立工作空间将几个相应的软件装到一起，例如比对的软件装在名为aligners的环境下：
```shell
conda  create  -n  aligners  bwa  bowtie  hisat2
$HOME/miniconda2/envs/aligner/bin/hisat2
```
用conda安装perl包
```shell
# 首先建议在bioconda官网https://anaconda.org/bioconda/repo搜索一下相关perl包，搜索的格式如perl-archive-extract（perl-父模块全小写-子模块全小写）这时会出现以下搜索结果，表明bioconda中有这个perl包。
# 可直接在终端使用以下命令
conda install perl-archive-extract
# 当然搞清楚了perl安装包的命名规则后也可以直接在终端用以下命令搜索
conda search perl-archive-extract
conda update --all
conda install perl-html-entities
conda install perl-lwp-simple
```
## Python模块安装
[http://www.runoob.com/python/python-tutorial.html](http://www.runoob.com/python/python-tutorial.html)
[https://packaging.python.org/pip](https://packaging.python.org/pip)
why use pip over easy_install：
[https://stackoverflow.com/questions/3220404/why-use-pip-over-easy-install](https://stackoverflow.com/questions/3220404/why-use-pip-over-easy-install)
**使用豆瓣上的PyPi源**
```
# Linux/Mac用户修改
# $HOME/.config/pip/pip.conf
[global]
timeout = 60
index-url = https://pypi.doubanio.com/simple

## 注意： 如果使用http链接，需要指定trusted-host参数

[global]
timeout = 60
index-url = http://pypi.douban.com/simple
trusted-host = pypi.douban.com
```
阿里云镜像配置
```
mkdir ~/.pip 
vim ~/.pip/pip.conf
[global]
index-url = http://mirrors.aliyun.com/pypi/simple/
[install]
trusted-host = mirrors.aliyun.com
# 注意事项：
# http://mirrors.aliyun.com/pypi/simple/中的simple目录必须有。
# --no-cache-dir重新下载安装包，而不是使用缓存包。
# trusted-host = mirrors.aliyun.com一定要加上这行，否则会报错。
```
# pip国内镜像源。
```
阿里云 http://mirrors.aliyun.com/pypi/simple/
中国科技大学 https://pypi.mirrors.ustc.edu.cn/simple/
豆瓣 http://pypi.douban.com/simple
Python官方 https://pypi.python.org/simple/
v2ex http://pypi.v2ex.com/simple/
中国科学院 http://pypi.mirrors.opencas.cn/simple/
清华大学 https://pypi.tuna.tsinghua.edu.cn/simple/
```
在用户目录安装软件，不需要root 权限
```
# 在用户目录安装软件，不需要root 权限

pip install --user <package>
# 搜索package
pip search biopython
# 安装特定版本的package，版本号可以从search的结果中找到
pip install biopython=1.69
# 卸载package
pip uninstall biopython
# 导出已安装的包信息
pip biopython > requirements.txt
# 其他使用方法可以参考pip的帮助说明
pip -h

pip install rpy2 --user 
pip install Numpy --user 
pip install Pandas --user 
pip install BioPython --user 
pip install matplotlib --user 
pip install PySQLite --user 
pip install cnvkit --use
```
virtualenv也是Python包，可以直接用 pip来进行安装。现在可以用它在电脑上创建不同的虚拟环境了，各个虚拟环境互不干扰，而且对原有的环境不会造成影响，删除环境是直接把对应的目录删除即可。
