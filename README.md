### Nginx基础学习

------

Nginx是一款轻量级的HTTP服务器，采用事件驱动的异步非阻塞处理方式框架，这让其具有极好的IO性能，时常用于服务端的反向代理和负载均衡。

**Nginx的优点**

- 支持海量高并发：采用IO多路复用epoll。官方测试Nginx能够支持5万并发链接，实际生产环境中可以支撑2-4万并发连接数。
- 内存消耗少：在主流的服务器中Nginx目前是内存消耗最小的了，比如我们用Nginx+PHP，在3万并发链接下，开启10个Nginx进程消耗150M内存。
- 免费使用可以商业化：Nginx为开源软件，采用的是2-clause BSD-like协议，可以免费使用，并且可以用于商业。
- 配置文件简单：网络和程序配置通俗易懂，即使非专业运维也能看懂。
- 反向代理功能，负载均衡功能等

#### 第一节、初识Ngnix和环境准备

** 学习环境 **

学习环境你可以有三种选择：

- **自己找个电脑搭建**：需要自己有闲置电脑或者服务器，优点是稳定性高，可控能力强，学习更方便。
- **购买阿里云ECS**：需要花些小钱，1.阿里云学生ECS，每月9.5元 2.注册实名认证后免费一个月的ECS
- **使用虚拟软件**：这个如果电脑配置高，可以安装虚拟软件，缺点是麻烦，影响电脑性能，而且配置也比较多

我选择的是阿里云ECS 操作系统是CentOS 7.6 64位版本

购买ECS后，可以看到实例信息

重置实例密码并重启实例，如图:

![SCE](./static/SCE.png)

```
# ssh root@ip  // 输入密码即可登录远程服务器
```

用yum安装必要的程序

```
yum -y install gcc gcc-c++ autoconf pcre-devel make automake
yum -y install wget httpd-tools vim
```

创建目录

```
mkdir itxing
cd itxing
mkdir app backup download logs work
```

#### 第二节、Ngnix快速搭建

Niginx版本说明 [下载地址](http://nginx.org/en/download.html)

- Mainline version: Mainline 是 Nginx 目前主力在做的版本，可以说是开发版
- Stable version: 最新稳定版，生产环境上建议使用的版本
- Legacy versions：遗留的老版本的稳定版

基于yum方式安装Nginx

查看一下yum是否已经存在

```
yum list | grep nginx
```

系统原来的源只支持1.1版本,版本较低

我们可以自行配置yum源

```
vim /etc/yum.repos.d/nginx.repo
```

复制以下代码，修改文件

```
[nginx]
name=nginx repo
baseurl=http://nginx.org/packages/centos/7/$basearch/
gpgcheck=0
enabled=1
```

安装nginx

```
yum install nginx
nginx -v
```

