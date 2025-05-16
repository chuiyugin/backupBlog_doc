---
title: Linux 学习笔记
tags:
  - Linux
  - ubuntu
categories:
  - Linux
date: 2025-5-14 16:00:00
excerpt: Linux 学习笔记
---
# Linux 学习笔记
## Linux 模型
+ Linux 模型图：

![Linux 模型图](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250514163438.png)

### Linux 模型简介
+ 内核（ `kernel` ）的作用：
	+ 管理硬件资源：
		+ cpu——>进程调度；
		+ 内存——>内存管理；
		+ 外部设备——>文件管理、网络通信、设备驱动...
	+ 对上层应用程序提供接口（接口：系统调用）
+ 系统调用（ `system calls` ）：内核给上层应用程序提供的接口（ `API` ）
+ 库函数（ `library functions` ）：对系统调用的封装
	+ 例如：
		+ 库函数： `malloc()` -> 系统调用： `sbrk`
		+ 库函数：`printf()` -> 系统调用：`write`
		+ 对于不同的操作系统，它们的系统调用是不一样的
	+ 具有以下性质：
		+ 方便使用
		+ 移植性
+ 命令解释器（ `shell` ）：`shell` 命令
	+ `shell` 命令是一类程序，如 `sh`、`bash`、`csh`、`zsh`、`ksh`...
	+ 命令：一般来说，就是一些简单的可执行程序
	+ 脚本：命令的集合

![shell命令和脚本](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250514164225.png)

## 常用命令
### 查看帮助手册（man）
+ 使用 `man`（ `manual` ）命令可以查看 Linux 内置的帮助手册。该手册分为多卷，比较重要的是前三卷：
	+ 第一卷是用来查看 `shell` 命令的；
	+ 第二卷是用来查看系统调用相关信息的；
	+ 第三卷是用来查看库函数信息的。

![man命令查看手册的不同卷](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250514165254.png)
+ `man` 命令的格式如下：

```shell
man [手册编号] cmd

man man
man mkdir
man 3 mkdir
```

+ 进入帮助界面后，可以采用下列按键浏览帮助信息：

![帮助页按键](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250514165525.png)

### 用户子系统命令
+ 常见命令：
	+ 创建用户：`useradd`
	+ 删除用户：`userdel`
	+ 修改用户密码：`passwd`
	+ 切换用户：`su`
	+ 退出切换：`exit`
+ Linux 用户
	+ `root`：根用户，超级用户，拥有最高权限
	+ `sudoers`：管理用户，安装 `ubuntu` 的时候，创建的用户默认为 `sudoers`
		+ 可以使用 `sudo` 命令，临时提升其权限。
	+ 普通用户

### 文件子系统命令
+ 文件子系统的分门别类：

![文件子系统的分门别类](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250515160215.png)

#### 查看当前工作目录
+ 查看当前工作目录：

```shell
$ pwd
/home/scordingbig
```

#### 改变当前工作目录
+ 改变当前工作目录：

![改变当前工作目录](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250515160433.png)

#### 创建目录
+ 创建目录：

![创建目录](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250515160910.png)

#### 删除空目录
+ 删除空目录：

![删除空目录](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250515161236.png)

#### 通配符
+ 通配符：
	+ 注意：
		+ 匹配文件名字：通配符
		+ 匹配文件内容：正则表达式

![通配符](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250515162658.png)

#### 查看目录内容
+ 查看目录内容：

![查看目录内容](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250515165529.png)

+ `ls -i`：显示物理文件标识

![显示物理文件标识](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250515165815.png)

+ `ls -l` 中的参数含义：

![ls -l 中的参数含义](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250515165921.png)

#### 移动文件和目录
+ `mv` 移动文件和目录
	+ 在同一个目录内是重命名和覆盖

![移动文件和目录](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250515173445.png)

+ 用于重命名或者移动：

![用于重命名或者移动](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250515173858.png)

#### 复制文件或目录
+ `cp` 复制文件或目录

![复制文件或目录](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250515174014.png)

#### 删除文件和目录
+ `rm` 删除文件和目录：

![删除文件和目录](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250515174357.png)

#### 别名
+ `alias` 给命令起别名

![给命令起别名](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250515174846.png)

#### 创建文件
+ `touch` 创建空文件或改变文件时间戳
	+ 文件存在则改变文件时间戳；
	+ 文件不存在则创建空文件。
+ `echo` 创建文件并输入简短内容，例如：

```shell
echo "hello world!" # 仅关联到屏幕
echo "hello world!" > text1 # 重定向到text1这个文件
```

+ `vim` 创建文件和编辑文件

#### 查找文件
+ 使用 `which` 命令来查找**可执行程序**的路径：
	+ `which` 命令是根据 `PATH` 环境变量中的路径依次去查找的，然后显示第一个匹配项，或者显示所有匹配项。

![使用 which 命令来查找可执行程序的路径](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250515183905.png)

+ `PATH` 路径展示：

![PATH 路径展示](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250515183957.png)

+ 使用 `find` 命令查找文件和目录

![使用 find 命令查找文件和目录]( https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250515191451.png )

+ `find` 命令的常见用法：

![find 命令的常见用法](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250515191542.png)

#### 查看文件内容
+ `cat` 命令查看文件：

![cat 命令查看文件](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250515191817.png)

+ `head` 命令查看文件的前几行：

![head 命令查看文件的前几行](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250515191906.png)

+ `tail` 命令查看文件后几行：

![tail 命令查看文件后几行：](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250515192002.png)

+ `less` ( `more` )命令单页浏览文件：

![less ( more )命令单页浏览文件](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250515192112.png)

#### 重定向
+ 重定向：

![重定向](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/202505161600421.png)

+ 常见用法：

![重定向常见用法](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/202505161605070.png)

#### 搜索文件内容
+ `grep` 命令可以用于搜索文件内容，功能非常强大！
+ `grep` 命令按照正则表达式去搜索文件，如果文件中某一项匹配指定的正则表达式，`grep` 命令则会显示这一项。

![grep命令搜索文件内容](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/202505161614234.png)

#### 正则表达式
+ 常用的正则表达式语法规则：

![常用的正则表达式语法规则](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/202505161641348.png)

#### 命令的组合
+ 命令的组合主要有以下三种方式：

![命令的组合](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/202505161715567.png)

+ 第一种组合用法：

![第一种组合用法](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/202505161728972.png)

+ 第二种组合用法：管道

![第二种组合用法：管道](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/202505161729102.png)

+ 第三种组合用法：`xargs`

![第三种组合用法：xargs](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/202505161731637.png)

#### 文件权限
+ 普通文件：

![普通文件的权限](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/202505161741009.png)

+ 目录文件：

![目录文件的权限](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/202505161742598.png)



