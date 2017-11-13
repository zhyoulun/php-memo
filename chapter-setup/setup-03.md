## storm安装备忘

选择镜像下载storm安装包

```
tar -zxvf apache-storm-0.9.7.tar.gz
mv apache-storm-0.9.7 storm-0.9.7
ln -s storm-0.9.7 storm
```

修改`conf/storm.yaml`文件

```
storm.zookeeper.servers:
    - "127.0.0.1"
 
storm.local.dir: "/data/storm"

nimbus.seeds: ["127.0.0.1"]

supervisor.slots.ports:
    - 6700
    - 6701
    - 6702
    - 6703
```

启动

```
# 启动Nimbus后台程序
nohup bin/storm nimbus > /log/storm/nimbus.log 2>&1 &

# 启动Supervisor后台程序
nohup bin/storm supervisor > /log/storm/supervisor.log 2>&1 &

# 启动UI后台程序，并放到后台执行，启动后可以通过http://{nimbus host}:8080观察集群的worker资源使用情况、Topologies的运行状态等信息
nohup bin/storm ui > /log/storm/ui.log 2>&1 &
```

### 参考

- [http://storm.apache.org/releases/0.10.2/Setting-up-a-Storm-cluster.html](http://storm.apache.org/releases/0.10.2/Setting-up-a-Storm-cluster.html)
- [Storm集群安装部署步骤【详细版】](http://www.cnblogs.com/panfeng412/archive/2012/11/30/how-to-install-and-deploy-storm-cluster.html)