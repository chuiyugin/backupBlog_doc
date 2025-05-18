---
title: Linux 系统编程
tags:
  - Linux
  - ubuntu
categories:
  - Linux
date: 2025-5-18 16:00:00
excerpt: Linux 系统编程
---
# Linux 系统编程
## 通用 Makefile 文件
+ 通用 Makefile 文件的代码如下：

```shell
SRCS := $(wildcard *.c)
Outs := $(patsubst %.c, %, $(SRCS))

CC := gcc
CFLAGS = -Wall -g

all: $(Outs)

%: %.c
	 $(CC) $< -o $@ $(CFLAGS)

.PHONY: clean rebuild ALL

clean:
	$(RM) $(Outs)
rebuild: clean all
```

## 目录相关操作
### 获取当前工作目录
+ 我们可以调用库函数 `getcwd()` 获取当前工作目录的绝对路径：

![库函数 getcwd()](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250518165819.png)

+ 如果传入的 `buf` 为 `NULL` ，且 `size` 为 `0`，则 `getcwd()` 会调用 `malloc` 申请合适大小的内存空间，填入当前工作目录的绝对路径，然后返回 `malloc` 申请的空间的地址。
	+ **注意：** `getcwd()` 不负责 `free` 申请的空间， `free` 是调用者的职责。
+ 代码：

```c
#include <unistd.h>
#include <stdio.h>
#include <stdlib.h>

int main()
{
    char* cwd;
    if((cwd = getcwd(NULL,0)) == NULL)
    {
        //错误处理
        perror("getcwd");
        exit(1);
    }
	//一定处理成功
    puts(cwd);
    free(cwd);//由用户来free

    return 0;
}
```

+ 也可以与库函数 `error()` 来联动进行错误处理，从下图中的数据手册可以看到 `getcwd()` 出错的时候会设置 `errno` 这个参数：

![库函数 getcwd() 的返回值处理](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250518171625.png)

+ 库函数 `error()` 的用法：

![库函数 error() 的用法](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250518171744.png)

+ 代码如下：

```c
#include <unistd.h>
#include <error.h>
#include <errno.h>
#include <stdio.h>
#include <stdlib.h>

int main()
{
    char cwd[20];
    if(getcwd(cwd,20) == NULL)
    {
        //错误处理
        error(1, errno, "getcwd");
    }
    puts(cwd);
    
    return 0;
}
```

+ 运行结果：

![test_error 运行结果](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250518172857.png)

### 改变当前工作目录
+ 我们可以调用库函数 `chdir()` 改变当前工作目录的绝对路径：

![库函数 chdir()](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250518201356.png)

+ 代码：

```c
#include <stdio.h>
#include <unistd.h>
#include <error.h>
#include <errno.h>
#include <stdlib.h>

int main(int argc,char* argv[])
{
    if(argc != 2)
    {
        error(1, errno, "Usage: %s path",argv[0]);
    }
    
    char* cwd;
    if((cwd = getcwd(NULL,0)) == NULL)
    {
        error(1, errno, "getcwd");   
    }
    puts(cwd);
    free(cwd);
    
    //改变目录的惯用法
    if(chdir(argv[1]) == -1)
    {
        error(1, errno, "chdir %s",argv[1]);
    }
    
    if((cwd = getcwd(NULL,0)) == NULL)
    {
        error(1, errno, "getcwd");   
    }
    puts(cwd);
    free(cwd);
    
    return 0;
}
```

+ 注意：当前工作目录是进程的属性，也就是说每一个进程都有自己的当前工作目录。且父进程创建 ( `fork` )子进程的时候，子进程会继承父进程的当前工作目录。
+ 运行结果：

![chdir() 运行结果](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250518205754.png)
