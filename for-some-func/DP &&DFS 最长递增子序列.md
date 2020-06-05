# DFS|DP最长递增子序列（不是子串）

题目描述

> 给定数组 arr=【2，1，5，3，6，4，8，9，7】，返回最长递增子序列 【1，3，4，8，9】



思路一：纯暴力

> 两层for遍历所有的子序列，维护看看哪个子序列最长，但是其实这种方法适合求最长子串的情况，因为字串是连续是，这时候ij两个变量就可以遍历。这种遍历所有可能的子序列可能要想到DFS？
>
> 但是吧，DFS时间复杂度似乎巨大？==**DFS只是选点遍历，不行就折返**==

## 一、求DP的思路

### **思路一：还是用DFS试试看**

和滑雪那题类比下，也是选点，然后满足的条件是递增

> int dfs(step)
>
> ​	如果这个点访问过就返回，返回void还是值得看要不要dfs返回值了
>
> ​	dp[step] =1 ;初始长度
>
> ​	从step起，往后一步步选择点
>
> ​		不满足的点跳过
>
> ​		满足的点用来更新dp

这个解法目前只是求DP，而没有优化为直接求出最长子序列；这个求出的dp[i]=m含义是，以arr[i]开头的最长子序列的长度为m。DFS的思路是从step开始，next一步步试探，撞墙或者不满足条件就下一步next，满足就DFS再按要求维护dp

```cpp
#include"stdafx.h"
#include<vector>
#include<iostream>
#define max(a,b) ((a)>(b))?(a):(b)
#define min (a,b) ((a)<(b))?(a):(b)
using  namespace std;
int arr[] = { 3, 18, 7, 14, 10, 12, 23, 41, 16, 24 };
int dp[10] = { 0 };
int len = sizeof(arr) / sizeof(arr[0]);

//从step开始的点到结束，最长的递增字串的长度
int  DFS(int step) {
	//意思是从step开始选路,如果这个地方访问过就折返
	if (dp[step]) {
		return dp[step];
	}
	dp[step] = 1;//第一次访问，最长序列从1开始

	//从step开始，往后遍历，找到最长的子序列
	for (int i = 1; i < len ; i++) {//脚步要从1开始
		int nextstep = step + i;
		//先看看是不是南墙
		if (nextstep < 0 || nextstep >= len || arr[nextstep] < arr[step]) continue;
		else {
			//记得先去dfs遍历
		DFS(nextstep);
		dp[step] = max(dp[step], dp[nextstep] + 1);
		}
		
	}
	return dp[step];
}

int main() {
    
	int maxnum=0;
	for (int i = 0; i < len; i++) {
		dp[i] = DFS(i);
		maxnum = max(maxnum, dp[i]);
	}
	cout << maxnum;
}
```



### **思路二 用DP遍历**

思路：

> dp[i]=m表示以arr[i]结尾的最长子序列长度为m
>
> 那么dp[i]是之前的dp中arr比自己小的中最大的那个dp+1咯
>
> **减治：**
>
> dp[i]=m;如何去求呢？先分析含义：以arr[i]结尾的最长子序列长度为m，那么上一个状态是什么呢？
>
> 最长的前面也就是次长，所以，在0....j....i-1中找次长的dp咯，条件就是arr[j]<arr[i]; 换句话说就是 0-i-1中满足基本条件arr[j]<arr[i]的选项，选择最大的dp[j]; 那么dp[i] = dp[j]+1;
>
> 为什么可以这样在i前面选择呢，因为含义里是以arr[i]结束的字串，所以i后面的是根据前面的来确定，也就是dp填表是从0开始的原因。

```cpp

 vector<int> GetDP(vector<int> arr) {
	
	 int N = arr.size();
	 int *dp = new(int [N]);
	 
	 for (int i = 0; i < N; i++) {
		 dp[i] = 1;//默认的长度都是1
		 for (int j = 0; j < i; j++) {
			 if (arr[j] < arr[i]) {//数据比之前的大才能更新dp
				 dp[i] = max(dp[i], dp[j] + 1);
			 }
		 }
	 }
	 vector<int> ans(dp,dp+N);
	 return ans;
}
```



