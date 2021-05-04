# GDB的使用

## debug模式编译

gcc **-g** -o file file.c

gdb file

## 打断点

* 看代码

  > list（l）查看源代码
  >
  > 

* 打断点

  > b(reak) 函数名
  >
  > b(reak) 行号
  >
  > b(reak) 文件名：行号
  >
  > b(reak) 行号 if 条件

* 查看断点

  > info break (i b)

* 删除断点

  > delete 断点号（d）

## 运行调试

* run	(r)
* continue   (c)
* quit    (q)



## 单步调试

step:			step into		进入

next:			  step over	下一句

finish:		  step over		完成这个函数



## 继续运行

## 打印和监控

p(rint)	x

w(atch)	x

---

## 技巧

wi

---

多文件的gbd调试就比较复杂点