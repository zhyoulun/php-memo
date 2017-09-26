## redis读写分离

`Redis`类基于PHP扩展[https://pecl.php.net/package/redis](https://pecl.php.net/package/redis)，该扩展的详细用法参考[https://github.com/phpredis/phpredis/](https://github.com/phpredis/phpredis/)。

```
<?php
class RedisRWProxy
{
    private static $pool = [];  //连接池
    private $dbName;
    /** @var Redis W库 */
    private $wConnection;
    /** @var Redis R库 */
    private $rConnection;

    /**
     * 这里的配置可以放在专用的配置文件中
     * @var array
     */
    public static $DB_CONFIG = [
        'redis1'=>[
            //写库的配置
            'w'=>[
                'host'=>'W_IP_ADDRESS1',
                'port'=>12345,
                'pass'=>'',
                'database'=>0,
                'connect_timeout'=>0.1,
            ],
            //读库的配置
            'r'=>[
                'host'=>'R_IP_ADDRESS2',
                'port'=>12345,
                'pass'=>'',
                'database'=>0,
                'connect_timeout'=>0.1,
            ],
        ],
        'redis2'=>[
            'w'=>[
                'host'=>'W_IP_ADDRESS3',
                'port'=>12345,
                'pass'=>'',
                'database'=>0,
                'connect_timeout'=>0.1,
            ],
            'r'=>[
                'host'=>'R_IP_ADDRESS4',
                'port'=>12345,
                'pass'=>'',
                'database'=>0,
                'connect_timeout'=>0.1,
            ],
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
        if(isset($this->wConnection)) $this->wConnection->close();
        if(isset($this->rConnection)) $this->rConnection->close();
    }

    /**
     * @param $dbName
     * @return RedisRWProxy
     */
    public static function getInstance($dbName)
    {
        if (!(isset(self::$pool[$dbName]) && self::$pool[$dbName] instanceof self)) {
            self::$pool[$dbName] = new self($dbName);
        }
        return self::$pool[$dbName];
    }

    public function __call($func, $params)
    {
        $instance = self::$pool[$this->dbName];
        return call_user_func_array(array($instance->connection, $func), $params);
    }

    private function connect()
    {
        //获取连接配置
        $rwParams = self::$DB_CONFIG[$this->dbName];

        $params = $rwParams['w'];
        $this->wConnection = new Redis();
        try{
            $this->wConnection->connect($params['host'], $params['port'], $params['connect_timeout']);
            $this->wConnection->auth($params['pass']);
            $this->wConnection->select($params['database']);
        }catch (Exception $e){
            /**
             * 可以依照自己的需求，做定制化的处理
             */
            echo "redis connect error";
            exit(1);
        }

        $params = $rwParams['r'];
        $this->rConnection = new Redis();
        try{
            $this->rConnection->connect($params['host'], $params['port'], $params['connect_timeout']);
            $this->rConnection->auth($params['pass']);
            $this->rConnection->select($params['database']);
        }catch (Exception $e){
            /**
             * 可以依照自己的需求，做定制化的处理
             */
            echo "redis connect error";
            exit(1);
        }
    }

    /**
     * 写入W库
     * @param $key
     * @param $value
     * @param int $timeout
     * @return bool
     */
    public function set($key, $value, $timeout=0)
    {
        return $this->wConnection->set($key, $value, $timeout);
    }

    /**
     * 从R库读取
     * @param $key
     * @return bool|string
     */
    public function get($key)
    {
        return $this->rConnection->get($key);
    }

    /**
     * 其它读写操作可以在这里接着复写
     */
}

//测试代码
$redis = RedisRWProxy::getInstance('redis1');
$r = $redis->set('hello', 'world', 30);
var_dump($r);
var_dump($redis->get('hello'));
```

### 参考

- [https://github.com/phpredis/phpredis/](https://github.com/phpredis/phpredis/)
- [https://pecl.php.net/package/redis](https://pecl.php.net/package/redis)