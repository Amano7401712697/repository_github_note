# Cent OS  7 

ip addr 指令替代 ifconfig 指令



## 网络配置

```cmd
$ cd /etc/sysconfig/network-scripts/
$ vi ifcfg-ens**

	TYPE=Ethernet
    PROXY_METHOD=none
    BROWSER_ONLY=no
    BOOTPROTO=static
    IPADDR=192.168.70.110
    NETMASK=255.255.255.0
    GATEWAY=192.168.70.2
    DEFROUTE=yes
    IPV4_FAILURE_FATAL=no
    IPV6INIT=yes
    IPV6_AUTOCONF=yes
    IPV6_DEFROUTE=yes
    IPV6_FAILURE_FATAL=no
    IPV6_ADDR_GEN_MODE=stable-privacy
    NAME=ens33
    UUID=5c3a3d2c-1035-4587-8355-89e29c4225cf
    DEVICE=ens33
    ONBOOT=yes
   
$ cd /etc/
$ vi resolv.conf
	search localdomain
	nameserver 114.114.114.114

$ service network restart
$ ip addr
```



## 安装JDK1.8

1. 前往Oracle官网下载jdk安装包 [下载链接](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)
2. 上传至服务器，并解压至  **/usr/java/**
3. 配置环境变量

```cmd
$ vi /etc/profile
	export JAVA_HOME=/usr/java/jdk1.8
    export PATH=$JAVA_HOME/bin:$PATH
    export CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib
$ source /etc/profile  ##重新加载配置文件
```



## 安装mysql

1. 下载安装包mysql  [下载链接](https://downloads.mysql.com/archives/community/)
2. 安装依赖

```shell
yum install -y gcc-c++
yum install -y numactl
```

1. 卸载系统自带的Mariadb

```cmd
$ rpm -qa|grep mariadb
mariadb-libs-5.5.60-1.el7_5.x86_64
$ mariadb-libs-5.5.60-1.el7_5.x86_64 ##执行卸载
```

3. 删除etc目录下的my.cnf 

```cmd
$ rm /etc/my.cnf
```

4. 解压mysql安装包

```shell
tar -zxvf mysql-5.7.24-linux-glibc2.12-x86_64.tar.gz
mv mysql-5.7.24-linux-glibc2.12-x86_64 mysql
```

4. 创建mysql用户和用户组

```cmd
$ groupadd mysql
$ useradd -g mysql mysql
```

5. 初始化配置

```cmd
$ bin/mysqld --initialize --user=mysql --basedir=/usr/local/mysql --datadir=/usr/local/mysql/data
```

6. 新建并修改配置文件

```cmd
$ cd /usr/local/mysql/support-files
$ cp mysql.server /etc/init.d/mysql
$ vi /etc/init.d/mysql
	basedir=/usr/local/mysql
	datadir=/usr/local/mysql/data
```

7. 启动mysql

   1. 启动mysql

   ```cmd
    /etc/init.d/mysql start
   ```

   2. 登录mysql

   ```cmd
   $ mysql -hlocalhost -uroot -p
    如果出现：-bash: mysql: command not found
    就执行： # ln -s /usr/local/mysql/bin/mysql /usr/bin --没有出现就不用执行
   ```

   3. 修改密码

   ```sql
   set password=password('123456');
   ```

   4. 设置root账户的host地址

   ```sql
   grant all privileges on *.* to 'root'@'%' identified by '123456';
   flush privileges;
   use mysql;
   ```

   5. 启动防火墙放行3306端口

   ```cmd
   vim /etc/sysconfig/iptables
   iptables -A INPUT -p tcp -m state --state NEW -m tcp --dport 3306 -j ACCEPT
   service iptables restart
   ```

   ## Mysql配置修改相关
   
   1.  允许远程连接Mysql
   
   ```sql
   -- 登录mysql
   mysql -uroot -p
   grant all on *.* to 'root'@'%' identified by 'mysql' with grant option; 
   flush privileges;
   ```
   
   2.  新建用户并赋予权限
   
   ```sql
   -- 选择mysql数据库
   use mysql;
   -- 从192.168.122.12登陆的用户
   create user 'mysql'@'192.168.122.12' identified by 'mysql';
   -- 从任意ip登陆的用户
   create user 'mysql'@'%' identified by 'mysql';
   -- 不做指定默认为'%'
   create user 'mysql' identified by 'mysql';
   -- 刷新MySQL的系统权限相关表，使添加用户操作生效，以免会出现拒绝访问
   flush privileges;
   
   -- 赋予部分权限，其中的shopping.*表示对以shopping所有文件操作。
   grant select,delete,update,insert on *.* to superboy@'localhost' identified by 'mysql';
   flush privileges;
   -- 赋予所有权限
   grant all privileges on test.* to mysql@'%' identified by 'mysql';
   flush privileges;
   -- 撤销update权限
   revoke update on simpleshop.* from superboy@localhost;
   -- 撤销所有权限
   revoke all on simpleshop.* from superboy@localhost;
   ```
   
   