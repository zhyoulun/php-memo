## 查找文件

### 列出当前目录及子目录下所有的文件和文件夹

```
[root@localhost folder]# find
.
./file1.txt
./folder1
./folder1/folder12
./folder1/folder12/file101.txt
./folder1/file1000.dat
./folder1/folder11
./file2.txt
./folder2
./folder2/file21.txt
./file3.txt
./folder3
```

### 输出完整路径

```
[root@localhost folder]# find /root/folder/
/root/folder/
/root/folder/file1.txt
/root/folder/folder1
/root/folder/folder1/folder12
/root/folder/folder1/folder12/file101.txt
/root/folder/folder1/file1000.dat
/root/folder/folder1/folder11
/root/folder/file2.txt
/root/folder/folder2
/root/folder/folder2/file21.txt
/root/folder/file3.txt
/root/folder/folder3
```

### 只输出文件或者文件夹

```
[root@localhost folder]# find /root/folder/ -type f
/root/folder/file1.txt
/root/folder/folder1/folder12/file101.txt
/root/folder/folder1/file1000.dat
/root/folder/file2.txt
/root/folder/folder2/file21.txt
/root/folder/file3.txt

[root@localhost folder]# find /root/folder/ -type d
/root/folder/
/root/folder/folder1
/root/folder/folder1/folder12
/root/folder/folder1/folder11
/root/folder/folder2
/root/folder/folder3
```

### 匹配文件名

```
[root@localhost folder]# find /root/folder/ -type f -name "*.dat"
/root/folder/folder1/file1000.dat
```

### 参考

- Linux Shell脚本攻略（第2版）