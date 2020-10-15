---
title: Maven
date: 2020-10-10 16:13:45
tags:
    - java
---
Maven是java开发中常见的编译工具, 拥有很多强大的plugin.
### 安装部分
<!--more-->  
* 准备maven  
    下载：`wget http://mirror.bit.edu.cn/apache/maven/maven-3/3.3.9/binaries/apache-maven-3.3.9-bin.tar.gz`  
    解压：`tar -zxvf apache-maven-3.3.9-bin.tar.gz`  
    移动到/usr/local：`# mv apache-maven-3.3.9 /usr/local/maven3`  

* 添加环境变量：`vim /etc/profile`，在最后添加如下两行  
    ```
    export MAVEN_HOME=/usr/local/maven3
    export PATH=$MAVEN_HOME/bin:$PATH
    ```
* 保存退出后输入命令使配置生效：`source /etc/profile`
* 检验是否安装成功：`mvn -v`

### 编译打包
* 基础  
    执行命令`mvn clean package`即可清理工作空间(target目录),并重新在target目录编译打包工程
* 跳过测试打包
    若是在工程中有单元测试用例,maven在编译打包时会自动执行,若需跳过只需要添加参数maven.test.skip为true即可
    `mvn clean package -Dmaven.test.skip=true`

### 部署
* 基础
    执行命令`mvn deploy`即可, 需要提前在maven的setting.xml中配置好仓库的用户名和密码  
* 存在父子模块的情况  
    1. 父子模块全都要发布, 直接在父目录执行命令`mvn deploy`即可  
    2. 只发布父模块, 在父目录执行命令时添加参数-N即可: `mvn delpoy -N`  