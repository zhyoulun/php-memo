## 判断主机字节序

- 小端字节序（little-endian）：将低序字节存储在起始地址
- 大端字节序（big-endian）：将高序字节存储在起始地址

与主机字节序（host byte order）相对应的是网络字节序（network byte order），网络字节序使用大端字节序来传送多字节整数。例如端口号是16位的，IPv4地址是32位的。

### 代码

使用了union结构体来判断。

```
#include <stdio.h>

int main()
{
    union {
        short s;
        char c[sizeof(short)];
    } un;

    un.s = 0x0102;
    if(sizeof(short)==2){
        if(un.c[0]==1 && un.c[1]==2)
            printf("big-endian\n");
        else if(un.c[0]==2 && un.c[1]==1)
            printf("big-endian\n");
        else
            printf("unknown\n");
    }else{
        printf("sizeof(short) = %d\n", sizeof(short));
    }

    return 0;
}
```