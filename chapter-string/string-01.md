## 生成随机字符串

### 方法1

```
function randomString($length)
{
    $dict = '0123456789abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ';
    $dictLen = strlen($dict);
    $result = '';
    for($i=0;$i<$length;$i++){
        $result .= $dict[rand(0, $dictLen-1)];
    }
    return $result;
}
```

> rand ($min, $max) 生成大于等于$min且小于等于$max的一个随机值

### 方法2

```
function randomString($length)
{
    if($length%2==0){
        return bin2hex(openssl_random_pseudo_bytes($length/2));
    }else{
        return substr(bin2hex(openssl_random_pseudo_bytes(ceil($length/2))), 1);
    }
}
```

生成的字符范围是0-9,a-e

> string openssl_random_pseudo_bytes ( int $length [, bool &$crypto_strong ] ) 生成一个伪随机字节串
> 依赖openssl扩展

### 方法3

```
function randomString($length)
{
    return substr(str_shuffle(str_repeat("0123456789abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ", $length)), 0, $length);
}
```

速度 2>1>3，1的范围有限制

### 参考

- [PHP random string generator](https://stackoverflow.com/questions/4356289/php-random-string-generator)
- [openssl_random_pseudo_bytes](http://php.net/manual/zh/function.openssl-random-pseudo-bytes.php)