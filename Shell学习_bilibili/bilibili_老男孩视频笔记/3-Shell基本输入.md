# Shell基本输入

* read命令

---

默认接收键盘的输入，回车代表输入结束



命令选项：

-p	打印信息

-t	限定时间

-s	不回显

-n	输入字符个数

```shell
#!/bin/bash
clear

#echo -n -e "Login:"
#read acc			#相当于一个变量
read -p "Login:" acc

echo -n -e "Passwd:"
read -s -t5 -n6 pw		#不回显，超时退出,只能识别6位数

echo "account: $acc		passwd:$pw"
```

