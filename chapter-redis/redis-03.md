## socket连接redis

### 代码

```
<?php
class RedisConnection
{
    private $hostname = 'localhost';
    private $port = 9379;
    private $unixSocket;
    private $password;
    private $database = 0;
    private $connectionTimeout = null;
    private $dataTimeout = null;
    private $socketClientFlags = STREAM_CLIENT_CONNECT;
    private $socket = false;

    public function open()
    {
        if($this->socket!==false){
            return;
        }

        $this->socket = stream_socket_client(
            $this->unixSocket?'unix://'.$this->unixSocket:'tcp://'.$this->hostname.':'.$this->port,
            $errorNumber,
            $errorDescription,
            $this->connectionTimeout?$this->connectionTimeout:ini_get('default_socket_timeout'),
            $this->socketClientFlags
        );
    }
}
```


### 参考

- [https://github.com/yiisoft/yii2-redis/blob/master/Connection.php](https://github.com/yiisoft/yii2-redis/blob/master/Connection.php)