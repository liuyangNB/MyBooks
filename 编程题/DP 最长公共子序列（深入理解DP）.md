# DP 最长公共子序列（深入理解DP）

**一，问题描述**

给定两个字符串，求解这两个字符串的最长公共子序列（Longest Common Sequence）。比如字符串1：BDCABA；字符串2：ABCBDAB

则这两个字符串的最长公共子序列长度为4，最长公共子序列是：BCBA

 

**二，算法求解**

这是一个动态规划的题目。对于可用动态规划求解的问题，一般有两个特征：①最优子结构；②重叠子问题

**①最优子结构**

设 X=(x1,x2,.....xn) 和 Y={y1,y2,.....ym} 是两个序列，将 X 和 Y 的最长公共子序列记为LCS(X,Y)

找出LCS(X,Y)就是一个**最优化问题**。因为，我们需要找到X 和 Y中**最长的**那个公共子序列。而要找X 和 Y的LCS，首先考虑X的最后一个元素和Y的最后一个元素。

**1）如果 xn=ym**，即X的最后一个元素与Y的最后一个元素相同，这说明该元素一定位于公共子序列中。因此，现在只需要找：LCS(Xn-1，Ym-1)

LCS(Xn-1，Ym-1)就是原问题的**一个**子问题。为什么叫子问题？因为它的规模比原问题小。（小一个元素也是小嘛....）

为什么是最优的子问题？因为我们要找的是Xn-1 和 Ym-1 的最长公共子序列啊。。。最长的！！！换句话说，就是最优的那个。（这里的最优就是最长的意思）

**2）如果xn != ym**，这下要麻烦一点，因为它产生了**两个**子问题：LCS(Xn-1，Ym) 和 LCS(Xn，Ym-1)

因为序列X 和 序列Y 的最后一个元素不相等嘛，那说明最后一个元素不可能是最长**公共**子序列中的元素嘛。（都不相等了，怎么公共嘛）。

LCS(Xn-1，Ym)表示：最长公共序列可以在(x1,x2,....x(n-1)) 和 (y1,y2,...yn)中找。

LCS(Xn，Ym-1)表示：最长公共序列可以在(x1,x2,....xn) 和 (y1,y2,...y(n-1))中找。

求解上面两个子问题，得到的公共子序列谁最长，那谁就是 LCS（X,Y）。用数学表示就是：

LCS=max{LCS(Xn-1，Ym)，LCS(Xn，Ym-1)}

由于条件 1)  和  2)  考虑到了所有可能的情况。因此，**我们成功地把原问题 转化 成了 三个规模更小的子问题。**

 

**②重叠子问题**

重叠子问题是啥？就是说原问题 转化 成子问题后，  子问题中有相同的问题。咦？我怎么没有发现上面的三个子问题中有相同的啊？？？？

OK，来看看，原问题是：LCS(X,Y)。子问题有 ❶LCS(Xn-1，Ym-1)    ❷LCS(Xn-1，Ym)    ❸LCS(Xn，Ym-1)

初一看，这三个子问题是不重叠的。可本质上它们是重叠的，因为它们只重叠了一大部分。举例：

第二个子问题：LCS(Xn-1，Ym) 就包含了：问题❶LCS(Xn-1，Ym-1)，为什么？

因为，当Xn-1 和 Ym 的最后一个元素不相同时，我们又需要将LCS(Xn-1，Ym)进行分解：分解成：LCS(Xn-1，Ym-1) 和 LCS(Xn-2，Ym)

也就是说：在子问题的**继续**分解中，有些问题是重叠的。

 

由于像LCS这样的问题，它具有重叠子问题的性质，因此：用递归来求解就太不划算了。因为采用递归，它重复地求解了子问题啊。而且注意哦，所有子问题加起来的个数 可是指数级的哦。。。。

