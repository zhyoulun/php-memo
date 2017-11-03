## 自动填写数据库登录密码

### 基本

假设我们有一个脚本`interactive.sh`，内容是：

```
#!/bin/sh
read -p "Enter number:" no ;
read -p "Enter name:" name
echo You have entered $no, $name;
```

这个脚本需要我们输入两次字符串，每次以回车键结束

运行结果如下

```
[root@localhost temp6]# ./interactive.sh 
Enter number:1
Enter name:hello
You have entered 1, hello
```

一种比较简单的自动化输入方式是

```
echo -e "1\nhello\n" | ./interactive.sh 
```

用管道将一个命令的stdout（标准输出）重定向到另一个命令的stdin（标准输入）

运行结果为

```
[root@localhost temp6]# echo -e "1\nhello\n" | ./interactive.sh 
You have entered 1, hello
```

或者使用文件

```
[root@localhost temp6]# echo -e "1\nhello\n" > input.txt
[root@localhost temp6]# cat input.txt 
1
hello

[root@localhost temp6]# ./interactive.sh < input.txt 
You have entered 1, hello
```

还可以使用expect命令，不受先后输入顺序的限制，更加灵活

编写文件`automate_expect.sh`，内容如下

```
#!/usr/bin/expect
spawn ./interactive.sh
expect "Enter number:"
send "1\n"
expect "Enter name:"
send "hello\n"
expect eof
```

其中

- spawn参数指定需要自动化哪一个命令，启动新的进程
- expect参数提供需要等待的消息
- send是要发送的消息
- expect eof指明命令交互结束

运行结果如下

```
[root@localhost temp6]# ./automate_expect.sh 
spawn ./interactive.sh
Enter number:1
Enter name:hello
You have entered 1, hello
```

### 自动输入MySQL命令

```
登录脚本的内容
[root@localhost temp6]# cat test.sh 
#!/bin/sh
mysql -u root -p

自动填充密码的脚本
[root@localhost temp6]# cat auto_fill_password.sh 
#!/usr/bin/expect
spawn ./test.sh
expect "Enter password:"
send "password123456\n"
#expect eof
interact

运行
[root@localhost temp6]# ./auto_fill_password.sh 
spawn ./test.sh
Enter password: 
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 7
Server version: 5.7.17 MySQL Community Server (GPL)

Copyright (c) 2000, 2016, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql>
```

其中，`interact`表示，执行完成后保持交互状态，把控制权交给控制台，这个时候就可以手工操作了。如果没有这一句登录完成后会退出，而不是留在远程终端上。如果你只是登录过去执行一段命令就退出，可改为`expect eof`

### 参考

- Linux Shell脚本攻略（第2版）
- [Shell脚本学习之expect命令](http://blog.csdn.net/leexide/article/details/17485451)