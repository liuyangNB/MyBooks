# 换钱的方法数

题目描述：

> 给定一组arr[],代表各种面值的货币，数量无限；再给定一个aim值，代表要兑换的金额；问有多少种方法？

思路：从暴力递归到dp动规

提炼模型：

> 给定数组arr[N]={1,5,10,15},这样一个选项范围
>
> 从中凑出target这个金额的所有可能方法。（有点像整数分解）

分析过程
z
> 选择的是一个数组arr[N];要收敛就必须让数组慢慢变小，target是一个定值，但是这个定值往往是应变量。
> 用1 5 10 15 凑成1000元的所有方法可以拆分为：
>
> > 用0张1元，让剩下的【5，10，15】凑成1000
> >
> > 用1张1元，让剩下的【5，10，15】凑成1000-1
> >
> > 用2张1元，让剩下的【5，10，15】凑成1000-2
> >
> > ...
> >
> > 用i张1元，让剩下的【5，10，15】凑成1000-i*1
> >
> > 1000-1*i>=0
>
> ```cpp
> int fun1(int arr[], int index, int aim) {
> 	int res=0;
> 	//出口
> 	//只有arr[0]可以用，这时候aim不是其倍数，结果为0；是其倍数，结果为1
> 	if (index == 0) {
> 		return res=aim%arr[0] == 0 ? 1 : 0;//出口用return，不然死循环
> 	}
> 	for (int k = 0; k*arr[index] <= aim;k++) {
> 		res += fun1(arr, index - 1, aim - k * arr[index]);
> 	}
> 	return res;
> }
> ```
>
> 暴力递归=》dp（加入map查表呗）
>
> ```cpp
> int map[1024][1024] = { 0 };
> int fun2(int arr[], int index, int aim) {
> 	int res = 0;
> 	//出口
> 	if (index == 0) {
> 		res = (aim%arr[0] == 0 ? 1 : 0);
> 		map[index][aim] = res;
> 		return res;
> 	}
> 	
> 	//一般情况
> 	for (int k = 0; k <= aim / arr[index];k++) {
> 		int mapvalue = map[index - 1][aim - k * arr[index]];
> 		if (mapvalue != 0)	res += mapvalue;
> 		else res += fun2(arr, index - 1, aim - k * arr[index]);
> 	}
> 	map[index][aim] = res;
> 	return res;
> }
> ```
>
> 

