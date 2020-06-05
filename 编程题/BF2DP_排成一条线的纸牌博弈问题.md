# BF2DP_排成一条线的纸牌博弈问题

题目描述

> 给定一个整型数组arr，代表数值不同的纸牌排成一条线，玩家A和玩家B依次拿走每张纸牌，
>规定玩家A先拿，玩家B后拿，但是每个玩家每次只能拿走最左和最右的纸牌，玩家A和玩家B绝顶聪明。请返回最后的获胜者的分数。

输入描述:

```
输出包括两行，第一行一个整数n(1 \leq n \leq 5000 )(1≤n≤5000)，
代表数组arr长度，第二行包含n个整数，第i个代表arr[i]( 1 \leq arr[i] \leq 10^5 )(1≤arr[i]≤105)。
```

输出描述:

```
输出一个整数，代表最后获胜者的分数。
```

示例1

输入

```
4
1 2 100 4
```

输出

```
101
```

![img](https://img-blog.csdn.net/20170824141116755?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvZ3JhdmUyMDE1/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)





暴力递归  =》DP

```cpp
#include <memory.h>
#include<iostream>
#include<string>
#include<vector>
using namespace std;
 
int s(vector<int> arr, int i, int j);

 //如果我先拿的情况 
int f(vector<int> arr, int i, int j){
 	//先拿的情况
	 //出口
	 if(i == j){
	 	return arr[i];
	 } 
	 
	 //理解这部分：先手拿了两边其中一个，剩余的那部分arr就是后手拿了 
	 return max(arr[i]+s(arr,i+1,j),arr[j]+s(arr,i,j-1));
 } 
 
 //如果我后拿的情况 
 int s(vector<int> arr, int i, int j){
 	//出口
	 if (i == j){
	 	return 0;//还剩一个，后拿肯定就没了咯 
	 }
	 
	 //理解这部分， 对方拿了一个肯定是会把最差的序列留给下家！！！！ 
	 return min(f(arr,i+1,j),f(arr,i,j-1));//后拿是在先拿的拿完之后的剩余部分先拿 
 }
 
  int Bruforce(vector<int> arr){
 	int length = arr.size();
 	return max(f(arr,0,length),s(arr,0,length));//无非先拿与后拿的情况 
 }
 
////////////////////////////////////////////////////////// 
 
int Bruforce2DP(vector<int> arr) {
   
	int length = arr.size();
	vector<vector<int> > fmap(length, vector<int>(length));
	vector<vector<int> > smap(length, vector<int>(length));
	
	//出口的情况 
	for(int i=0;i<length;i++){
		fmap[i][i] = arr[i];
		smap[i][i] = 0;
	}
	
	//关于ij的边界范围，ij 来自左边下边，左边下边也要在定义域 
	for(int j=0;j<length;j++){
		for(int i=j-1;i>=0;i--){//i=j的话，[i+1][j] =>[j+1][j]超出了 y=x这条边界 
			fmap[i][j] = max(arr[i]+smap[i+1][j],arr[j]+smap[i][j-1]);
			smap[i][j] = min(fmap[i+1][j],fmap[i][j-1]);
		}
	} 
	
	
	return max(fmap[0][length-1],smap[0][length-1]);
	
}

 

 
int main()
{
	
	int n;
	cin>>n;
	vector<int> arr(n,0);
	for(int i=0;i<n;i++){
		int temp;
		cin>>temp;
		arr[i] = temp;
	}
	
	cout<<Bruforce(arr)<<endl;
	cout<<Bruforce2DP(arr);

}

```

