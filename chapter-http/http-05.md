## python调用curl获取网页

使用了pycurl轮子

### 简单请求

如下代码可以获取到网页源代码

```
import pycurl
from StringIO import StringIO

def main():
    buffer  = StringIO()
    c = pycurl.Curl()
    c.setopt(c.URL, 'http://www.fabuler.cn')
    c.setopt(c.WRITEDATA, buffer)
    c.perform()
    c.close()

    body = buffer.getvalue()
    print str(body)

if __name__ == '__main__':
    main()
```

### 处理charset=gb2312

如果不处理，会出现乱码

```
import pycurl
from StringIO import StringIO

def main():
    buffer  = StringIO()
    c = pycurl.Curl()
    c.setopt(c.URL, 'http://www.kanunu8.com')
    c.setopt(c.WRITEDATA, buffer)
    c.perform()
    c.close()

    body = buffer.getvalue()
    print str(body).decode('gbk').encode('utf8')

if __name__ == '__main__':
    main()
```

### 指定user-agent

```
import pycurl
from StringIO import StringIO

def main():
    buffer  = StringIO()
    c = pycurl.Curl()
    c.setopt(c.URL, 'http://www.fabuler.cn')
    c.setopt(c.USERAGENT, 'Mozilla/5.0 (Windows NT 6.1; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/61.0.3163.100 Safari/537.36')
    c.setopt(c.WRITEDATA, buffer)
    c.perform()
    c.close()

    body = buffer.getvalue()
    print str(body)

if __name__ == '__main__':
    main()
```

### 捕获异常

```
import pycurl
from StringIO import StringIO

def main():
    try:
        buffer = StringIO()
        c = pycurl.Curl()
        c.setopt(c.URL, 'https://www.baidu.com')
        c.setopt(c.WRITEDATA, buffer)
        c.perform()
        c.close()

        body = buffer.getvalue()
        print str(body)
    except pycurl.error as e:
        print e

if __name__ == '__main__':
    main()
```

### 抓取https网页

```
import pycurl
from StringIO import StringIO

def main():
    try:
        buffer = StringIO()
        c = pycurl.Curl()
        c.setopt(c.URL, 'https://www.zhihu.com')
        
		c.setopt(c.SSL_VERIFYPEER, 0)
        c.setopt(c.SSL_VERIFYHOST, 0)
		
        c.setopt(c.WRITEDATA, buffer)
        c.perform()
        c.close()

        body = buffer.getvalue()
        print str(body)
    except pycurl.error as e:
        print e

if __name__ == '__main__':
    main()
```