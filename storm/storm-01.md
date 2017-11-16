## strom中的NotSerializableException异常

当topology发布时，所有的bolt和spout组件首先会进行序列化，然后通过网络发送到集群中。如果spout或者bolt在序列化之前（比如说再构造函数中生成）实例化了任何无法实例化的实例变量，在进行序列化时会抛出NotSerializableException异常，topology就会部署失败。因为HashMap<String, Long>是可以序列化的，所以在构造函数中进行实例化也是安全的。但是，通常情况下最好是在构造函数中对基本数据类型和可序列化的对象进行赋值和实例化，在prepare()方法中对不可序列化的对象进行实例化。

### 参考

- Storm分布式实时计算模式 P7