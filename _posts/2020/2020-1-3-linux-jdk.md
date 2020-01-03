---
title: Linux CentOS 7 操作系统配置JDK
tags: Linux CentOS 7 操作系统配置JDK
titlebar: arch
author: 一百零八天

---

## 前言

##### 工具
- Window 7/10 台式机
- VMware Workstation Pro虚拟机
- CentOS7操作系统
- JDKjdk1.8.0_40安装包
- XShell6

##### 说明
远程命令式操作工具[XShell](https://www.netsarang.com/zh/xshell/)
> 使用 [XShell](https://www.netsarang.com/zh/xshell/)服务器或者虚拟机可以使用rz命令上传文件，有的链接工具此命令并不能上传文件(例如：阿里云的网页版链接工具)
![XFTP](https://www.netsarang.com/wp-content/uploads/2018/12/xshell_wc.png)

远程视图文件上传下载操作工具[XFTP](https://www.netsarang.com/zh/xftp/)
>或者使用[XFTP](https://www.netsarang.com/zh/xftp/)可以上传文件，链接之前，请打开远程登录功能，并且打开21,22端口或者直接关闭防火墙（本地测试可以直接关闭防火墙）
![XFTP](https://www.netsarang.com/wp-content/uploads/2018/12/xftp_wc.png)


### 删除已经存在的JDK
```
# cd /usr/lib/jvm/ #jdk安装目录
# rm -rf *  #删除jvm 下面的所有文件
```
> 如果提示没有权限，可以切换到root账号，或者给你登录的账号授权
> 账号切换命令 ``` sudo su root ``` 然后再输入密码，账号切换成功

### 上传要安装JDK版本的安装包
```
# rz  #上传文件命令
```

### 解压文件到 /usr/lib/jvm 目录
```
# tar zxvf ./jdk-8u40-linux-x64.gz  -C /usr/lib/jvm
```

### 配置环境变量
```
# vi ~/.bashrc
```
在文件.bashrc最下面添加如下脚本
> export JAVA_HOME=/usr/lib/jvm/jdk1.8.0_40
> export JRE_HOME=${JAVA_HOME}/jre
> export CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib
> export PATH=${JAVA_HOME}/bin:$PATH

### 使环境变量生效
```
# source ~/.bashrc
```
### 验证JDK安装是否成功
```
# java -version
```
### 结果
> java version "1.8.0_40"
> Java(TM) SE Runtime Environment (build 1.8.0_40-b25)
> Java HotSpot(TM) 64-Bit Server VM (build 25.40-b25, mixed mode)

成功！

### 可能会遇到的问题
**1. XShell远程连接不上** 
> 原因可能是远程连接没有打开，或者远程连接的端口21没有打开（21远程连接，22远程文件传输）

永久打开防火墙端口命令如下：
```
# firewall-cmd --zone=public --add-port=21/tcp --permanent
# firewall-cmd --zone=public --add-port=22/tcp --permanent
```
关闭防火墙端口
```
# firewall-cmd --zone= public --remove-port=80/tcp --permanent
```
关闭防护墙
```
# systemctl stop firewalld
```
重新载入防火墙
```
# firewall-cmd --reload
```
-----------------------------The end-----------------------------
XShell文件:https://pan.baidu.com/s/11YG7M9ph0dTviu-jWl9AwQ 提取码：gcxx
