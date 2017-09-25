## 异常捕获

### 代码1

```
<?php
function func1($a,$b,$c,$d)
{
    throw new Exception();
}

function func2($a,$b,$c)
{
    func1($a,$b,$c,4);
}

function func3($a, $b)
{
    func2($a,$b,3);
}

try{
    func3(1,2);
}catch (Exception $e){
    var_dump($e);
}
```

输出

```
object(Exception)#1 (7) {
  ["message":protected]=>
  string(0) ""
  ["string":"Exception":private]=>
  string(0) ""
  ["code":protected]=>
  int(0)
  ["file":protected]=>
  string(40) "D:\git.f\zhyoulun.com\runtime\logs\a.php"
  ["line":protected]=>
  int(4)
  ["trace":"Exception":private]=>
  array(3) {
    [0]=>
    array(4) {
      ["file"]=>
      string(40) "D:\git.f\zhyoulun.com\runtime\logs\a.php"
      ["line"]=>
      int(9)
      ["function"]=>
      string(5) "func1"
      ["args"]=>
      array(4) {
        [0]=>
        int(1)
        [1]=>
        int(2)
        [2]=>
        int(3)
        [3]=>
        int(4)
      }
    }
    [1]=>
    array(4) {
      ["file"]=>
      string(40) "D:\git.f\zhyoulun.com\runtime\logs\a.php"
      ["line"]=>
      int(14)
      ["function"]=>
      string(5) "func2"
      ["args"]=>
      array(3) {
        [0]=>
        int(1)
        [1]=>
        int(2)
        [2]=>
        int(3)
      }
    }
    [2]=>
    array(4) {
      ["file"]=>
      string(40) "D:\git.f\zhyoulun.com\runtime\logs\a.php"
      ["line"]=>
      int(19)
      ["function"]=>
      string(5) "func3"
      ["args"]=>
      array(2) {
        [0]=>
        int(1)
        [1]=>
        int(2)
      }
    }
  }
  ["previous":"Exception":private]=>
  NULL
}
```

### 代码2

将代码1中的`var_dump($e);`替换为`printException($e);`

输出结果

```
index:1---------------
file: D:\git.f\zhyoulun.com\runtime\logs\a.php
line: 19
function: func3
args: 1,2
index:2---------------
file: D:\git.f\zhyoulun.com\runtime\logs\a.php
line: 14
function: func2
args: 1,2,3
index:3---------------
file: D:\git.f\zhyoulun.com\runtime\logs\a.php
line: 9
function: func1
args: 1,2,3,4
index:4---------------
code: 0
message: 
file: D:\git.f\zhyoulun.com\runtime\logs\a.php
line: 4
```

可以用作定制化展示，例如Yii2对异常的处理。

### 参考

- [Exception::__construct](http://php.net/manual/zh/exception.construct.php)