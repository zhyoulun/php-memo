## 获取当前脚本所在文件夹的路径

```
basepath=$(cd `dirname $0`; pwd)
```

解释：

1. dirname $0，取得当前执行的脚本文件的父目录
2. cd `dirname $0`，进入这个目录(切换当前工作目录)
3. pwd，显示当前工作目录(cd执行后的)

### 参考

- [linux shell 获取当前正在执行脚本的绝对路径](http://www.cnblogs.com/FlyFive/p/3640267.html)