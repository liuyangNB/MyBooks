# 探索系统移植世界

## 概述

* 搭建交叉开发环境
* bootloaer的选择和移植
* kernel的配置、编译、移植和调试
* 根文件系统的制作

---

### 方法和思路：

> 先整体后局部：如何编译\=\=》如何添加命令和功能\=\=》如何定义自己的开发板

### 移植的基本步骤：

> * **确定目标机和主机的连接方式**
>
>   > UART串口通信：速率低，实用性强
>   >
>   > USB串口：速度快，驱动要移植修改
>   >
>   > TCP/IP网络通信：速度快，要移植驱动
>   >
>   > DEBUG jtag调试接口：方便快捷，价格高
>
> * **安装交叉编译器**
>
>   > 安装交叉编译器
>   >
>   > - 安装芯片厂商已经编译好的工具链
>   >
>   >   > arm-none-linux-gnueabi-	//arm-linux-
>   >
>   > - 自己手动编译交叉工具链
>   >
>   >   > 要有一定的功底，不建议。可以去看一本书: 《The GNU Toolchain for ARM Target HOWTO》
>
> * **搭建主机-目标数据传输通道**
>
>   > TFTP
>   >
>   > NFS
>
> * **编译三大子系统**
>
>   > bootloader功能子系统
>   >
>   > 内核核心子系统
>   >
>   > 文件系统子系统
>
> * **烧写测试**
>
>   > 烧写

---

---

---

## 交叉编译工具集

### 为什么要有交叉编译

在X86上编译arm能够理解的逻辑；比如0x1010在x86代表add，而arm代表mov，两者的指令逻辑不同，因此需要交叉编译。主机和目标机是不同的平台；相同的平台比如x86体系下的gcc就说普通的编译器。

一些文件，怎么知道是什么平台的？ 使用指令file： file build 可以看到文件的属性。

### 安装交叉编译链

解压到linux目录，并安装。

使用：

> * 使用简单方法
>
>   > 方便，但是如果有多个编译器可能会出问题
>   >
>   > **#arm-linux-gcc** -o build file.c
>
>   > 添加环境变量：
>   >
>   > PATH=\$PATH:/opt/FriendlyARM/toolschain/4.5.1/bin/
>   >
>   > 或者在/etc/environment中用vim添加
>   >
>   > source /etc/environment
>
> * 使用绝对路径方法
>
>   > 麻烦点，但是可靠
>   >
>   > **#/opt/FriendlyARM/toolschain/4.5.1/bin/arm-linux-gcc** -o buile file.c

### 工具集介绍

---

* readelf

  >  elf头信息，（windows是PE头信息）。readelf就说读取这个头信息。
  >
  > readelf -h build

* size

  > 读取可执行程序的大小：代码的、数据段、bss、dec等

* nm

  > 打印符号表信息

* strip

  > 剔除符号表；arm环境需要加上那个前缀。

* strings

  > 查看字符串

* objdump

  > 反汇编；objdump -b build

* objcopy

  > objcopy被用来复制一个目标文件的内容到另一个文件中，可以使用不同于源文件的格式来输出目的文件，即可以进行格式转换。

* addr2line

  > 用的不多。。。

---

---

---

## 环境搭建

主机数据如何传递到开发板：

>  多数是网络接口TFTP。

调试：

> 挂载调试

---

台式机：

> 路由器环境：lan口接路由器；或者直接连PC（不建议，因为这样不好上网了）
>
> 双网卡环境：两个网卡，专门一个网卡和板卡连接

笔记本：

> 虚拟机配置双网卡
>
> > 硬件上添加一个网络适配器，一个NAT（上网），一个桥接（调试）
>
> 串口配置

---

## 系统移植初步

先学会移植配置好的系统，这里是uboot移植。学习下什么是uboot。

### 常用命令（当成shell学习）

> * print	打印uboot的集成环境变量
>
> * setenv savenv    设置环境变量， setenv abc 100 200 ...//abc是变量，后面的都是值 ///setebv abc  什么都不跟就说删除
>
> * 网络环境    setenv ipaddr xxxxx配置板卡的地址，然后板卡去ping主机（主机ping不了板卡，因为uboot精简了echo，没回应）
>
> * nand     nand\[动词]\[内存地址（s还是d看动词）]\[nandflash地址]\[搬移大小]
>
>   > nand中5M地址空间读到0x2100000,读1k：nand read 0x2100000 0x500000 1024
>   >
>   > nand erase 0x500000 1024;
>   >
>   > nand中5M地址空间读到0x2100000,写1k:	nand write 0x2100000 0x500000 1024
>
> * tftp    uboot没有设计tcp协议，而是采用基于udp的文件传输协议---tftp；用tftp验证传输层。///windows选tftpd   linux 32位：sudo apt-get install tftpd-hpa   //64位：sudo apt-get install tftpd openbsc-xinetd
>
> * bootm 启动内核
>
> * go    跳转