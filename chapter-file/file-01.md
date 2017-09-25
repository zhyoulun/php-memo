## 递归创建文件夹

### 例子

```
$r = mkdir('./depth1/depth2/depth3/', 0755, true);
echo $r?'success':'fail';
```

> 函数原型 bool mkdir ( string $pathname [, int $mode = 0777 [, bool $recursive = false [, resource $context ]]] )

### 创建mode=777的文件夹

如果我们使用上述代码尝试创建一个`777`模式的文件夹，会发现结果只是一个`755`模式的文件夹。这是因为在`mkdir()`函数执行过程中，指定的模式会被权限掩码`umask`作用。我们可以使用`umask()`函数来临时修改`umask`，创建好以后再将`umask`修改回来。

```
  0777
- 0022
======
  0755 = rwxr-xr-x.
```

```
$oldmask = umask(0);
mkdir("test", 0777);
umask($oldmask);
```

或者

```
mkdir("test",0777); 
chmod("test",0777); 
```

umask是chmod配套的，总共为4位（gid/uid,属主，组权，其它用户的权限）,不过通常用到的是后3个。

默认情况下的umask值是022(可以用umask命令查看），此时你建立的文件默认权限是644(6-0,6-2,6-2)，建立的目录的默认权限是755(7-0,7-2,7-2)，可以用`ls -l`验证。现在应该知道umask的用途了吧，它是为了控制默认权限，不要使默认的文件和目录具有全权而设的。

### 参考

- [mkdir](http://php.net/manual/zh/function.mkdir.php)
- [Why can't PHP create a directory with 777 permissions?](https://stackoverflow.com/questions/3997641/why-cant-php-create-a-directory-with-777-permissions)
- [Linux命令之umask](http://1123697506.blog.51cto.com/3783048/882064)