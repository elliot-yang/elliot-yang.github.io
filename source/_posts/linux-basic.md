---
title: linux常用命令
date: 2020-03-18 15:12:58
tags:
    - linux
---
### 前言 

经常在Linux服务器上操作，记录一些常用的以及不常用的命令方便以后查阅。  


<!--more-->


### 常用命令  

1. 查找命令`find`  

```shell
# 查找当前目录下所有后缀为.log的文件  
find . -name "*.log"  
```  

2. 查看文件列表`ls`  

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

3. 输出文件内容`cat,tail`  

```shell
# 直接输出文件全部内容
cat xxx.log
# 输出文件末位的几行内容
tail xxx.log
# 输出文件末尾几行内容并且持续更新,一般用来看日志
tail -f xxx.log
```  

4. 编辑文件`vi`  

```shell
vi xxx.config
```  

5. 显示当前路径`pwd`  

6. 切换用户`su`  

```shell
# 切换到web用户，但是环境变量不会重载
su web
# 切换到web用户，并且环境变量也重载
su - web
```  

7. 复制文件`cp`  

```shell
# 复制单个文件
cp source target
# 复制文件夹以及内部所有文件
cp -r sourceDir targetDir
```  

现在用到的命令也没有很多，后续还会继续补充。

### 总结  
Linux命令虽然比较复杂，但是常用的不多，有些命令用的较少但功能也比较强大，还是平时需要多使用才能记住。