# 常用工程软件在Linux（CentOS 7）的部署安装

**摘要：本文是一份详尽的实战指南，手把手带领您在Linux服务器上从零部署一套生产级别的工程软件栈。**



## <u>首先简单介绍一下yum</u>

**什么是yum？**

**yum**（全称为 **Y**ellowdog **U**pdater **M**odified）是一个在 **RPM** 为基础的 Linux 发行版（如 CentOS、RHEL、Fedora）中使用的**高级包管理器**。

您可以把它理解为类似于手机上的 **“应用商店”** 或 Windows 上的 **“软件管理中心”**。

语法：yum [-y] [install | remove |search] 软件名称（需要root权限）

* -y,自动确认，无需手动确认安装或卸载
* install:安装
* remove:卸载
* search:搜索



## MySQL 8.0安装部署

```shell
# 更新密钥
rpm --import https://repo.mysql.com/RPM-GPG-KEY-mysql-2023

# 安装Mysql8.x版本 yum库
rpm -Uvh https://dev.mysql.com/get/mysql80-community-release-el7-2.noarch.rpm

# yum安装Mysql
yum -y install mysql-community-server

#得到初始密码
grep 'temporary password' /var/log/mysqld.log

# 执行登录操作
mysql -uroot -p

#修改密码
ALTER USER 'root'@'localhost' IDENTIFIED BY '密码';

#设置简单密码
set global validate_password.policy=0;# 密码安全级别低
set global validate_password.length=4;# 密码长度最低4位即可

# 第一次设置创建用户用于远程登录，并配置远程密码
CREATE USER '用户'@'%' IDENTIFIED BY '密码';

#授予所有权限（可根据需要限制权限）
GRANT ALL PRIVILEGES ON *.* TO '用户'@'%' WITH GRANT OPTION;

#刷新权限
FLUSH PRIVILEGES;

# 开机自启
systemctl enable mysqld
```



## Redis安装部署

```shell
#安装了gcc环境等
yum install -y gcc tcl make

#下载redis(.tar.gz)
cd /usr/local/src
wget https://download.redis.io/releases/redis-7.0.12.tar.gz
tar -zxvf redis-7.0.12.tar.gz -C /usr/local/redis
cd /usr/local/redis/redis-7.0.12

#安装编译
make && make install

#备份配置文件
cp redis.conf redis.conf.bak

#修改配置文件
vim redis.conf
# daemonize 的值从 no 修改成 yes（Redis服务默认是前台运行，需要修改为后台运行）
 daemonize no ---> daemonize yes
# requirepass foobared注释去掉并在后加上密码（注意中间加个空格）
requirepass foobared ---> requirepass 密码	
# 设置redis记录日志，默认不记录日志（redis.log为文件名）
logfile " " ---> logfile "redis.log"

#注册服务
vim /etc/systemd/system/redis.service
#填入内容
[Unit]
Description=redis-server
After=network.target

[Service]
Type=forking
ExecStart=/usr/local/bin/redis-server /usr/local/redis/redis-7.0.12/redis.conf
PrivateTmp=true

[Install]
WantedBy=multi-user.target

#让服务生效
systemctl daemon-reload

# 开机自启
systemctl enable redis
```



## Docker安装部署

```shell
#卸载旧版本
yum remove docker \
    docker-client \
    docker-client-latest \
    docker-common \
    docker-latest \
    docker-latest-logrotate \
    docker-logrotate \
    docker-engine


# 安装必要工具
yum install -y yum-utils device-mapper-persistent-data lvm2

# 添加Docker的阿里云源
yum-config-manager --add-repo https://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo

sed -i 's+download.docker.com+mirrors.aliyun.com/docker-ce+' /etc/yum.repos.d/docker-ce.repo

#更新yum
yum makecache fast

#安装docker
yum install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

#开机自启
systemctl enable docker

#配置镜像加速
mkdir -p /etc/docker
tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["加速器地址"]
}
EOF
systemctl daemon-reload
systemctl restart docker
```

**如何获取加速器地址？**

介绍一种通过阿里云配置镜像加速的方法

确保已有阿里云账号，访问[[容器镜像服务 ACR 控制台](https://cr.console.aliyun.com/cn-hangzhou/instances/mirrors)](https://cr.console.aliyun.com/cn-hangzhou/instances/mirrors)

<img width="1898" height="1579" alt="Image" src="https://github.com/user-attachments/assets/2930a619-bd4f-4574-8524-4c3548c02eac" />



## Nginx安装部署

```shell
# 安装必要工具
yum install -y gcc-c++ pcre-devel zlib-devel openssl-devel

#下载tar包
cd /usr/local/src
wget https://nginx.org/download/nginx-1.28.0.tar.gz
tar -zxvf nginx-1.28.0.tar.gz -C /usr/local/nginx
cd /usr/local/nginx/nginx-1.28.0

# 编译前的配置和依赖检查
./configure

# 编译安装
make && make install

#配置 Nginx 服务文件
vim /etc/systemd/system/nginx.service

#填入如下内容
[Unit]
Description=Nginx HTTP Server
After=network.target

[Service]
Type=forking
ExecStart=/usr/local/nginx/sbin/nginx
ExecReload=/usr/local/nginx/sbin/nginx -s reload
ExecStop=/usr/local/nginx/sbin/nginx -s stop
PrivateTmp=true

[Install]
WantedBy=multi-user.target

#重新加载文件
systemctl daemon-reload

# 开机自启
systemctl enable nginx	
```



## 安装JDK（以jdk-21为例）

```shell
# 下载tar包
cd /usr/local/src
wget https://download.oracle.com/java/21/latest/jdk-21_linux-x64_bin.tar.gz
tar -zxvf jdk-21_linux-x64_bin.tar.gz -C /usr/local/jdk

# 创建软链接
cd /usr/local/jdk
ln -s /usr/local/jdk/jdk-21.0.8 /usr/local/jdk/jdk

#添加环境变量
vim /etc/profile
#添加如下信息
export JAVA_HOME=/usr/local/jdk/jdk
export PATH=$PATH:$JAVA_HOME/bin

#删除默认版本的jdk
rm -f /usr/bin/java

#把新安装的jdk-21指向/usr/bin/java
ln -s /usr/local/jdk/jdk/bin/java /usr/bin/java

#查看jdk版本（版本为新安装的21即为成功）
java -version
```

## 安装Tomcat

```shell
#添加一个新用户tomcat
useradd tomcat
su - tomcat

#安装tomcat
wget https://dlcdn.apache.org/tomcat/tomcat-10/v10.1.46/bin/apache-tomcat-10.1.46.tar.gz
#切回root执行解压操作，否则解压地址没有权限
su - root
cd /usr/local
mkdir tomcat
#解压缩
cd /home/tomcat/
tar -zxvf apache-tomcat-10.1.46.tar.gz -C /usr/local/tomcat
cd /usr/local/tomcat
ln -s /usr/local/tomcat/apache-tomcat-10.1.46 /usr/local/tomcat/tomcat

# 把文件拥有者改为tomcat用户
chown -R tomcat:tomcat tomcat
chown -R tomcat:tomcat apache-tomcat-10.1.46

#启动tomcat(默认绑定在8080端口)
/usr/local/tomcat/tomcat/bin/startup.sh
```


