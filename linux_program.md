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
+ 系统调用 `open()` 打开文件。
	+ 打开成功：返回新的文件描述符（最小可用的文件描述符）；
	+ 打开失败：返回 `-1`，设置 `errno` 。

![open() 系统调用的用法](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250519204706.png)

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

+ 通过系统调用 `open()` 的过程介绍内核管理文件的数据结构：
	+ 文件描述符表的数组长度默认是 1024，并且可以进行设置： 

![文件描述符表的数组长度默认是 1024](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250603171236.png)

+ 系统调用 `open()` 的内核管理文件流程：

![open()系统调用内核管理文件流程](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250519210649.png)

### 关闭文件
+ 系统调用 `close()` 关闭文件。
	+ 关闭成功：返回 `0` ；
	+ 关闭失败：返回 `-1`，设置 `errno` 。
+ 系统调用 `close()` 的内核管理文件流程：

![close()系统调用内核管理文件流程](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250603195205.png)

### 读取文件
+ 系统调用 `read()` 读取文件。
	+ 读取成功：返回实际读取的字节数目（`0`，表示读取的起始位置在文件末尾）；
	+ 读取失败：返回 `-1`，设置 `errno` 。

![系统调用read()读取文件](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250603195627.png)

+ 系统调用 `read()` 的内核管理文件流程：

![read()系统调用内核管理文件流程](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250603195750.png)

### 写入文件
+ 系统调用 `write()` 读取文件。
	+ 写入成功：返回实际写入字节数目（其中 `n≤count` ）；
	+ 写入失败：返回 `-1`，设置 `errno` 。

![write()系统调用](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250603200844.png)

+ 系统调用 `write()` 的内核管理文件流程：

![write()系统调用内核管理文件流程](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250603200953.png)

### 修改文件偏移量
+ 系统调用 `lseek()` 修改文件偏移量，本质上就是修改 `current file offset (pos)` 的数值。
	+ 修改成功：返回文件的位置（距离文件开头的字节数目）；
	+ 修改失败：返回 `-1`，设置 `errno` 。

![系统调用 lseek() 修改文件偏移量](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250603205059.png)

+ 系统调用 `lseek()` 的内核管理文件流程：

![系统调用 lseek() 的内核管理文件流程](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250603205158.png)

+ 文件库函数和文件系统调用之间的关系：

![文件库函数和文件系统调用之间的关系](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250603205358.png)

### 文件描述符和文件流的异同
+ 文件描述符的系统调用与文件流的函数：

![文件描述符的系统调用与文件流的函数](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250605145014.png)

+ 文件描述符和文件流的数据访问流程：

![文件描述符和文件流的数据访问流程](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250605145112.png)

### 同步内存中所有已修改的文件数据到储存设备
+ 函数 `fsync()` 同步内存中所有已修改的文件数据到储存设备。
	+ 将和文件描述符相关联的脏页刷新到磁盘。
	+ 刷新成功：返回 `0`；
	+ 刷新失败：返回 `-1` 并设置 `errno`。

![系统调用 fsync()](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250605145633.png)

### 修改文件长度
+ 函数 `ftruncate()` 修改文件长度为 `length` 个字节大小。
+ 用法如下：

![系统调用 ftruncate() 的用法](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250605161558.png)

+ 修改文件的长度有两种情况：
	+ 情况二可能会出现文件空洞的情况，此时数据全为 `0` 的页不会分配磁盘空间。

![修改文件的长度的两种情况](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250605161818.png)

+ 使用代码示例：

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
    // ./test_ftruncate file length
    if(argc != 3){
        error(1, 0, "Usage: %s file length", argv[0]);
    }
    
    off_t length; // 要截断的文件长度
    sscanf(argv[2], "%ld", &length);
    
    int fd = open(argv[1], O_RDWR);
    if(fd == -1){
        error(1, errno, "open %s", argv[1]);
    }
    
    if(ftruncate(fd, length) == -1){
        error(1, errno, "ftruncate %d", fd);
    } // 截断成功
    
    close(fd);
    
    return 0;
}
```

+ 关于文件空洞的测试：

![关于文件空洞的测试](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250605162213.png)

### 获取文件状态信息
+ 函数 `fstat()` 用于获取文件状态信息。
	+ 获取成功：返回 `0`；
	+ 获取失败：返回 `-1`，设置 `errno`。
+ 其具体用法和状态信息结构体如下：

![fstat() 的具体用法和状态信息结构体](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250605165212.png)

+ 代码示例：

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
    // ./test_fstat file
    if(argc != 2){
        error(1, 0, "Usage %s file", argv[0]);
    }
    
    int fd = open(argv[1], O_RDONLY);
    if(fd == -1){
        error(1, errno, "open %s", argv[1]);
    }
    
    struct stat sb; // 存储文件元数据信息
    if(fstat(fd, &sb) == -1){
        error(1, errno, "fstat %d", fd);
    }
    
    // 打印 struct stat 里面的成员信息
    printf("st_ino=%ld\nst_mode=%lo\nst_nlink=%ld\nst_size=%ld\nst_blocks=%ld\n",
            (long)sb.st_ino,
            (long)sb.st_mode,
            (long)sb.st_nlink,
            (long)sb.st_size,
            (long)sb.st_blocks);
            
    return 0;
}
```

+ 打印结果：

![打印结果](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250605165347.png)

### 文件描述符的复制
+ 系统调用 `dup()` 用于对文件描述符的复制。
	+ 复制成功：返回新的文件描述符；
	+ 复制失败：返回 `-1`，设置 `errno`。
