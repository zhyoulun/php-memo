## 使用UserAgent判断是否为移动端

### 代码

```
$userAgent = $_SERVER['User-Agent'];
if(1===preg_match("/AppleWebKit.*Mobile/i", $userAgent) ||
    1===preg_match("/(MIDP|SymbianOS|NOKIA|SAMSUNG|LG|NEC|TCL|Alcatel|BIRD|DBTEL|Dopod|PHILIPS|HAIER|LENOVO|MOT-|Nokia|SonyEricsson|SIE-|Amoi|ZTE)/",$userAgent)){
    //重定向到移动端
}
```
