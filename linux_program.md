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
	$(CC) $(CFLAGS) $< -o $@

.PHONY: clean rebuild all

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

### 创建目录
+ `mkdir()` 函数可以用来创建目录。

![创建目录](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250519164757.png)

+ 代码：

```c
#include <unistd.h>
#include <stdio.h>
#include <sys/stat.h>
#include <sys/types.h>
#include <stdlib.h>
#include <error.h>
#include <errno.h>

int main(int argc, char* argv[])
{
    // ./test_mkdir dir mode(八进制)
    //参数校验
    if(argc != 3)
    {
        error(1, 0, "Usage %s dir mode",argv[0]);
    }
    
    //参数类型转换
    mode_t mode;
    sscanf(argv[2], "%o", &mode);
    int err = mkdir(argv[1], mode);
    if(err == -1)
    {
        error(1, errno, "mkdir %s",argv[1]);
    }
    return 0;
}
```

+ 当不知道 `mode_t` 的类型是什么的时候，可以使用以下方法来查看：

```sh
gcc -E test_mkdir.c -o test_mkdir.i
grep -nE "mode_t" test_mkdir.i
```

+ 运行结果：

![运行结果](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250519165619.png)

### 删除空目录
+ `rmdir()` 可以删除空目录。

![删除空目录](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250519171201.png)

+ 代码：

```c
#include <unistd.h>
#include <stdio.h>
#include <sys/stat.h>
#include <sys/types.h>
#include <stdlib.h>
#include <error.h>
#include <errno.h>

int main(int argc, char* argv[])
{
    // ./test_rmdir dir 
    //参数校验
    if(argc != 2)
    {
        error(1, 0, "Usage %s dir",argv[0]);
    }
    //功能实现
    if(rmdir(argv[1]) == -1)
    {
        error(1, errno, "rmdir %s",argv[1]);
    }
    
    return 0;
}
```

### 目录流
+ 使用目录流，可以查看目录中的内容。
	+ 流模型："流"类似"水流"，顺序访问流中的数据时，是不需要关注位置的。
	+ 目录流与文件流相比，文件流中的基本单位是字符或字节。而目录流中的基本单位是目录项。如下图所示：

![目录流模型](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250519195922.png)

#### 打开目录流
+ `opendir()` 可以打开一个目录，得到一个指向目录流的指针 `DIR*` 。

![打开一个目录](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250519200055.png)

#### 关闭目录流
+ `closedir()` 关闭目录流。

![关闭目录流](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250519200152.png)

#### 读取目录流
+ `readdir()` 读目录流，得到指向下一个目录项的指针。

![读取目录流](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250519200337.png)

+ 结构体 `dirent` 的定义：

![结构体 dirent 的定义](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250519200424.png)

+ 读取一个目录内的内容并打印的代码：

```c
#include <unistd.h>
#include <stdio.h>
#include <sys/stat.h>
#include <sys/types.h>
#include <stdlib.h>
#include <error.h>
#include <errno.h>
#include <dirent.h>

int main(int argc, char* argv[])
{
    // ./test_dirent dir
    //参数校验
    if(argc != 2)
    {
        error(1, 0, "Usage %s dir",argv[0]);
    }
    //打开目录流
    DIR* stream = opendir(argv[1]);
    if(!stream)
    {
        error(1, errno, "opendir %s",argv[1]);
    }
    //处理目录流
    errno = 0; // 等于0表示没出错
    struct dirent* pdirent;
    //readdir():读目录流，得到指向下一个目录项的指针。
    while((pdirent = readdir(stream)) != NULL)
    {
        printf("d_info=%ld, d_off=%ld, d_reclen=%hu, d_type=%d, d_name=%s\n",
        pdirent->d_ino,
        pdirent->d_off,
        pdirent->d_reclen,
        pdirent->d_type,
        pdirent->d_name);
    }
    if(errno != 0)
    {
        error(1, errno, "readdir %s",argv[1]);
    }
    //关闭目录流
    closedir(stream);
    
    return 0;
}
```

#### 递归地打印目录
+ 实现青春版 `tree` 命令：输出内容分为三部分 
	+ 1)目录的名字；
	+ 2)递归打印每一个目录项；
	+ 3)最后是统计信息。
+ 代码：

```c
#include <unistd.h>
#include <stdio.h>
#include <sys/stat.h>
#include <sys/types.h>
#include <stdlib.h>
#include <error.h>
#include <errno.h>
#include <dirent.h>
#include <string.h>

//width:缩进的空格数目
void dfs_print(const char* path, int width);

int directories = 0, files = 0;

int main(int argc, char* argv[])
{
    // 用法: ./young_tree dir
    if(argc != 2)
        error(1, 0, "Usage: %s dir", argv[0]);
    //遍历根节点
    puts(argv[1]);
    //递归打印每一个子树
    dfs_print(argv[1], 4);
    
    printf("\n%d directories, %d files\n", directories, files);
    
    return 0;
}

void dfs_print(const char* path, int width){
    //打开目录流
    DIR* stream = opendir(path);
    if(!stream){
        error(1, errno, "opendir %s", path);
    }
    //遍历每一个目录项
    errno = 0; // 等于0表示没出错
    struct dirent* pdirent;
    //readdir():读目录流，得到指向下一个目录项的指针。
    while((pdirent = readdir(stream)) != NULL){
        char* filename = pdirent->d_name;
        //忽略 . 和 ..
        if(strcmp(filename,".") == 0 || strcmp(filename,"..") == 0)
            continue;
        //打印这个目录项的名字
        for(int i=0; i<width; i++){
            putchar(' ');
        }
        puts(filename);
        //如果是目录的话递归地处理
        if(pdirent->d_type == DT_DIR){
            directories++;
            //拼接路径
            char subpath[128];
            sprintf(subpath, "%s/%s", path, filename);
            dfs_print(subpath, width+4);
        }
        else
            files++;
    }
    closedir(stream);
    if(errno){
        error(1, errno, "readdir");
    }
}
```

+ 效果：

![递归打印目录实验结果](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250528165342.png)

## 文件相关操作
### 打开文件
+ `open()` 打开文件。

![open() 函数的用法](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250519204706.png)

+ 部分 `flags` 的含义：

![部分 flags 的含义](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250519204812.png)

+ 代码：

```c
#include <unistd.h>
#include <stdio.h>
#include <sys/stat.h>
#include <sys/types.h>
#include <stdlib.h>
#include <error.h>
#include <errno.h>
#include <fcntl.h>

int main(int argc, char* argv[])
{
    // ./test_open file
    //参数校验
    if(argc != 2)
    {
        error(1, 0, "Usage %s file",argv[0]);
    }
    //打开文件，获取文件描述符
    int fd = open(argv[1], O_RDWR | O_CREAT);
    if(fd == -1)
    {
        error(1, errno, "Usage %s", argv[1]);
    }
    //打印获取到的文件描述符
    printf("%d\n",fd);
    
    return 0;
}
```

### 内核管理文件的数据结构
+ 通过库函数 `open()` 的过程介绍内核管理文件的数据结构：

![内核管理文件的数据结构](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250519210649.png)