---

---

## **二 根据dp表求出最长子序列**

### 思路一： DFS初始一波

在这里的思路是：从最大dp出开始，一步步探测合适的next；其实这部分可以用for来处理

```cpp
int arr[] = { 1,2,8,3,4,9,5,6,7 };
int visited[9] = { 0 };
int len = sizeof(arr) / sizeof(arr[0]);
vector<int> DFS_CHOOSE(int step) {
	vector<int> ans;
	if (visited[step]) return ans;
	visited[step] = 1;

	//出口条件，由于最后一位的选择条件不是说选最小的就好，因此可以放着先不处理，之后再处理
	if (dp[step] == 1) {
		return ans;
	}

	int next = step + 1;
	int min_t = arr[step];
	//选取的条件很重要,相同的dp遍历选出最小的arr
	while (dp[next] == dp[step] && next < len ) {
		if (min_t > arr[next]) min_t = arr[next];
		next++;
	}
	//选择和一个ap-1的位置，因为中间可能有不合适的位置比如 1，2，3，9，4；9的dp=1
	while (dp[next] != dp[step] - 1 && next < len) {
		next++;
	}
	step = next;
	
	 ans = DFS_CHOOSE(step);
	 ans.insert(ans.begin(),min_t);//这时候step已经到dp[step]==1的第一个位置，就需要在dp[]==1的地方找到最小的合适的数了

	return ans;
}
```





### **思路二：以dp方法来求解的**

辅助问题：

> dp[i] = k;  以i号位置结尾的情况下，arr【0-i】中最大递增子序列长度。
> 决策条件：
> 				i之前的dp[j]中, 取最大的dp[j]加上1
> 				for(j=0-i) if( arr[j]<arr[i]) 那么 dp[i] = max(dp[i],dp[j]+1); 

主要思路：

> 利用每个位置的dp值，也就是当前i位置的最长子序列长度值，然后向前找第一个比它小的dp值，这个位置对应的元素数据也要比i 的数据小，
> 		那么这个位置就是i前一个合适的位置。

其它思路：

> 先sort一个B；
> B与原来的A用最长公共子序列。

```cpp
#include "stdafx.h"
#include<iostream>
#include<stdio.h>
#include <math.h>
#include <vector>
#include <algorithm>

using namespace std;


 vector<int> GetDP(vector<int> arr) {
	
	 int N = arr.size();
	// cout << N << endl;
	 int *dp = new(int [N]);
	 
	 for (int i = 0; i < N; i++) {
		 dp[i] = 1;//默认的长度都是1
		 for (int j = 0; j < i; j++) {
			 if (arr[j] < arr[i]) {//数据比之前的大才能更新dp
				 dp[i] = max(dp[i], dp[j] + 1);
			 }
		 }
	 }
	 vector<int> ans(dp,dp+N);
	 return ans;
}


int main()
{
	/*****读取数据****************/
	freopen("DATA.txt", "r", stdin);
	vector<int> input = { 2,1,5,3,6,4,8,9,7};
	vector<int> dp = GetDP(input);
	vector<int> ans;

	/*****先找到最大的地方******/
	int max = 0;
	int max_pos = 0;
	for (int i = 0; i < input.size(); i++) {
		if (input[i] > max) {
			max_pos = i;
			max = input[i];
		}
	}

	/*****根据dp的隐藏信息去找出序列****/
	int now = max_pos;
	ans.push_back(input[now]);
	for (int i = max_pos-1; i >= 0; i--) {
		if (dp[i] == dp[now] - 1&&input[i]<input[now]) {//如果dp相差1，且值按序就是所选数
			ans.push_back(input[i]);
			now = i;
		}
	
	}

	/******打印数据****************/
	for (int i = ans.size()-1; i >= 0; i--) {
		cout << ans[i] << " ";
	}
	cout << endl;


	return 0;
}
```





