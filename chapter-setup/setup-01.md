## redis安装备忘

```
wget http://download.redis.io/releases/redis-4.0.2.tar.gz
tar -zxvf redis-4.0.2.tar.gz
cd redis-4.0.2

make
make PREFIX=/software/redis-4.0.2 install

cp redis.conf /software/redis-4.0.2/bin/
cd /software/redis-4.0.2
cp redis.conf redis.conf.bak

cd /software
ln -s redis-4.0.2 redis
```

修改`redis.conf`：

```
# 以后端模式启动
daemonize yes
# 设置日志文件
logfile "/log/redis.log"
# 设置磁盘备份数据位置，该文件夹必须提前创建好
dir /data/redis/
```

启动

```
./redis-server redis.conf
```

### 参考

- [Linux下redis安装和部署](http://www.jianshu.com/p/bc84b2b71c1c)