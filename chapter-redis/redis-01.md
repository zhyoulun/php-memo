## 操作redis的单例模式

`Redis`类基于PHP扩展[https://pecl.php.net/package/redis](https://pecl.php.net/package/redis)，该扩展的详细用法参考[https://github.com/phpredis/phpredis/](https://github.com/phpredis/phpredis/)。

### 代码

```
class RedisProxy
{
    private static $pool = [];  //连接池
    private $dbName;
    /** @var Redis */
    private $connection;

    /**
     * 这里的配置可以放在专用的配置文件中
     * @var array
     */
    public static $DB_CONFIG = [
        'redis1'=>[
            'host'=>'IP_ADDRESS',
            'port'=>12345,
            'pass'=>'',
            'database'=>0,
            'connect_timeout'=>0.1,
        ],
        'redis2'=>[
            'host'=>'IP_ADDRESS2',
            'port'=>12345,
            'pass'=>'',
            'database'=>0,
            'connect_timeout'=>0.1,
        ],
    ];

    private function __construct($dbName)
    {
        $this->dbName = $dbName;
        $this->connect();
    }

    private function __clone()
    {
    }

    function __destruct()
    {
        if(isset($this->connection)){
            $this->connection->close();
        }
    }

    /**
     * @param string $dbName
     * @return Redis
     */
    public static function getInstance($dbName)
    {
        if (!(isset(self::$pool[$dbName]) && self::$pool[$dbName] instanceof self)) {
            self::$pool[$dbName] = new self($dbName);
        }
        return self::$pool[$dbName];
    }

    //调用Redis类的方法
    public function __call($func, $params)
    {
        $instance = self::$pool[$this->dbName];
        return call_user_func_array(array($instance->connection, $func), $params);
    }

    private function connect()
    {
        $this->connection = new Redis();
        $params = self::$DB_CONFIG[$this->dbName];
        try{
            $this->connection->connect($params['host'], $params['port'], $params['connect_timeout']);
            $this->connection->auth($params['pass']);
            $this->connection->select($params['database']);
        }catch (Exception $e){
            /**
             * 可以依照自己的需求，做定制化的处理
             */
            echo "redis connect error";
            exit(1);
        }
    }
}

//测试代码
$redis = RedisProxy::getInstance('redis1');
$r = $redis->setex('hello', 30, 'world');
var_dump($r);
var_dump($redis->get('hello'));
```

### 参考

- [https://github.com/phpredis/phpredis/](https://github.com/phpredis/phpredis/)
- [https://pecl.php.net/package/redis](https://pecl.php.net/package/redis)