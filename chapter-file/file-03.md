## 写入文件时使用独占锁

用于在写入文件时，防止其他人同时也在写该文件。

如何验证？？？

### 代码

```
file_put_contents($file, $content, LOCK_EX);
```

### 参考

- [file_put_contents](http://php.net/manual/zh/function.file-put-contents.php)