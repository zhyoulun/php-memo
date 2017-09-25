## 解析ini配置文件

### 例子1

配置文件`test1.ini`：

```
[session1]
one=1
pi=3.14
string='this is a test'

[session2]
array[]=11
array[]=22
array[]=33
```

### 代码1

将配置文件中的所有键值对解析出来

```
$config=parse_ini_file("test1.ini");
var_dump($config);
```

输出结果

```
array(4) {
  ["one"]=>
  string(1) "1"
  ["pi"]=>
  string(4) "3.14"
  ["string"]=>
  string(14) "this is a test"
  ["array"]=>
  array(3) {
    [0]=>
    string(2) "11"
    [1]=>
    string(2) "22"
    [2]=>
    string(2) "33"
  }
}
```

### 代码2

处理session，可以得到一个多维数组，包括了配置文件中每一节的名称和设置

```
$config=parse_ini_file("test1.ini", true);
var_dump($config);
```

输出结果

```
array(2) {
  ["session1"]=>
  array(3) {
    ["one"]=>
    string(1) "1"
    ["pi"]=>
    string(4) "3.14"
    ["string"]=>
    string(14) "this is a test"
  }
  ["session2"]=>
  array(1) {
    ["array"]=>
    array(3) {
      [0]=>
      string(2) "11"
      [1]=>
      string(2) "22"
      [2]=>
      string(2) "33"
    }
  }
}
```

### 例子2

配置文件`test2.ini`

```
[base]
host=localhost
user=testuser
pass=testpass
database=default

[users:base]
pass=testpass-users
database=users

[archive : base]
host = 127.0.0.1
database=archive
```

### 代码3

```
function parse_ini_file_extended($filename)
{
    $config = parse_ini_file($filename, true);
    $result = array();
    foreach ($config as $section=>$subConfig){
        if(strpos($section,':')===false){
            $result[$section] = $subConfig;
            continue;
        }

        list($section1, $section2) = explode(':', $section, 2);
        $section1 = trim($section1);
        $section2 = trim($section2);
        if(isset($config[$section2])){
            $result[$section1] = array_merge($config[$section2], $config[$section]);
        }else{
            $result[$section1] = $config[$section];
        }
    }
    return $result;
}
var_dump(parse_ini_file_extended('test2.ini'));
```

输出

```
array(3) {
  ["base"]=>
  array(4) {
    ["host"]=>
    string(9) "localhost"
    ["user"]=>
    string(8) "testuser"
    ["pass"]=>
    string(8) "testpass"
    ["database"]=>
    string(7) "default"
  }
  ["users"]=>
  array(4) {
    ["host"]=>
    string(9) "localhost"
    ["user"]=>
    string(8) "testuser"
    ["pass"]=>
    string(14) "testpass-users"
    ["database"]=>
    string(5) "users"
  }
  ["archive"]=>
  array(4) {
    ["host"]=>
    string(9) "127.0.0.1"
    ["user"]=>
    string(8) "testuser"
    ["pass"]=>
    string(8) "testpass"
    ["database"]=>
    string(7) "archive"
  }
}
```

### 参考

- [parse_ini_file](http://php.net/manual/zh/function.parse-ini-file.php)