[这篇文章](http://blog.csdn.net/trochiluses/article/details/37966729)中就演示了一个递归求解重叠子问题的示例。

那么问题来了，你说用递归求解，有指数级个子问题，故时间复杂度是指数级。这指数级个子问题，难道用了动态规划，就变成多项式时间了？？

呵呵哒。。。。

关键是采用动态规划时，并不需要去一 一 计算那些重叠了的子问题。或者说：用了动态规划之后，有些子问题 是通过 “查表“ 直接得到的，而不是重新又计算一遍得到的。废话少说：举个例子吧！比如求Fib数列。关于Fib数列，[可参考：](http://www.cnblogs.com/hapjin/p/5571352.html)

[![img](https://images2015.cnblogs.com/blog/715283/201606/715283-20160611203653590-28450133.png)](https://images2015.cnblogs.com/blog/715283/201606/715283-20160611203653590-28450133.png)



求fib(5)，分解成了两个子问题：fib(4) 和 fib(3)，求解fib(4) 和 fib(3)时，又分解了一系列的小问题....

从图中可以看出：根的左右子树：fib(4) 和 fib(3)下，是有很多重叠的！！！比如，对于 fib(2)，它就一共出现了三次。**如果用递归来求解，fib(2)就会被计算三次，而用DP(Dynamic Programming)动态规划，则fib(2)只会计算一次，其他两次则是通过”查表“直接求得。而且，更关键的是：查找求得该问题的解之后，就不需要再继续去分解该问题了。而对于递归，是不断地将问题分解，直到分解为 基准问题(fib(1) 或者 fib(0))**

 

说了这么多，还是要写下最长公共子序列的递归式才完整。借用网友的一张图吧：）

 

[![img](https://pic002.cnblogs.com/images/2012/214741/2012111100085930.png)](https://pic002.cnblogs.com/images/2012/214741/2012111100085930.png)

**c[i,j]表示：(x1,x2....xi) 和 (y1,y2...yj) 的最长公共子序列的长度。**（是长度哦，就是一个整数嘛）。公式的具体解释可参考《算法导论》动态规划章节

 

 

这张DP表很是重要，从中我们可以窥见最长公共子序列的来源，同时可以根据这张表打印出最长公共子序列的构成路径

[![img](https://images2018.cnblogs.com/blog/1358881/201807/1358881-20180724195351829-1792192564.png)](https://images2018.cnblogs.com/blog/1358881/201807/1358881-20180724195351829-1792192564.png)



```cpp
//通过牛客网
#include<iostream>
#include<stdio.h>
#include <math.h>
#include <vector>
#include <algorithm>
#include <string>
using namespace std;

/***************************************************************************/
vector<vector<int> > GetDP(string str1, string str2) {
	int row = str1.length();
	int col = str2.length();
	vector<vector<int> > dp(row, vector<int>(col));///////////

	dp[0][0] = str1[0] == str2[0] ? 1 : 0;
	for (int i = 1; i < row; i++) {
		dp[i][0] = max(dp[i-1][0],(str1[i] == str2[0] ? 1 : 0));
	}
	for (int j = 1; j < col; j++) {
		dp[0][j] = max(dp[0][j-1], (str1[0] == str2[j] ? 1 : 0));
	}
	for (int i = 1; i < row; i++) {
		for (int j = 1; j < col; j++) {
			if (str1[i] == str2[j]) dp[i][j] =max(dp[i][j],dp[i - 1][j - 1] + 1);
			else dp[i][j] = max(dp[i-1][j],dp[i][j-1]);
		}
	}
	return dp;
}
/***************************************************************************/
int main()
{
	/*****读取数据****************/
	string str1;// = "1A2C3D4B56";
	string str2;// = "B1D23CA45B6A";
    
    cin>>str1>>str2;
	string ans;
	vector<vector<int> > dp = GetDP(str1, str2);

	/******逆向根据dp寻找路径（也可以用DFS递归枝剪法）*******/
	int row = str1.length();
	int col = str2.length();
	int max = 0; int pos = 0;
	int len = dp[row - 1][col - 1];
	int i = row-1;
	int j = col - 1;
	while(ans.length()<len){
			//从尾部逆向找方向
			if (i>0 && dp[i][j] == dp[i-1][j]){
				//说明来自左方
				i--;
			}
			else if (j>0 && dp[i][j] == dp[i ][j-1]) {
				//说明来自上方
				j--;
			}
			else{// if (i>0 && j>0 && dp[i][j] == dp[i - 1][j - 1]+1) { 后面这种情况就死循环，打印不了最后那个
				//说明这个str1[i]和str2[j]相等
				char temp = str1[i];
				ans += temp;
				i--;
				j--;
			}

	}//cout << endl;
	
	reverse(ans.begin(), ans.end());
	cout << ans;
	return 0;
}


```

