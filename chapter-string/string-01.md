## 生成随机字符串

### 方法1

循环生成随机字符串中的每一个字符，将字符串连接起来构成字符串并返回。

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

使用openssl中的`openssl_random_pseudo_bytes()`函数，生成一个伪随机字符串，其中每一位是一个字节；然后再由`bin2hex()`函数将一个字节转换成两个字符，每个字符的范围是`0-9a-f`。

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

生成的字符范围是0-9,a-f

> string openssl_random_pseudo_bytes ( int $length [, bool &$crypto_strong ] ) 生成一个伪随机字节串
> 依赖openssl扩展

### 方法3

使用`str_repeat()`将候选集重复指定长度次，然后使用`str_shuffle()`将字符串打乱，从中取出要求长度的字符串作为结果返回。可以看出，这种方式占用内存较多。

```
function randomString($length)
{
    return substr(str_shuffle(str_repeat("0123456789abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ", $length)), 0, $length);
}
```

对于以上三种方法，多次遍历得到的结果是，速度2>1>3，2最好，3最差，但是2的范围有限制。

### 参考

- [PHP random string generator](https://stackoverflow.com/questions/4356289/php-random-string-generator)
- [openssl_random_pseudo_bytes](http://php.net/manual/zh/function.openssl-random-pseudo-bytes.php)