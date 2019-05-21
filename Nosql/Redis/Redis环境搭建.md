# Redis集群环境搭建

## 依赖安装

```shell
$ yum -y install gcc-c++
$ yum -y install ruby
$ yum -y install rubygems
```

## 安装Redis

```shell
$ wget http://download.redis.io/releases/redis-5.0.5.tar.gz
$ tar -zxvf redis-5.0.5.tar.gz
$ cd /home/redis/redis-5.0.5
$ make && make install
```

## 启动Redis

```shell
$ cd /usr/local/bin
$ ./redis-server
```

## 集群环境搭建

```shell
![506829-20170927221856575-1339828523](C:\Users\imwf\Desktop\506829-20170927221856575-1339828523.png)$ cd /home/redis
$ mkdir redis700{1,2,3,4,5,6}

$ cd /usr/local/bin/
$ cp redis-cli redis-server /home/redis/software/redis7001/
$ cp redis-cli redis-server /home/redis/software/redis7002/
$ cp redis-cli redis-server /home/redis/software/redis7003/
$ cp redis-cli redis-server /home/redis/software/redis7004/
$ cp redis-cli redis-server /home/redis/software/redis7005/
$ cp redis-cli redis-server /home/redis/software/redis7006/

$ cd /home/redis/software/redis-5.0.5
$ cp redis.conf /home/redis/software/redis7001
$ cp redis.conf /home/redis/software/redis7002
$ cp redis.conf /home/redis/software/redis7003
$ cp redis.conf /home/redis/software/redis7004
$ cp redis.conf /home/redis/software/redis7005
$ cp redis.conf /home/redis/software/redis7006

$ vim redis.conf
```

### 修改配置内容

![506829-20170927221856575-1339828523](C:\Users\imwf\Desktop\506829-20170927221856575-1339828523.png)

## 创建启动脚本

```shell
$ cd /home/redis/software
$ vim vim startall.sh
$ chmod 777 startall.sh
$ sh -x startall.sh
```

脚本内容:

```shell
cd redis7001
./redis-server redis.conf
kill -2
cd ../redis7002
./redis-server redis.conf
kill -2
cd ../redis7003
./redis-server redis.conf
kill -2
cd ../redis7004
./redis-server redis.conf
kill -2
cd ../redis7005
./redis-server redis.conf
kill -2
cd ../redis7006
./redis-server redis.conf
kill -2
```

## 通过ruby使用redis集群

```shell
$ cd /home/redis/software/redis-5.0.5/src
$ cp redis-cli ../../
$ ./redis-cli --cluster create 127.0.0.1:7001 127.0.0.1:7002 127.0.0.1:7003 127.0.0.1:7004 127.0.0.1:7005 127.0.0.1:7006 --cluster-replicas 1
```

