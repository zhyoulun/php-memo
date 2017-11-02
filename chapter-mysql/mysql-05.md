## MySQL中的控制流函数

### case

语法

```
CASE value 
    WHEN [compare_value] THEN result 
    [WHEN [compare_value] THEN result ...] 
    [ELSE result] 
END

CASE 
    WHEN [condition] THEN result 
    [WHEN [condition] THEN result ...] 
    [ELSE result] 
END
```

备注：如果没有[ELSE result]，且没有条件匹配上，则会返回NULL

示例

```
mysql> select case 1 when 1 then 'one'
    -> when 2 then 'two'
    -> else 'more'
    -> end as c;
+-----+
| c   |
+-----+
| one |
+-----+

mysql> SELECT CASE WHEN 1>0 THEN 'true' ELSE 'false' END as c;
+------+
| c    |
+------+
| true |
+------+
```

### if

语法

```
IF(expr1,expr2,expr3)
```

如果expr1为true，或者不等于0，或者不等于NULL，返回expr2；否则返回expr3

返回值的格式：

- 如果expr2或者expr3是字符串，结果就是字符串
- 如果expr2和expr3都是字符串，并且其中一个字符串是大小写敏感的，那么结果就是大小写敏感的
- 如果expr2或者expr3产生一个浮点值，结果就是一个浮点值
- 如果expr2或者expr3产生一个整数，结果就是一个整数

### ifnull

语法

```
IFNULL(expr1,expr2)
```

如果expr1是NULL，则返回expr1；否则返回expr2

### nullif

语法

```
NULLIF(expr1,expr2)
```

如果expr1=expr2，返回NULL；否则返回expr1。等价于`CASE WHEN expr1 = expr2 THEN NULL ELSE expr1 END`

### 参考

- [https://dev.mysql.com/doc/refman/5.7/en/control-flow-functions.html](https://dev.mysql.com/doc/refman/5.7/en/control-flow-functions.html)