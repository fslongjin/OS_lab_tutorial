---
sidebar: auto
---

# 进程创建与进程间通信

## 本章导读

通过练习理解进程创建以及进程并发执行的过程，握fork系统调用的方法，了解进程间通信的常用方法。

## 实验内容

### 进程创建

请问在下方的代码中，一共有多少个进程被创建？

>可以通过fork返回的值来判断当前进程是子进程还是父进程。

```c
#include <unistd.h>
#include <stdio.h>
 
int main()
{
    fork();
    fork() && fork() || fork();
    fork();
    sleep(100);
    return 0;
}
```

### 创建进程树

编写程序创建进程树如图所示，在每个进程中显示当前进程标识getpid()和父进程标识getppid()。

![图片1](../.vuepress/public/tree.png '进程树')

### 信号通信

用fork()创建两个子进程，再用系统调用signal()让父进程捕捉键盘上来的中断信号（即DEL键）；当捕捉到中断信号后，父进程用系统调用kill()向两个子进程发出信号，子进程捕捉到信号后分别输出下列信息后终止：

Child process1 is killed by parent!

Child process2 is killed by parent!

父进程等待两个子进程终止后，输出如下的信息后终止：

Parent process is killed!

### 管道通信

实现进程的管道通信，用系统调用pipe()建立一管道，二个子进程P1和P2分别向管道各写一句话：

Child 1 is sending a message!

Child 2 is sending a message!

父进程从管道中读出二个来自子进程的信息并显示。
