# 大数运算_加减乘除

```cpp
#include <stdio.h>
#include<iostream>
#include <string.h>
#include<stdlib.h>
#include<algorithm>
using namespace std;
//基于C++的大整数运算
//首先，为了方便后面运算，我们先定义一个结构体
struct bign{
	int d[1000];
	int len;
	//下面定义构造函数，用来初始化！ 
	bign(){
		memset(d,0,sizeof(d));
		len=0;
	}
}; 
//一般来说，大整数一般是使用字符串输入的，下面将字符串储存的大整数
//存放在结构体中
bign change(char str[]){
	bign a;
	a.len=strlen(str);
	for(int i=0;i<a.len;i++){
		a.d[i]=str[a.len-i-1]-'0';//这里把大整数的地位切换为高位 
	}
	return a; 
} 

bign change(string str){
	bign a;
	a.len=str.size();
	for(int i=0;i<a.len;i++){
		a.d[i]=str[a.len-i-1]-'0';//这里把大整数的地位切换为高位 
	}
	return a; 
} 
//比较两个大整数的大小
int compare(bign a,bign b){
	if(a.len>b.len)return 1;//a大于b
	else if(a.len<b.len)return -1;//a<b
	else{
		for(int i=a.len-1;i>=0;i++){
			if(a.d[i]>b.d[i])return 1;
			else if(a.d[i]<b.d[i])return -1;
		}
		return 0;//两个数相等 
	} 
}


/***********************************************************/
//下面是大数的四则运算法则
bign add(bign a,bign b){
	bign c;
	int carry=0;//这里的carry表示进位
	for(int i=0;i<a.len||i<b.len;i++){
		int temp=a.d[i]+b.d[i]+carry;
		c.d[c.len++]=temp%10;
		carry=temp/10;
	} 
	if(carry!=0){//如果最后一位的进位不为0，直接付给结果的最高位
	   c.d[c.len++] =carry;
	}
	return c;
} 
//值得注意的是，上面的算法的条件都是非负整数，那么如果有一个数是负数怎么办呢？？？？
//如果有一个数字为负数，我们可以使用高精度减法
bign sub(bign a,bign b){
	bign c;
	for(int i=0;i<a.len||i<b.len;i++){
		if(a.d[i]<b.d[i]) {//如果不够减法，则向高位借位 
			if(a.d[i+1]>0){
				a.d[i+1]--;
				a.d[i]+=10; 
			}
		}
		c.d[c.len++]=a.d[i]-b.d[i];
	}
	while(c.len-1>=1&&c.d[c.len-1]==0){
		c.len--;
	}//去除高位为0的！同时保留一个最低位 
	return c;
} 
//高精度的乘法运算规则
bign multi(bign a,int b){
	bign c;
	int carry=0;
	for(int i=0;i<a.len;i++){
		int temp=a.d[i]*b+carry;
		c.d[c.len++]=temp%10;
		carry=temp/10;
	}
	while(carry!=0)
	{
		c.d[c.len++]=carry%10;
		carry/=10;
	}
	return c;
} 
//高精度与低精度的除法
bign divide(bign a,int b,int &r){//r为余数，这里表示为引用
    bign c;
	c.len=a.len;
	for(int i=a.len-1;i>=0;i--){
		r=r*10+a.d[i];
		if(r<b) c.d[i]=0;
		else{
			c.d[i]=r/b;
			r=r%b;
		}
	} 
	while(c.len-1>=1&&c.d[c.len-1]==0){
		c.len--;
	}
	return c;
} 
void print(bign a){
	for(int i=a.len-1;i>=0;i--)
	{
		printf("%d",a.d[i]);
	}
}
int main(){
	string str1 = "123456";
	string str2 = "654321";
	char* stra = (char *)str1.c_str();
	char* strb =(char*) str2.c_str();
	bign a=change(stra);//bign a=change(str1)也行
	bign b=change(strb);//bign b=change(str2)也行
	print(add(a,b)); 
	return 0;
}

```

