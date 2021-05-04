# Shell循环语句-for

* for 循环介绍
* for语法
* 循环控制

---

## 一 for简介

没啥好说

## 二 for语法

### 2.1语法1

```shell
for var in val1 val2 val3...
	do
		commonds
done
```

### 2.2语法2 C式

```shell
for((变量；条件；运算))
	do
		commonds
done
```



---

## 三 循环控制

### 3.1  sleep N 脚本执行到该步休眠N秒

（控制循环节奏，但是感觉这不是解决问题的根本，sleep还是在占用cup吧）



### 3.2 continue

​	跳过

### 3.3 break

​	跳出循环

​		break N 多层次的循环，跳出某层N（细节百度下？）

## job

> 扫描你网段中那些机器是存活的
>
> 手动写一个同步拉脚本
>
> 新建user01-20用户，密码随机6位
>
> 写一个mysql备份脚本
>
> 写一个猜数字脚本
>
> 为DNS写一个自动判断WEB解析的脚本
>
> 写一个99乘法表