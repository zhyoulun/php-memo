## 多个程序向同一个文件追加数据

先做`lseek`操作后做`write`操作，这不是一个原子操作，会出现竞争状态，执行的结果依赖于内核对两个进程的的调度顺序。

可以在打开文件的时候，设置`O_APPEND`标志来解决这个问题。

`O_APPEND`的意思是，如果设置了该标志位，在每次write之前，文件的offset都会被设置到末尾。（是一个原子操作）

### 参考

- Linux/Unix系统编程手册，上，p74页 
- [http://man7.org/linux/man-pages/man3/open.3p.html](http://man7.org/linux/man-pages/man3/open.3p.html)
