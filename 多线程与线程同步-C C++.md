# 多线程与线程同步-C/C++

## 1.线程概述

线程是轻量级的进程，在linux环境下本质就是进程。在计算机上运行的程序是一组指令及指令参数的组合，指令按照既定的逻辑控制计算机运行。操作系统会进程为单位，分配系统资源，**进程是资源分配的最小单位，线程是操作系统调度执行的最小单位**

* 进程有自己独立的地址空间，多个线程共用一个地址空间
    * 线程更加节省系统资源，效率可以保持，甚至提高
    * 在一个地址空间中多个线程独享：每个线程都有属于自己的栈区，寄存器（内核中管理的）
    * 在一个地址空间中多个线程独享：代码段，堆区，全局数据区，打开的文件（文件描述符表）都是线程共享的
* 进程是资源分配的最小单位，线程是操作系统调度执行的最小单位
    * 每个进程对应一个虚拟地址空间，一个进程只能抢一个CPU时间片
    * 一个地址空间可以划分出多个线程，在有效的资源基础上，能够抢更多的CPU时间片

<img src="C:\Users\油腻中年男cjh\Pictures\Saved Pictures\1048430-20170710134655212-558296442.png" alt="线程与进程" style="zoom: 67%;" />

* CPU的调度和切换：线程的上下文切换比进程快地多

    上下文切换：进程/线程分时复用CPU时间片，在切换之前会保存（寄存器）上一个任务的状态，下次切换回这个任务的时候会重新加载这个状态继续运行，**任务从保存到再次加载这个过程就是上下文切换**

* 线程更加廉价，启动速度更快，退出也快，对系统资源的冲击小

**在处理多任务程序的时候使用多线程比使用多进程更要有优势，但是线程不是越多越好**

1. 文件IO操作：文件IO对CPU使用率不高，因此可以分时复用CPU时间片，***线程的个数=2*CPU核心数（效率最高）**
2. 处理复杂的算法（主要是CPU的负担大），***线程的个数=CPU核心数（效率最高）**

## 2.创建线程

### 2.1线程函数

每一个线程都有一个唯一的线程ID，ID类型为`pthread_t`，这个ID是一个无符号长整形数，可以调用如下函数：

``` c
pthread_t pthread_self(void);	// 返回当前线程的线程ID
```

在一个进程中调用线程创建函数，就可得到一个子线程，和进程不同，需要给每一个创建出的线程指定一个处理函数，否则这个线程无法工作

``` c
#include <pthread.h>
int pthread_create(pthread_t *thread, const pthread_attr_t *attr,
                   void *(*start_routine) (void *), void *arg);
// Compile and link with -pthread, 线程库的名字叫pthread, 全名: libpthread.so libptread.a
```

* 参数
    * thread: 传出参数，是无符号长整形数，线程创建成功，会将线程 ID 写入到这个指针指向的内存中
    * attr: 线程的属性，一般情况下使用默认属性即可，写 NULL
    * start_routine: 函数指针，创建出的子线程的处理动作，也就是该函数在子线程中执行
    * arg: 作为实参传递到 start_routine 指针指向的函数内部
* 返回值：线程创建成功返回 0，创建失败返回对应的错误号

### 2.2创建线程



在创建过程中一定要保证编写的线程函数与规定的函数指针类型一致：void *(*start_routine) (void *):

```` c
// pthread_create.c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <string.h>
#include <pthread.h>

// 子线程的处理代码
void* working(void* arg)
{
    printf("我是子线程, 线程ID: %ld\n", pthread_self());
    for(int i=0; i<9; ++i)
    {
        printf("child == i: = %d\n", i);
    }
    return NULL;
}

int main()
{
    // 1. 创建一个子线程
    pthread_t tid;
    pthread_create(&tid, NULL, working, NULL);

    printf("子线程创建成功, 线程ID: %ld\n", tid);
    // 2. 子线程不会执行下边的代码, 主线程执行
    printf("我是主线程, 线程ID: %ld\n", pthread_self());
    for(int i=0; i<3; ++i)
    {
        printf("i = %d\n", i);
    }

    // 休息, 休息一会儿...
    // sleep(1);

    return 0;
}
````

编译测试程序，会看到如下错误信息：

``` shell
$ gcc pthread_create.c
/tmp/cctkubA6.o: In function `main':
pthread_create.c:(.text+0x7f): undefined reference to `pthread_create'
collect2: error: ld returned 1 exit status
```

**错误原因是因为编译器链接不到线程库文件（动态库），需要在编译的时候通过参数指定出来**，动态库名为**libpthread.so** 需要使用的参数为 **-l**，根据规则掐头去尾最终形态应该写成：**-lpthread（参数和参数值中间可以有空格）**。正确的编译命令为：

``` shell
# pthread_create 函数的定义在某一个库中, 编译的时候需要加库名 pthread
$ gcc pthread_create.c -lpthread
$ ./a.out
子线程创建成功, 线程ID: 139712560109312
我是主线程, 线程ID: 139712568477440
i = 0
i = 1
i = 2
```

在打印的日志输出中为什么子线程处理函数没有执行完毕呢（只看到了子线程的部分日志输出）？
主线程一直在运行，执行期间创建出了子线程，说明主线程有 CPU 时间片，在这个时间片内将代码执行完毕了，主线程就退出了。**子线程被创建出来之后需要抢cpu时间片, 抢不到就不能运行，如果主线程退出了, 虚拟地址空间就被释放了, 子线程就一并被销毁了。但是如果某一个子线程退出了, 主线程仍在运行, 虚拟地址空间依旧存在。**

得到的结论：**在没有人为干预的情况下，虚拟地址空间的生命周期和主线程是一样的，与子线程无关。**

目前的解决方案：让子线程执行完毕，主线程再退出，可以在主线程中添加挂起函数 **sleep();**

### 3.线程退出
