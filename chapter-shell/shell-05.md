## sed

```
# 将CA替换为Canada，s是替换命令
# 建议养成习惯使用单引号，单引号可以阻止shell解释编辑指令中的特殊字符或空格
sed -e 's/CA/Canada/' input.txt

# -e可以省略
sed 's/CA/Canada/' input.txt

# 使用多条指令
sed -e 's/CA/Canada/' -e 's/JP/Japan' input.txt

# 使用分号和并多条指令
sed -e 's/CA/Canada/;s/JP/Japan/' input.txt

# 从文件读取多条指令
sed -f cmd.txt input.txt
# 其中cmd.txt的内容是
s/CA/Canada/
s/JP/Japan/

# -n选项可以阻止自动输出
# 打印命令p，只显示受命令影响的行
sed -n -e 's/CA/Canada/p' input.txt
```