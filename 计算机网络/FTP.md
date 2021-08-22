* FTP协议
* 搭建FTP服务器
* FTP客户端
* TFP命令





# FTP



**文件传输协议( File Transfer Protocol )**

 TCP/IP 协议组中的协议之一 

CS架构

控制文件的双向传输

与FTP服务器的21端口建立连接来传输命令，与服务器的21/其他大于1024的端口(取决于模式)来传输数据。

用来传数据的连接有两种方式：主动模式与被动模式 。



## 两种工作模式



**主动模式(PORT)：**

客户端先与服务端的21端口建立连接用来发送命令，然后在需要接受数据时发送PORT命令(包含客户端用哪个接口去接收数据)，服务端再用自己的20端口连接到客户端指定的接口去发送数据。

主动的意思就是：FTP服务器主动连接客户端的端口传输数据。

这时候要注意：客户端这个端口在防火墙里时就不会传输

主动模式下，FTP服务器的端口是固定的，



**被动模式(PASV)：**

客户端先与服务端的21端口建立连接用来发送命令，然后发送PASV命令，FTP服务器会打开一个大于1024的端口告诉客户端在这个端口上传数据， 客户端连接FTP服务器此端口，通过三次握手建立通道，然后FTP服务器将通过这个端口进行数据的传送。 

注意：FTP服务器这个端口在防火墙里面时，就不会传输数据。

样FTP服务器随机打开一个大于1024的端口，就没办法在防火墙里面配置。但是FTP服务器可以限定打开端口的范围。



***主动模式还是被动模式由客户端发送的命令决定***



# Centos搭建FTP服务器



[参考](https://www.cnblogs.com/guohongwei/p/10848698.html)



## **三种认证模式**



**匿名模式**

不需要密码就能登录FTP服务器，不安全。

默认模式 开放匿名用户的上传、下载文件的权限，以及让匿名用户创建、删除、更名文件的权限 



**本地用户模式**

使用FTP服务器所在的操作系统中的用户名和密码(即本地用户)来进行认证。这种做法相对安全，但是当通过FTP破解了用户名和密码，就可能控制机器。



**虚拟用户模式**

虚拟几个用户，这几个用户在本地是不存在的，会映射到本地用户，所以只破解FTP的用户名和密码不会控制机器。



## **安装vsftpd**



查看是否安装了vsftpd

```bash
rpm -qa | grep vsftpd
```



安装vsftpd

```bash
yum -y install vsftpd
```



启动vsftpd

```bash
systemctl start vsftpd
```



**配置文件**

 /etc/vsftpd/下 存在三个配置文件：  vsftpd.conf ,ftpusers，user_list

`vsftpd.conf` 主配置文件

`ftpusers`中是不允许访问的用户

`user_list`中是允许/不允许访问的用户(取决于主配置，userlist_deny=NO表示只允许这个文件中的用户访问，默认为yes，表示不能访问)



**主配置文件**

```bash
# 是否以独立运行的方式监听服务
listen=[YES|NO] 
# 设置要监听的IP地址
listen_address=IP地址 
# 设置FTP服务的监听端口
listen_port=21  
# 是否允许下载文件
download_enable＝[YES|NO] 
# userlist文件中的用户是允许访问还是不允许访问的
userlist_enable=[YES|NO]
# userlist文件中的用户是允许访问还是不允许访问的
userlist_deny=[YES|NO]   
# 最大客户端连接数，0为不限制
max_clients=0 
# 同一IP地址的最大连接数，0为不限制
max_per_ip=0 
# 是否允许匿名用户访问
anonymous_enable=[YES|NO] 
# 是否允许匿名用户上传文件
anon_upload_enable=[YES|NO]
# 匿名用户上传文件的umask值
anon_umask=022   
# 匿名用户的FTP根目录，默认/var/ftp/pub
anon_root=/var/ftp  
# 是否允许匿名用户创建目录
anon_mkdir_write_enable=[YES|NO] 
# 是否开放匿名用户的其他写入权限（包括重命名、删除等操作权限）
anon_other_write_enable=[YES|NO]  
# 匿名用户的最大传输速率（字节/秒），0为不限制
anon_max_rate=0
# 是否允许本地用户登录FTP
local_enable=[YES|NO] 
# 本地用户上传文件的umask值
local_umask=022
# 本地用户的FTP根目录，默认就是本地用户的Home目录
local_root=/var/ftp
# 是否将用户权限禁锢在FTP目录，以确保安全
chroot_local_user=[YES|NO]
# 本地用户最大传输速率（字节/秒），0为不限制
local_max_rate=0                    
```

可以看到，配置文件**只对匿名用户和本地用户进行配置**，因为虚拟用户只是虚拟一个用来认证的用户信息，然后映射到一个本地用户



### **匿名用户模式**

```
anonymous_enable=YES
anon_umask=022
anon_upload_enable=YES
anon_mkdir_write_enable=YES
anon_other_write_enable=YES
local_enable=YES
write_enable=YES
local_umask=022
dirmessage_enable=YES
xferlog_enable=YES
connect_from_port_20=YES
xferlog_std_format=YES
listen=NO
listen_ipv6=YES
pam_service_name=vsftpd
userlist_enable=YES
tcp_wrappers=YES
```

重启vsftpd

```bash
systemctl restart vsftpd
```

**设置为开机自启**

```bash
systemctl enable vsftpd
```

查看vsftp状态

```bash
systemctl status vsftpd
```

连接ftp服务器，用户名为anonymous，密码直接回车



### 本地用户模式

```
# 进制匿名模式
anonymous_enable=NO
# 打开本地模式
local_enable=YES
write_enable=YES
local_umask=022
dirmessage_enable=YES
xferlog_enable=YES
connect_from_port_20=YES
xferlog_std_format=YES
listen=NO
listen_ipv6=YES
pam_service_name=vsftpd
userlist_enable=YES
tcp_wrappers=YES
```

重启服务

就可以使用本地的用户信息来登录FTP服务器(不在"用户名单"中的)

可以发现，在黑名单中的用户直接不用输入密码就失败了

本地用户登录后访问的是它自己的HOME目录，所以不存在权限问题



### 虚拟用户模式



db_load命令

file命令

PAM文件



虚拟用户模式登录失败









# FQA

1. 550 Create directory operation failed

* 配置文件中有没有允许创建
* 用户是否有权限在该路径创建文件夹
* 是否可以关掉SELinux

2. 