+ 其具体用法如下，分为 `dup()` 和 `dup2()` 两个系统调用：

![两个文件描述符系统调用的具体用法](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250606173642.png)

+ 系统调用 `dup()` 的内核管理文件流程：

![系统调用 dup() 的内核管理文件流程](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250606174115.png)

+ 使用 `dup()` 函数实现重定向：

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
    // 对 stderr 重定向
    int fd = open("application.log", O_RDWR | O_CREAT | O_APPEND, 0664);
    if (fd == -1){
        error(1, errno, "open application.log");
    }
    
    // STDERR_FILENO 标准错误输出的文件描述符
    write(STDERR_FILENO, "The first error massage\n", 24);
    
    //对 stderr 进行重定向
    close(STDERR_FILENO);
    dup(fd);
    
    write(STDERR_FILENO, "The second error massage\n", 25);
    
    return 0;
}
```

+ 使用 `dup2()` 函数实现重定向：

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
    // 对 stderr 重定向
    int fd = open("application.log", O_RDWR | O_CREAT | O_APPEND, 0664);
    if (fd == -1){
        error(1, errno, "open application.log");
    }
    
    // STDERR_FILENO 标准错误输出的文件描述符
    write(STDERR_FILENO, "The first error massage\n", 24);
    
    //对 stderr 进行重定向
    //close(STDERR_FILENO);
    if(dup2(fd, STDERR_FILENO) == -1){
        error(1, errno, "dup2 %d %d", fd, STDERR_FILENO);
    }
    
    write(STDERR_FILENO, "The second error massage\n", 25);
    
    return 0;
}
```

+ 分别执行完两个代码的运行结果：
	+ 第一个代码重定向 `STDERR_FILENO` 文件描述符到 `application.log` 文件，写入一段语句；
	+ 第二个代码同样重定向 `STDERR_FILENO` 文件描述符到 `application.log` 文件，写入一段语句。

![分别执行完两个代码的运行结果](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250606174455.png)

### 零拷贝（mmap）
+ 系统调用零拷贝（ `mmap()` ）能够省去内核态和用户态的内存拷贝。

![零拷贝](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/202506071540394.png)

+ 系统调用零拷贝（ `mmap()` ）的工作原理：
	+ 相当于将文件的一部分内存通过 `I/O` 操作拷贝到物理内存中，并且内核态和用户态共用这部分内存，不再需要重复拷贝。

![image.png](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/202506071542883.png)

+ 系统调用零拷贝（ `mmap()` 和 `munmap()` ）的使用方法：

![系统调用零拷贝 mmap() 和 munmap() 的使用方法](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/202506071600326.png)

+ 系统调用零拷贝（ `mmap()` 和 `munmap()` ）的返回值：

![系统调用零拷贝 mmap() 和 munmap() 的返回值](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/202506071602492.png)

+ 系统调用零拷贝（ `mmap()` 和 `munmap()` ）的复制大文件使用场景：

![系统调用零拷贝 mmap() 和 munmap() 的复制大文件使用场景](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/202506071651605.png)

+ 代码示例：

```c
#include <unistd.h>
#include <stdio.h>
#include <sys/stat.h>
#include <sys/types.h>
#include <stdlib.h>
#include <error.h>
#include <errno.h>
#include <fcntl.h>
#include <string.h>
#include <sys/mman.h>

// offset 应该是页大小的整数倍
#define MMAP_SIZE (4096 * 10)

int main(int argc, char *argv[])
{
    // ./mmap_cp src dst
    if (argc != 3){
        error(1, 0, "Usage: %s src dst", argv[0]);
    }
    
    int srcfd = open(argv[1], O_RDONLY);
    if (srcfd == -1){
        error(1, errno, "open %s", argv[1]);
    }
    // 需要读写权限
    int dstfd = open(argv[2], O_RDWR | O_CREAT | O_TRUNC, 0666);
    if (dstfd == -1){
        close(srcfd);
        error(1, errno, "open %s", argv[2]);
    }
    // 1.需要事先知道文件大小
    // 获取src大小
    struct stat sb;
    fstat(srcfd, &sb);
    off_t fsize = sb.st_size;
    // 将dst文件大小设置为fsize
    ftruncate(dstfd, fsize); // 目标文件大小需要事先固定
    // 设置初始偏移量，表示已经复制的数据
    off_t offset = 0;
    while (offset < fsize)
    {
        // 计算映射区长度
        off_t length;
        if (fsize - offset >= MMAP_SIZE)
            length = MMAP_SIZE;
        else
            length = fsize - offset;
        // 映射
        void *addr1 = mmap(NULL, length, PROT_READ, MAP_SHARED, srcfd, offset);
        if (addr1 == MAP_FAILED){
            error(1, errno, "mmap %s", argv[1]);
        }
        
        void *addr2 = mmap(NULL, length, PROT_READ | PROT_WRITE, MAP_SHARED, dstfd, offset);
        if (addr2 == MAP_FAILED){
            error(1, errno, "mmap %s", argv[2]);
        }
        // 通过将addr1的内容复制到addr2中实现文件的复制
        memcpy(addr2, addr1, length);
        offset += length;
        // 解除映射
        int err = munmap(addr1, length);
        if (err == -1){
            error(1, errno, "munmap %s", argv[1]);
        }
        
        err = munmap(addr2, length);
        if (err == -1){
            error(1, errno, "munmap %s", argv[2]);
        }
    } // offset == fsize
    return 0;
}
```