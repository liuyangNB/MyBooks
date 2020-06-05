# 最小编辑代价

题目描述

> 给定两个字符串 str1 str2；再给定三个整数 ic、dc、rc分别代表 插入、删除、替换 一个字符的代价，返回编辑str1成str2的最小代价。

套路：

> * 确定是dp后先分析给定的量：str1、str2、cost[3]（两变，一表）
>
> * 然后分析dp\[i][j]=m的映射：
>
>   > i:	str1的收敛
>   >
>   > j:	str2的收敛
>   >
>   > m: 	应变量求什么就是什么
>
> * dp的思路就是减治，把问题往前推一步的所有情况列出了

本题：

> dp\[i][j] 就是str1[0...i]	\==> str2[0...j]
>
> > 那么**其来源可能是**：
> >
> > * 先删一个	再str1[0...i-1] \==> str2[0...j]
> >
> > * 先str1[0...i]\==>str2[0...j-1]     再插入一个
> >
> > * 关于替换如果str1[i]!=str2[j] 那么必须先
> >
> >   str1[0...i-1]\==>str2[0...j]	再把str1[i]替换掉
> >
> > * 如果str1[i]==str2[j],那么这时候的消费就等于
> >
> >   str1[0...i-1]\==>str2[0...j]
> >
> >   选其小者即可



```cpp
//这个算法牛客网只能过85%，因为空间复杂度是n*m;时间复杂度倒是满足O(n*m)
int GetDP(string str1, string str2, int ic, int rc, int dc) {
    /*创建二维数组 dp[row][col]*/
	int row = str1.length();
	int col = str2.length();
	vector<vector<int> > dp(row, vector<int>(col));

	
    /*平凡情况*/
    //dp[0][0]的情况
	if (str1[0] == str2[0]) dp[0][0] = 0;
	else dp[0][0] = min(rc, dc + ic);
	//求dp[i][0]
	for (int i = 1; i < row; i++) {
		if (str1[i] != str2[0]) {
            //abc=>d; 要不先dc再加上之前的变化，要不删的还剩1个再rc
			dp[i][0] = min(dc+dp[i-1][0],dc*(i)+rc);
		}
		else {
            //最后那么相同，那么删除之前的即可
			dp[i][0] = dc * (i - 1);
		}
	}
	//dp[0][j]
	for (int j = 1; j < col; j++) {
		if (str2[j] != str1[0]) {
            //a=>bcd;要不先删a，再一直ic；要不先变成bc，再ic
			dp[0][j] = min(dc+ic*(j+1),dp[0][j-1] + ic);
		}
		else {
			dp[0][j] = ic * j;//直接把前面的ic就好了
		}
	}
	//一般情况
	for (int i = 1; i < row; i++) {
		for (int j = 1; j < col; j++) {
			dp[i][j] = min(dc + dp[i - 1][j], dp[i][j - 1] + ic);
			if (str1[i] == str2[j]) dp[i][j] = min(dp[i][j],dp[i - 1][j - 1]);
			else dp[i][j] =min(dp[i][j],dp[i - 1][j - 1] + rc);

		}
	}
	return dp[row-1][col-1];
}
```

要通过全部用例就要学会压缩空间，具体再学吧