# DP 最小路径和

**DP思想适合于求什么什么最小或者最大值之类的，然后就是要找到减治的状态转移方程**

**还有一个明显的特征是：现在状态是之前状态的情况转移，目前状态只受前面状态影响！！！**

**题目**

> 给定一个矩阵m,从左上角走到右下角，只能向下或者向右走，路径上的位置有权值，求路径累加的权值最小的值。

**分析**：

> **当前状态有哪些之前的状态转移过来：要不来自左边，要不来自上边**
>
> 自变量两个 i，j 应变量一个K；
> dp\[i][j]=k		到达位置[i ,j]的权值是k；
> 	
> 则dp\[i][j] = dp\[i-1][j]或者dp\[i][j-1]  + m\[i][j];我们需要小的，那么就
> dp\[i][j] =min{ dp\[i-1][j] , dp\[i][j-1]}  + m\[i][j];
> 	
> 边界分析：
> i=0时候，dp\[0][j] 只能选择左边来的情况，因此 dp\[0][j] = dp\[0][j-1]+m\[i][j];
> j=0时候，dp\[i][0]=dp\[i-1][0]+m\[i][j];			



```cpp
#include<iostream>
#include<stdio.h>
#include <math.h>
#include <algorithm>
#include<vector>

using namespace std;
vector<vector<int> > dp;
vector<vector<int> > arr;


void GETDP() {
	int row = arr.size();
	int col = arr[0].size();
    dp[0][0] = arr[0][0];
	for (int i = 1; i < row; i++) {
		dp[i][0] = dp[i - 1][0] + arr[i][0];
	}
	for (int j = 1; j < col; j++) {
		dp[0][j] = dp[0][j - 1] + arr[0][j];
	}

	for (int i = 1; i < row; i++) {
		for (int j = 1; j < col; j++) {
			dp[i][j] = min(dp[i - 1][j], dp[i][j - 1]) + arr[i][j];
		}
	}
}

int main()
{	
	/**初始化数据arr dp **/
	int row ;
	int col ;
	cin>>row>>col;
	for(int i=0;i<row;i++){
        vector<int> temp_vec;
        vector<int> temp_dp;
        for(int j=0;j<col;j++){
            int temp;
            cin>>temp;
            temp_vec.push_back(temp);
            temp_dp.push_back(0);
        }
        arr.push_back(temp_vec);
        dp.push_back(temp_dp);
    }
    
    /*******************/

    GETDP();

	cout << dp[row - 1][col - 1];

	return 0;
}


```

