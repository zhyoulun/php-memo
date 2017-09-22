## 递归创建文件夹

### 例子

```
$r = mkdir('./depth1/depth2/depth3/', 0755, true);
echo $r?'success':'fail';
```

> bool mkdir ( string $pathname [, int $mode = 0777 [, bool $recursive = false [, resource $context ]]] )

### 创建mode=777的文件夹

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

## 参考

- [mkdir](http://php.net/manual/zh/function.mkdir.php)
- [Why can't PHP create a directory with 777 permissions?](https://stackoverflow.com/questions/3997641/why-cant-php-create-a-directory-with-777-permissions)