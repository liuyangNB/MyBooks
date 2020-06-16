# Shell流程控制语句-if

* shell中的五大运算
* if语句

---

## 一 shell中的运算

### 1.1 数学比较运算

> 运算符解释（整型）
>
> -eq	等于
>
> -gt	大于
>
> -lt	小于
>
> -ge	大于等于
>
> -le	小于等于
>
> -ne	不等于

### 1.2 字符串比较运行

> 运算符解释、注意字符串一定不要忘记用引号引起来
>
> ==	相等		test \$USER == 'root';echo \$?
>
> !=	不相等
>
> -n 	检测字符串长度是否大于0
>
> -z	检查字符串长度是否为0

### 1.3 文件比较与检查

> #存在性
>
> -d	检查文件是否存在且为目录
>
> -e	检查文件是否存在(文件或目录)
>
> -f	检查文件是否存在且为文件
>
> #权限
>
> -r	检查文件是否可读
>
> -w	检查文件是否可写
>
> -x	检查文件是否可执行
>
> -s	检查文件是否存在且不为空（有空格也不算空）
>
> -O	检查文件是否存在且为当前用户拥有 Own
>
> -G	检查文件是否存在且为当前用户组	Group
>
> file1 -nt file2	检查文件1是否比文件2新
>
> file1 -ot file2	检查文件1是否比文件2旧
>
> （echo $? 	返回0为真）
>
> ######man test可以看更全的

### 1.4 逻辑运算

> &&
>
> ||
>
> ！

---

## 二 if语法

### 2.1 单if

```shell
if [condition]
	then
		commands
fi
```

```shell
#例子，假如在/tmp/abc这个文件夹 无则创建
if [! -d /temp/abc]
	then
		mkdir -v /tmp/abc
		echo "finished"
fi
```

### 2.2 多部逻辑

```shell
if [condition]
	then
		commonds
else
		commends
fi		
```

```shell
if []
	then
		commonds
elif
	then 
		commonds
...
else
		commonds
fi
```

## 补充：

(())	可以做数学表达式

[[]]	可以做字符匹配



## 作业

判读一个机器是否能ping通

判断服务器上某服务是否开启

判断某年是否为闰年

写一个Nginx安装脚本，加入判断，当上一步成功后执行下一步

写一个备份脚本，将A机器当天修改后的文件收集到/cash目录，打包并发送到B机器的/opt/backup目录下，通过MD5值判断B机器上的备份的完整性