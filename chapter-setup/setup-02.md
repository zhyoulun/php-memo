## zookeeper安装备忘

选择镜像，下载zookeeper安装包

```
tar -zxvf zookeeper-3.4.10.tar.gz

ln -s zookeeper-3.4.10 zookeeper

cp conf/zoo_sample.cfg conf/zoo.cfg
```

修改conf/zoo.cfg文件

```
dataDir=/data/zookeeper
```

启动

```
bin/zkServer.sh start
```

### 参考

- [http://zookeeper.apache.org/doc/r3.4.10/zookeeperStarted.html](http://zookeeper.apache.org/doc/r3.4.10/zookeeperStarted.html)