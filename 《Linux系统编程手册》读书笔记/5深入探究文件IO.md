概述：

这一章主要讲文件，fd，inode，open，fcntl，文件属性等

* 引入原子操作概念 --一次性加以执行
* 多用途fcntl() --读取和设置文件状态标志
* 文件描述符的数据结构
* 支持拓展读写的系统调用
* 临时文件相关

---

# 5.1 原子操作和竞争条件

竞争状态是啥：操作共享资源的两个进程或线程，其结果取决于一个无法预期的顺序。也就是这些进程获取CPU使用权的相互顺序



**以独占方式创建一个文件：**

已知：同时指定 O_EXCL | O_CREAT ，如果文件已经存在就会返回一个错误，对文件的检查和创建属于一个原子操作。

看看下面没有O_EXCL 的错误代码示例：

```c
int main(int argc, char *argv[])
{
    fd = open( argv[1], O_WRONLY );
    if( fd < 0 ){
        printf( "pid=%d, 结束睡眠\n", getpid() );
        if( errno != ENOENT ) {	//其他错误原因，导致文件打开失败
            perror( "open" );
            exit( -1 );
        }else {
            //文件不存在　导致文件打开失败
            fd = open( argv[1], O_WRONLY | O_CREAT, S_IRUSR | S_IWUSR );    
            if( fd < 0 ) {
                printf( "文件%s创建失败\n", argv[1] );
                exit( -1 );
            }
            printf( "进程id=%d\n", getpid() );
            close( fd );
        }
    }else {
        printf( "文件%s,打开成功:fd=%d\n", argv[1], fd );
        printf( "进程id=%d\n", getpid() );
        close( fd );
    }
    return 0;
}
```

以上代码本意：先RW模式打开文件，如果打开成功就ok，如果打开失败，就创建并打开。本身没啥问题；但是考虑下，两次open之间如果有其它进程创建了文件咋么办？为了示例明显，可以在两个open加上sleep加以分析。

这时候，第二次open，就会认为是你这个进程自己打开的文件，也就是会open成功；但是实际上，你希望是不成功的！！！

虽然这样概率很低，但是也是存在问题的，这种open依赖于进程的调用顺序，是不可靠的。

当第一个进程在 **检查文件是否存在** 和**创建文件** 之间发生中断，这就会导致问题，解决方法是：一次性调用open，加上O_EXCL 属性。

```c
fd = open( argv[1], O_WRONLY | O_CREAT | O_EXCL, S_IRUSR | S_IWUSR );
```



# 5.2 文件控制操作 : fcntl()

* 就是调用一个打开的文件描述符，然后执行一系列控制操作

```c
#include <fcntl>
int fcntl(int fd, int cmd, ...);
```

后面细讲



# 5.3 打开文件的状态标志

* fcntl 用途之一：针对一打开文件，获取或修改其 **访问模式** 和 **状态标志**

```c
int flags, accessMode;
flags = fcnt(fs, F_GETFL);	//第三参数不是必须
if (flags == -1) 
    errExit("fcntl");
//以下代码可用于判断文件是否以同步方式打开
if(flags & O_SYNC)
    printf("writes are synchronized");
```
* 获取文件的 **读写模式**

```c
//判断文件的访问模式有点复杂，需要通过O_ACCESSMODE
accessMode = flags & O_ACCMODE
if (accessMode == O_WRONLY || accessMode == O_RDWR)
    printf("file is writeable");
```

* 通过fcntl和**F_SETFL**来修改打开文件的某些状态标志

```c
//允许修改是模式有：O_APEND O_NONBLOCK O_NOATIME O_ASYNC O_DIRECT
//(其他UNIX运行fcntl修改其他标志如：O_SYNC)
int flages;
flages = fcntl(fd, F_GETFL);
if (flags == -1) 
    errExit("fcntl");
flages |= O_APPEND;
if (fcntl(fd, F_SETFL, flages) == -1)
    errExit("fcntl");
```

* fcntl的适用场景

> 文件不是调用者打开，所以不能open()时候设置；如文件是那3个标准输入输出描述符

> 文件描述符是通过open()之外的系统调用获取的，如pipe() socket()