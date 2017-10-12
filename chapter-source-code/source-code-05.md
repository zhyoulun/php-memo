## 基于C语言的简单HTTP服务器

tinyHttpd主页：[http://tinyhttpd.sourceforge.net/](http://tinyhttpd.sourceforge.net/)

### 基础代码

这里将接受到的用户请求放在线程中处理，即执行函数`void accept_request(int client)`。

```
#include <stdio.h>
#include <sys/socket.h>
#include <sys/types.h>
#include <netinet/in.h>
#include <arpa/inet.h>
#include <unistd.h>
#include <ctype.h>
#include <strings.h>
#include <string.h>
#include <sys/stat.h>
#include <pthread.h>
#include <sys/wait.h>
#include <stdlib.h>


void error_die(const char *sc);

void accept_request(int client);

int startup(u_short *port);

/**
 * 报错并终止程序
 * @param sc
 */
void error_die(const char *sc) {
    perror(sc);
    exit(1);
}

/**
 * 接收用户请求
 * @param client
 */
void accept_request(int client) {
    printf("accept_request\n");//在服务端打印

    close(client);
}

/**
 * 设置端口并初始化服务端socket
 * 如果端口为0，则会动态指定一个端口
 * @param port
 * @return
 */
int startup(u_short *port) {
    int httpd = 0;
    struct sockaddr_in name;

    httpd = socket(PF_INET, SOCK_STREAM, 0);
    if (httpd == -1)
        error_die("socket");
    memset(&name, 0, sizeof(name));
    name.sin_family = AF_INET;
    name.sin_port = htons(*port);//host to network short
    name.sin_addr.s_addr = htonl(INADDR_ANY);//host to network long
    if (bind(httpd, (struct sockaddr *) &name, sizeof(name)) < 0)
        error_die("bind");
    if (*port == 0)  /* 动态分配端口 */
    {
        socklen_t name_len = sizeof(name);
        if (getsockname(httpd, (struct sockaddr *) &name, &name_len) == -1)
            error_die("getsockname");
        *port = ntohs(name.sin_port);//network to host short
    }
    if (listen(httpd, 5) < 0)
        error_die("listen");
    return (httpd);
}

int main(void) {
    int server_sock = -1;
    u_short port = 23456;//简化问题，指定服务端的端口为23456
    int client_sock = -1;
    struct sockaddr_in client_name;
    socklen_t client_name_len = sizeof(client_name);
    pthread_t thread;

    server_sock = startup(&port);
    printf("httpd running on port %d\n", port);

    while (1) {
        client_sock = accept(server_sock,
                             (struct sockaddr *) &client_name,
                             &client_name_len);
        if (client_sock == -1)
            error_die("accept");
        /* accept_request(client_sock); */
        if (pthread_create(&thread, NULL, accept_request, client_sock) != 0)
            perror("pthread_create");
    }

    close(server_sock);

    return (0);
}
```

编译并执行`gcc -Wall -lpthread test.c -o test && ./test`，发起多次curl请求，会得到如下结果

```
httpd running on port 23456
accept_request
accept_request
```



### 参考

- [https://stackoverflow.com/questions/176409/build-a-simple-http-server-in-c](https://stackoverflow.com/questions/176409/build-a-simple-http-server-in-c)