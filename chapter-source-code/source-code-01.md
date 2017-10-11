## C语言函数学习

### perror

- [http://www.cplusplus.com/reference/cstdio/perror/](http://www.cplusplus.com/reference/cstdio/perror/)

将errno的值解释为一句错误信息

例子

```
#include <stdio.h>
int main ()
{
    FILE * pFile;
    pFile=fopen ("unexist.ent","rb");
    if (pFile==NULL)
        perror ("The following error occurred");
    else
        fclose (pFile);
    return 0;
}
```

输出

```
The following error occurred: No such file or directory
```

### vsprintf vsnprintf

- [vsprintf and vsnprintf](http://blog.csdn.net/nichael99/article/details/23994365)
- [http://www.cplusplus.com/reference/cstdio/vsnprintf/](http://www.cplusplus.com/reference/cstdio/vsnprintf/)

### va_list va_start va_arg va_end

- [va_list/va_start/va_arg/va_end深入分析](http://www.cnblogs.com/justinzhang/archive/2011/09/29/2195969.html)
- [ C语言中可变参数的用法——va_list、va_start、va_arg、va_end参数定义](http://blog.csdn.net/edonlii/article/details/8497704)

### bzero memset

- [bzero, memset ,setmem 区别](http://blog.csdn.net/joeblackzqq/article/details/8257877)

