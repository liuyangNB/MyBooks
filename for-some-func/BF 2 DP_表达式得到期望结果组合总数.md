# BF 2 DP_表达式得到期望结果组合总数

题目描述

> 给定一个只由0（假）、1（真）、&（逻辑与）、|（逻辑或）和\^（异或）五种字符组成的字符串express，再给定一个布尔值desires。求出express能有多少种组合方式，可以达到desired的结果。并输出你所求出的总方案数对$10^{9}+7$取模后的值。

输入描述:

```
输出两行，第一行包含一个只有0、1、&、|和^组成的字符串。其长度小于500，第二行只有一个布尔值，代表desired。
```

输出描述:

```
输出一个整数，表示取模后的答案。
```

输入

```
1^0|0|1
false
```

输出

```
2
```

说明

```
1^((0|0)|1)和1^(0|(0|1))可以得到false
```

## 暴力递归 VS DP：

> 1 找到变量：i,j=>dp\[i][j]
>
> 2写清楚递归关系，出口dp先填表
>
> 3一般情况确定dp填写顺序

> 本题分析：
>
> **fun(bool TF, int i, int j)//三个变量应该是3维，TF是bool，所以就是两个dp表，Tmap和Fmap分别代表TF是true和false的情况，两个表要一起维护。**
>
> **出口条件是 i==j**
>
> 一般情况是 [i........j] = $\sum_{ij间所有op_k} [i...k-1]*[k+1...j]$ 
>
> **所以在dp\[i][j] 要根据dp\[i][i...j] 和 dp\[i....j][j]来确定，在矩阵中就是：**
> $$
> \begin{matrix}
> 		xx & xx & \cdots & ij\\
> 		00 & 00 & \cdots & xx\\
> 		\vdots & \vdots & \ddots & \vdots \\
> 		00 & 00 & \cdots & xx \\
> 	\end{matrix}
> $$
> **dp\[i][j]需要同行同列的数据，所以由此确定填表的顺序： j ++  i--**

```cpp
#include <memory.h>
#include<iostream>
#include<string>
#include<vector>
using namespace std;

 bool Check(string str){
 	if(str.length()%2==0) return false;
 	for(int i=0;i<(int)str.length();i++){
 		if((i%2==0)&&(str[i]!='1'&&str[i]!='0')) return false;
 		if((i%2==1)&&(str[i]!='^'&&str[i]!='|'&& str[i]!='&')) return false;
	 }
	 return true;
 }
////////////////暴力递归//////////////////////////////////
 int p(string exp,bool desired,int l,int r)
	{
         if(desired && tmap[l][r]>=0){
             return tmap[l][r];
         }
         if((!desired ) && fmap[l][r]>=0){
             return fmap[l][r];
         }

		//递归出口 
       if(l==r)
       {
       	if(exp[l]=='1')
       	{
       		return desired?1:0;
       	}else{	
       		return desired?0:1;
       	}
       }
       
       int res=0;
       //分类相加分步相乘 
       
       //desired为true分为三种情况
       if(desired)
       {
         	for (int i = l + 1; i < r; i += 2) {
				switch (exp[i]) {
				case '&':
               	 res += p(exp, true, l, i - 1) * p(exp, true, i + 1, r);
					break;
				case '|':
				 res += p(exp, true, l, i - 1) * p(exp, false, i + 1, r);
				 res += p(exp, false, l, i - 1) * p(exp, true, i + 1, r);
				 res += p(exp, true, l, i - 1) * p(exp, true, i + 1, r);
					break;
				case '^':
				 res += p(exp, true, l, i - 1) * p(exp, false, i + 1, r);
				 res += p(exp, false, l, i - 1) * p(exp, true, i + 1, r);
					break;
				}
			}
       }
       //desired为false分为三种情况
       else{
        	for (int i = l + 1; i < r; i += 2) {
				switch (exp[i]) 
				{
				case '&':
				 res += p(exp, true, l, i - 1) * p(exp, false, i + 1, r);
				 res += p(exp, false, l, i - 1) * p(exp, true, i + 1, r);
				 res += p(exp, false, l, i - 1) * p(exp, false, i + 1, r);
					break;
				case '|':
				 res += p(exp, false, l, i - 1) * p(exp, false, i + 1, r);
					break;
				case '^':
				 res += p(exp, true, l, i - 1) * p(exp, true, i + 1, r);
				 res += p(exp, false, l, i - 1) * p(exp, false, i + 1, r);
					break;
				}
			}
   
       }
      return res;
 
	}

 
int num01(string express, bool desired){
		if(express=="")
		{
			return 0;
		}
		//字符串转数组
		//char[]exp=express.toCharArray();
        if(!Check(express))
        {
        	return 0;
        }
        //递归函数
        return p(express,desired,0,express.length()-1);
 
}
///////////////////////DP///////////////////////////////
 long long Bruforce2DP(string str, bool desired) {
    long long  M = 1000000007;
	int length = str.length();
	vector<vector<long long> > tmap(length, vector<long long>(length));
	vector<vector<long long> > fmap(length, vector<long long>(length));
	if (Check(str) == false) return 0;

	//出口情况，l=r，也就是只有一个数字的情况;（其实这里的出口情况可以放到一般情况，这里只是为了好分析）
	for (int i = 0; i < length; i += 2) {
		tmap[i][i] = str[i] == '1' ? 1 : 0;
		fmap[i][i] = str[i] == '0' ? 1 : 0;
	}

	//一般情况,先自己在草稿上明确dp[i][j]来自哪里，从而确定方向
	for (int j = 2; j < length; j += 2) {

		for (int i = j ; i >= 0; i--) {
			for (int k = i+1; k < j; k += 2) {//k是[i,j]之间的op
				if (str[k] == '&') {
					tmap[i][j] += tmap[i][k-1]*tmap[k+1][j];
					fmap[i][j] += tmap[i][k-1]*fmap[k+1][j]+fmap[i][k-1]*tmap[k+1][j]+fmap[i][k-1]*fmap[k+1][j];
                    tmap[i][j] %= M;//因为太大，所以路上就mod起来
                    fmap[i][j] %= M;
				}
				else if (str[k] == '|') {
					tmap[i][j] += tmap[i][k-1]*fmap[k+1][j]+fmap[i][k-1]*tmap[k+1][j]+tmap[i][k-1]*tmap[k+1][j];
					fmap[i][j] += fmap[i][k-1]*fmap[k+1][j];
                    tmap[i][j] %= M;//因为太大，所以路上就mod起来
                    fmap[i][j] %= M;
				}
				else if (str[k] == '^') {
					tmap[i][j] += tmap[i][k-1]*fmap[k+1][j]+fmap[i][k-1]*tmap[k+1][j];
					fmap[i][j] += tmap[i][k-1]*tmap[k+1][j]+fmap[i][k-1]*fmap[k+1][j];
                    tmap[i][j] %= M;//因为太大，所以路上就mod起来
                    fmap[i][j] %= M;
				}

			}
		}
	}

	return desired?tmap[0][length-1]:fmap[0][length-1];
	
}

///////////////////////////////////////////////////////////

 
int main()
{
	//memset(fmap,-1,sizeof fmap);
	//memset(tmap,-1,sizeof fmap);
    string str;// = "1^0|0|1";
    string expect;
    cin>>str;
    cin>>expect;
    bool TF = expect == "true"? true:false;
    //cout<<num01(str,false)<<endl;
    cout<<Bruforce2DP(str,TF);

}

```
