---
title: redis基础
date: 2018-06-12 22:29:05
tags:
    - redis
    - NoSQL
---
### 安装部分
<!--more-->  
环境：Ubuntu 16.04 LTS
1. 先从redis官网上下载redis的源码得到 redis-3.2.8.tar.gz
2. 使用命令解压文件到当前用户的software目录下（目录按照个人习惯定）  
	```sh
	tar -zxvf redis-3.2.8.tar.gz -C ~/software/
	```
3. 使用make命令  
	```sh
	cd ~/software/redis-3.2.8
	make
    ```
4. make完成之后，可执行文件就在src目录下了(可以选择单独将几个redis-*开头的可执行文件复制到单独的目录使用)
	```sh
	cd src 
	# 这样是直接启动redis服务器，使用默认配置
	./redis-server
	# 或者使用指定配置文件的方式启动
	./redis-server ~/software/redis-3.2.8/redis.conf
	```
5. 修改配置文件，redis自带的redis.conf默认是只允许本机访问的，如果需要别的机器访问的话是需要修改一下配置的
    - 启动vi编辑器修改配置文件
	```sh
    vi ~/software/redis-3.2.8/redis.conf
    ```
	- 找到下面这句，这是将redis配置绑定在本机
	```sh
    bind 127.0.0.1
    ```
	- 为了方便，我们直接将这句注释掉,这样就是所有的主机都可以访问redis,修改为下面这样
	```sh
     # bind 127.0.0.1
     ```
	- 不过为了安全着想，最好设置固定的访问地址，不然部署在公网上的redis没有密码的话是很容易被攻击的
6. 设置为自动启动
    - 先打开解压缩后的目录编辑utils/redis_init_scrip文件，修改文件顶部的几个配置，指定对应的路径（pid文件在运行过redis后会自动生成在/var/run/目录下
	- 将utils/redis_init_scrip文件复制到/etc/init.d/目录下命名为redis，并授权`chmod +x`
	- 开启服务自启，执行命令`chkconfig redis on`
	- 添加到开机自启，执行命令`chkconfig –add redis`

--- ---
### 使用部分
首先需要启动redis自带的客户端,同样在src目录中已经包含了  
	```sh
	cd ~/software/redis-3.2.8/src
	./redis-cli
	# 若是访问远程redis-server的话需要指定host和端口
	./redis-cli -h <host> -p <port>
	```
使用上面的命令就能进入redis客户端了。
1. string类型的一些简单操作
	```sh
	# 添加一个键为name，值为yang的string类型数据
	set name 'yang'
	# 获取键为name的string类型值
	get name
	# 设置键值，并获取旧值,单双引号注意配对
	getset name "yang's blog"
	# 设置键为a的值自增长1，和a=a+1类似，但为atomic操作，不会出现并发问题
	set a 1
	incr a
	# 设置值自减1
	decr a
	# 追加值到指定键的值
	append a 2  -->  这时候a的值应该为12
	# 设置键值的过期时间,设置a过期时间为100s
	setex a 100 s
	# 获取剩余时间，值得注意的是：没有设置过期时间的键得到的值为-1，已过期的键得到的值为-2，其余的显示实际剩余时间
	ttl a
    ```
2. list类型的基本操作  
list的操作一般都分为左右两种，即list头部操作为L，list尾部操作为R，另外list是有序的数据结构  
	```sh
	# 向list中添加一个值
	rpush lst 1
	# 添加多个值
	rpush lst 2 3 4
	# 获取指定索引位置的值
	lindex lst 0
	# 从list尾部获取一个值，并从list中删除
	rpop lst
	# 获取list指定范围索引的值,索引从0开始，可以使用负数，代表从尾部开始的第几个数
	lrange lst 0 -1
	# 获取list的长度，即list元素的个数
	llen lst
	# 设置指定索引位置的值,设置lst的0索引上的值为111
	lset lst 0 111
	# 去除指定范围之外的值，下面这句可以去除list首尾各一个数据
	ltrim lst 1 -2
	```
3. set类型的基本操作
	```sh
	# 添加值 'yang'至s中
	sadd s 'yang'
	# 显示s所有成员
	smembers s
	# 判断obj是否包含于set集合s中
	sismember s obj
	# 显示s成员个数
	scard s
	# 取若干set集合的交集（每个set集合中都包含的才会显示）
	sinter s s1 s2
	# 取交集保存到指定集合target
	sinterstore target s s1 s2
	# 取并集
	sunion s s1 s2
	# 取并集保存到指定集合target
	sunionstore target s s1 s2
	# 差集
	sdiff s s1 s2
	# 取差集保存到指定集合
	sdiffstore target s s1 s2
	```
4. 哈希类型操作：类似于Java中的Map和Python中的Dict
	```sh
	# 设置一个哈希值
	hset hsh name 'yang'
	hset hsh age 21
	# 获取指定哈希中指定字段的值
	hget hsh name
	# 获取哈希中所有的键
	hkeys hsh
	# 获取哈希中所有的值
	hvals hsh
	# 获取哈希中所有的键值
	hgetall hsh
	# 获取哈希中字段个数
	hlen hsh
	# 哈希中某字段自增长1，类似string中的incr
	hincrby hsh age
	```