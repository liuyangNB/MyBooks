# make的作用

* 组织工程，编译复杂程序
* 安装卸载程序
* 脚本

---

先从例子开始

```c
//func1.c
#include<stdio.h>
int fun1(int x, int y){
    return x+y;
}

//fun2.c
#include<stdio.h>
int fun2(int x, int y){
    return x*y;
}

//main.c
#include<stdio.h>
#include<stdlib.h>
int main(){
    int x,y;
    scanf("%u %u"&x,&y);
    printf("%u\n",fun1(x,y));
    printf("%u\n",fun2(x,y));
    return 0;
}
```

```shell
#正常情况下我们这样做：
gcc -o main main.c fun1.c fun2.c
```

---

```makefile
#creat first rule
hello:main.o fun1.o fun2.o
	gcc main.o fun1.o fun2.o -o main
main.o:main.c
	gcc -c main.c
fun1.o:fun1.c
	gcc -c fun1.c
fun2.o: fun2.c
	gcc -c fun2.c

clean:
	rm fun1.o fun2.o main.o hello
```

---

变量：

> CFLAG = XXX
>
> $(CFLAG)
>
> 就像宏展开，好处就是修改维护方便

---

变量、make流程、规则、伪目标、引用其他makefile嵌套、条件判断、使用函数、makefile管理命令

---

# Autotools

一些大型的商业软件，makefile太多，不太可能自己写吧，特别是一堆三方库等。