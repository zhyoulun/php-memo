## kafka-manager安装备忘

下载release包，[https://github.com/yahoo/kafka-manager/releases](https://github.com/yahoo/kafka-manager/releases)，解压包进入文件夹执行如下命令

```
BASE_PATH=$(cd `dirname $0`; pwd)
ZK_HOSTS="localhost:2181"

$BASE_PATH/sbt clean dist
```

成功后会有如下输出（执行中会下载很多文件，可能需要使用代理）

```
Getting org.scala-sbt sbt 0.13.9 ...
...
...
[info] Your package is ready in /da1/s/software/kafka-manager-1.3.3.14/target/universal/kafka-manager-1.3.3.14.zip
[info] 
[success] Total time: 1289 s, completed Oct 16, 2017 5:48:56 PM
```

解压生成的压缩包，执行命令

```
ZK_HOSTS="localhost:2181"
bin/kafka-manager -Dhttp.port=8080
```

就可以在浏览器中操作了`http://localhost:8080`

### 参考

- [https://github.com/yahoo/kafka-manager](https://github.com/yahoo/kafka-manager)