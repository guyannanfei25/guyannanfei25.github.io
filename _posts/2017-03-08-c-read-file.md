---
layout: post
title: c 语言读取文件
categories: c
description: c 语言读写文件示例
keywords: c, 文件读写
---

一直对c语言文件操作不是很熟悉，有个小需求过来，
各种异常情况，搞得也是焦头烂额，正好趁这个机会
梳理下。

文件读写操作主要有fopen、fclose、fread、fwrite、
ftell、fseek几个函数。都在stdio.h头文件中。 
函数原型如下:

    FILE *fopen(const char *path, const char *mode);
    int fclose(FILE *fp);
    size_t fread(void *ptr, size_t size, size_t nmemb, FILE *stream);
    size_t fwrite(const void *ptr, size_t size, size_t nmemb, FILE *stream);
    int fseek(FILE *stream, long offset, int whence);
    long ftell(FILE *stream);
    void clearerr(FILE *stream);
    int feof(FILE *stream);
    int ferror(FILE *stream);
    int fileno(FILE *stream);

重点说明下fread，man fread 解释如下：

    The  function  fread() reads nmemb elements of data, each size bytes long, from the stream pointed to by stream, storing them at the location given
    by ptr.


读取文件示例代码：
```c
#include <stdio.h>                                                
#define bufLen 1024                                            

int main(int argc, char **argv) {                              
    if (argc != 2) {                                           
        printf("usage:\n");                                    
        printf("      %s file_to_be_echo_name\n", argv[0]);    
        return -1;                                             
    }                                                          
                                                               
    char buf[bufLen];                                          
    FILE* fp = fopen(argv[1], "rb");                           
    if (ferror(fp)) {                                          
        printf("Open %s err\n", argv[1]);                      
        return -2;                                             
    }                                                          
                                                               
    while(!feof(fp)) {                                         
        // read len buflen - 1, to ensure buf is c style string
        size_t size = fread(buf, 1, bufLen - 1, fp);           
        buf[size] = '\0';                                      
        printf("%s", buf);                                     
    }                                                          
    fclose(fp);
    return 0;                                                  
}
```                                                           

## tips 

- fopen设置追加模式的时候，a需要放在最前面

## 进阶话题
fread返回的是读取的字节数，但是不一定会等于nmemb，在有些情况下需要保证返回的字节数
一定要和nmemb相等。应该怎么保证呢？

管道、FIFO以及某些设备，特别是终端、网络和STREAMS设备有下列两种性质：

（1）一次read操作所返回的数据可能少于所要求的数据，即使还没有达到文件尾端也可能是这样。
**这不是一个错误** ，应当继续读该设备。

（2）一次write操作的返回值也可能少于指定输出的字节数。这可能是由若干因素造成的，例如，
下游模块的流量控制限制。 **这也不是错误** ，应当继续写余下的数据至该设备。（通常，只有对
非阻塞描述符，或捕捉到一个信号时，才发生这种write的中途返回。）

在读、写磁盘文件时从未见到过这种情况，除非文件系统用完了空间，或者我们接近了配额限制，而不能将要求写的数据全部写出。

unp中已经有现成的代码可参考：

```c
#include "unp.h"
ssize_t readn(int filedes, void *buff, size_t nbytes);
ssize_t writen(int filedes, const void *buff, size_t nbytes);
ssize_t readline(int filedes, void *buff, size_t maxlen);
```
具体实现
```c
ssize_t readn(int fd, void *vptr, size_t n) {
    size_t nleft;
    ssize_t nread;
    char *ptr;

    ptr = vptr;
    nleft = n;
    while(nleft > 0) {
        if ((nread = read(fd, ptr, nleft)) < 0) {
            if (errno == EINTR) {
                nread = 0;
            } else {
                return -1;
            }
        } else if (nread == 0) {
            break;  // EOF
        }

        nleft -= nread;
        ptr   += nread;
    }

    return (n - nleft);
}

ssize_t writen(int fd, const void *vptr, size_t n) {
    size_t nleft;
    ssize_t nwritten;
    const char *ptr;

    nleft = n;
    ptr   = vptr;

    while (nleft > 0) {
        if ((nwritten = write(fd, ptr, nleft)) <= 0) {
            if (nwritten < 0 && errno == EINTR) {
                nwritten = 0;
            } else {
                return -1;
            }
        }

        nleft -= nwritten;
        ptr   += nwritten;
    }

    return n;
}
```


> 参考unp
