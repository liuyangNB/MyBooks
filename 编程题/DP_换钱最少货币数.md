# 换钱最少货币数

题目：

> 给一组数组arr[5,2,3]元素代表货币面额，给一个数字aim=20 代表面额；求找开aim需要最少的货币数

贪心：从大面额依次进行取余，这种思路不适合所有情况

DP：

> arr[N]={2,3,5}	==>i:这个变量肯定要的，而且肯定是依次遍历
>
> target=aim		==>j:这个应变量，因为dp是从其他
>
> dp\[i][j]=只用arr[0...i]的货币，构成target=j的最小数量
>
> > 如何分类来处理？用arr[0..i]的钱来凑成j无非以下分类（逮着i）；以下是分类，这么多情况的最小值才是dp\[i][j]的应有值。
> >
> > 用0张arr[i]的情况 = dp\[i-1][j];
> >
> > 用1张arr[i]的情况 = dp\[i-1][j-arr[i]]+1;
> >
> > 用2张arr[i]的情况 = dp\[i-1][j-2*arr[i]]+2;
> >
> > ...==只用k个arr[i],剩下的target=aim-k*arr[i];然后剩下的target要用arr[0...i-1]来凑==
> >
> > 用k张arr[i]的情况 = dp\[i-1][j-k*arr[i]]+k;
>
> 以上是暴力罗列所有情况，其实从公式上可以推导出min(all)的情况是：
>
> dp\[i][j] = min{dp\[i-1][j-k*arr[i]]+k(k>=0)}=...=min{dp\[i-1][j],dp\[i][j-arr[i]]+1}
>
> > 以上公式可以从数学上得出，但是物理上理解应该如下：
> >
> > dp\[i][j]是用arr\[0-i][target=j];所有情况可以分为：
> >
> > ==1、不用arr[i] 凑成j==
> >
> > ==2、至少用1张arr[i]来凑成j-arr[i];==
> >
> > > 以上情况1好理解，2的情况是至少用1张arr[i],反正我确定用了1张arr[i],这时候target = j-arr[i];但是要构成这个target，可以依然从arr[0...i]中来取钱。
>
> 

边界条件分析：

> dp\[0][j]: 只有arr[0]来凑，这时候如果aim可以整除arr[0]就能凑成，否则凑不成，返回应该是MAX_INT，因为要求的是最少。
>
> dp\[i][0]:无论是什么货币凑成0元，最少的货币数也是0

```cpp
//数组的货币无限量提供
int fun(int arr[],int wd, int aim) {
	//C++申明一个二维数组
	int row = wd;//
	int col = aim+1;
	int** dp = new int*[row];//申请aim个数组的数组
	for (int i = 0; i < row; i++) {
		dp[i] = new  int[col];
	}
	//手动填0
	for (int i = 0; i < row; i++) {
		for (int j = 0; j < col; j++) {
			dp[i][j] = 0;
		}
	}


	//填边界dp[i][0] aim=0的时候，返回的张数应该是0；
	for (int i = 0; i < row; i++) {
		dp[i][0] = 0;
	}
	//填边界dp[0][i] 只有arr[0]这种面额的时候（注意不是没有面额），
	for (int i = 0; i < col; i++) {
		dp[0][i] = MAX;//代表没有方法
		if (i%arr[0] == 0) dp[0][i] = i / arr[0];
	}

	//递推式
	for (int i =1; i < row; i++) {
		for (int j = 1; j < col; j++) {
			int left = MAX;
			if (j - arr[i] >= 0 && dp[i][j - arr[i]] != MAX)	left = dp[i][j - arr[i]] + 1;
			dp[i][j] = min(left, dp[i - 1][j]);
		}
	}
    return dp[row - 1][aim];
}
	
```

```cpp
//数组的货币只提供一张==》取与不取
int fun1(int arr[], int wd, int aim) {
	//创建dp[][]且设置0；其实可以用vector
	int row = wd;
	int col = aim + 1;
	int**dp = new int*[row];
	for (int i = 0; i < row; i++) {
		dp[i] = new int[col];
	}
	for (int i = 0; i < row; i++) {
		for (int j = 0; j < col; j++) {
			dp[i][j] = 0;
		}
	}

	//设置边界 i=0;//只有arr[0]有的选，aim=j;
	for (int j = 0; j < col; j++) {
		dp[0][j] = MAX;
		if (j == arr[0]) dp[0][arr[0]] = 1;///////这里j变化说明aim在变，而现在只有一张arr[0]的面额，也就是ap[0][arr[0]]=1
	}
	//边界情况 j=0;
	for (int i = 0; i < row; i++) {
		dp[i][0] = 0;//aim=0,也就是没有换钱的必要了
	}
	//一般情况
	for (int i = 1; i < row; i++) {
		for (int j = 1; j < col; j++) {
			int temp = MAX;
			if (j-arr[i]>=0&&dp[i-1][j-arr[i]]!=MAX) 
                temp = dp[i - 1][j - arr[i]] + 1;

			dp[i][j] = min(dp[i - 1][j], temp);
			
		}
	}

	return dp[row - 1][aim]==MAX?-1: dp[row - 1][aim];
}

```

