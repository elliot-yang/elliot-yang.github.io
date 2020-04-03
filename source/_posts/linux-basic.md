---
title: linux常用命令
date: 2019-03-18 15:12:58
tags:
    - linux
---

经常在Linux服务器上操作，记录一些常用的以及不常用的命令方便以后查阅。  


<!--more-->


### 常用命令  

#### 查找命令`find`  
```shell
# 查找当前目录下所有后缀为.log的文件  
find . -name "*.log"  
```  

#### 查看文件列表`ls`  
```shell
# 列表形式显示文件
ls -al
# 效果同上
ll
# 列表形式显示所有文件，包括隐藏文件
ls -al
# 列表显示文件，并筛选出包含 xx 的文件
ll | grep xx
```  

#### 输出文件内容`cat,tail`  
```shell
# 直接输出文件全部内容
cat xxx.log
# 输出文件末位的几行内容
tail xxx.log
# 输出文件末尾几行内容并且持续更新,一般用来看日志
tail -f xxx.log
```  

#### 编辑文件`vi`  
```shell
vi xxx.config
```  

#### 显示当前路径`pwd`  

#### 切换用户`su`  
```shell
# 切换到web用户，但是环境变量不会重载
su web
# 切换到web用户，并且环境变量也重载
su - web
```  

#### 复制文件`cp`  
```shell
# 复制单个文件
cp source target
# 复制文件夹以及内部所有文件
cp -r sourceDir targetDir
```  

#### 磁盘使用情况`du`  
```shell
# 显示各个挂载点的磁盘占用情况,-h human readable以人类可读方式展示
du -h
# 显示某个目录磁盘占用情况
du -sh xxxDir
```  

#### 服务器之间复制文件`scp`  
```shell
# 复制到远程服务器的/path目录下, -p保留源文件的修改时间和访问权限
scp -p xxxFile user@host:/path/xxxFile
```  

#### 解压缩`unzip`,`tar`
```shell
# 直接解压zip文件
unzip xxx.zip
# 解压zip文件到指定目录
unzip xxx.zip -d /path/xxx
# 解压tar
tar -xvf xxx.tar
# 解压tar.gz, 带有gzip属性的压缩包
tar -zxvf xxx.tar.gz
# 解压tar.bz2, 带有compress属性的压缩包
tar -jxvf xxx.tar.bz2
```  

#### 压缩`zip`,`tar`
```shell
# 压缩当前目录下所有文件到xxx.zip, -r 表示递归所有子目录
zip -r xxx.zip
# 压缩xxx目录到当前目录的zip文件中
zip -r xxx.zip /path/xxx
# 压缩tar
tar -czvf xxx.tar xxx
```  

#### 查找文件内容`sed`  
```shell
# 找出xxx.log中包含2020-03-26的内容并打印到控制台, 可以结合重定向`>`命令输出到文件中方便查看日志
sed -n '/2020-03-26/p' xxx.log
# 找出从2020-03-20到2020-03-26的内容
sed -n '/2020-03-20/,/2020-03-26/p' xxx.log
```  

现在用到的命令也没有很多，后续还会继续补充。

### 总结  
Linux命令虽然比较复杂，但是常用的不多，有些命令用的较少但功能也比较强大，还是平时需要多使用才能记住。更多的命令详解可以参照[linux命令详解](https://www.runoob.com/linux/linux-command-manual.html)