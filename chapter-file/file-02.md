## 使用文件缓存

使用文件的修改时间作为是否过期时间

### 代码

这里使用了一个单例模式。

```
<?php

class FileCache
{
    private static $instance;
    private $baseFolder;

    private function __construct()
    {
        $this->baseFolder = '/tmp/';
    }

    private function __clone()
    {
    }

    public static function getInstance()
    {
        if (!(self::$instance instanceof self)) {
            self::$instance = new self();
        }
        return self::$instance;
    }

    /**
     * @param string $key 键
     * @param string $value 值
     * @param integer $duration 时长
     * @return bool
     */
    public function set($key, $value, $duration)
    {
        $file = $this->getFile($key);
        $r = file_put_contents($file, $value);
        if ($r === false) {
            return false;
        }

        $r = touch($file, time() + $duration);//更新修改时间
        if($r===false){
            return false;
        }

        return true;
    }

    /**
     * @param string $key 键
     * @return string|false 成功返回字符串，失败返回false
     */
    public function get($key)
    {
        $file = $this->getFile($key);
        if(!file_exists($file)){
            return false;
        }

        $r = filemtime($file);//获取修改时间
        if($r===false){
            return false;
        }

        if($r>time()){//判断修改时间是否超时
            return file_get_contents($file);
        }
        return false;
    }

    /**
     * 获取key对应的文件
     * @param string $key
     * @return string
     */
    private function getFile($key)
    {
        return $this->baseFolder . $key;
    }

}
```

> 查看文件时间信息 stat file.txt
> bool touch ( string $filename [, int $time = time() [, int $atime ]] ) 设定文件的访问和修改时间

### 参考

- [yii\caching\FileCache文档](http://www.yiichina.com/doc/api/2.0/yii-caching-filecache)
- [yii\caching\FileCache源码](https://github.com/yiisoft/yii2/blob/master/framework/caching/FileCache.php)