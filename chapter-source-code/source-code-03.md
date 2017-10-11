## sockaddr结构分析

### struct sockaddr_in

`struct sockaddr_in`的长度是16字节，可以通过sizeof函数验证

```
/* Structure describing an Internet (IP) socket address. 
   IP套接字地址结构 */
#define __SOCK_SIZE__	16		/* sizeof(struct sockaddr)	*/
struct sockaddr_in
{
  sa_family_t	 sin_family;	/* Address family 地址族 */
  in_port_t	 sin_port;	/* Port number 端口号，16位TCP或者UDP端口号，网络字节序 */
  struct in_addr sin_addr;	/* Internet address 地址 */

  /* Pad to size of `struct sockaddr'. 补全结构体的长度到struct sockaddr一样的值 */
  unsigned char  __pad[__SOCK_SIZE__ - sizeof(short int)
			- sizeof(unsigned short int) - sizeof(struct in_addr)];
};
```

其中

```
typedef __sa_family_t sa_family_t; /* 16bit */
typedef __uint16_t __sa_family_t;

typedef	__uint16_t	in_port_t;   /* 16bit */

/* Internet address. 地址 */
struct in_addr
{
  in_addr_t s_addr;   /* 32位IPv4地址，网络字节序 */
};
typedef	__uint32_t	in_addr_t;	/* base type for internet address 地址的基础类型 */
```

### POSIX规范要求的数据类型

|数据类型|说明|头文件|
|--|--|--|
| int8_t | 带符号的8位整数 | <sys/types.h> |
| uint8_t | 无符号的8位整数 | <sys/types.h> |
| int16_t | 带符号的16位整数 | <sys/types.h> |
| uint16_t | 无符号的16位整数 | <sys/types.h> |
| int32_t | 带符号的32位整数 | <sys/types.h> |
| uint32_t | 无符号的32位整数 | <sys/types.h> |
| sa\_family\_t | 套接字地址结构的地址族 | <sys/socket.h> |
| socklen_t | 套接字地址结构的长度，一般为32位 | <sys/socket.h> |
| in\_addr\_t | IPv4地址，一般为uint32_t | <netinet/in.h> |
| in\_port\_t | TCP或者UDP端口，一般为uint16_t | <netinet/in.h> |