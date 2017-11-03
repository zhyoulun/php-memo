## expect使用示例

### ssh自动填写密码登录

`goto`文件的内容

```
#!/usr/bin/expect
if {$argc<1} {
	send_user "usage: $argv0 hostname\n"
	exit
}

set host [lindex $argv 0]
spawn ssh $host

expect {
    # 自动填写yes
	"*yes/no*" {
		send "yes\n"
		exp_continue
	}
    # 自动填写密码
	"*password:" {
		send "password123456\n"
        # 不能要
#		exp_continue 
	}
# 没有作用
#	eof {
#		exit
#	}
}

interact
```

其中`#`号开头的行被注释掉了，运行

```
$ ./goto 
usage: ./goto hostname
```

### 参考

- [Shell脚本学习之expect命令](http://blog.csdn.net/leexide/article/details/17485451)
- [linux expect详解(ssh自动登录)](http://www.cnblogs.com/lzrabbit/p/4298794.html)