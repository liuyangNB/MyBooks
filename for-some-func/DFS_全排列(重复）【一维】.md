## DFS之全排列（重复）【一维】

去重思路:以[1,2,2]为例，可以要求index=1的数字2在最后生成的排列中一定要在index=2的数字2的前面,强制规定了顺序，避免了重复。即先从小到大排序，使相同的数字聚在一起，若前后2个数字相同，访问后一个数字的时候，前一个数字必须已经访问过了。只需添加一行代码

```cpp
#include<iostream>
#include<algorithm>
using namespace std;
using std::cout;
using std::cin;
 
//int n = 3;
#define n 3
char seat[n] = { 0 };//三个位置，所放的数字先全部用0初始化
int mark[n] = { 0 };//分别标记0，1，2三个数组是否放置在了位置上，如已经放置值用1表示，没放置用0。
char card[] = {'2','1','2'};
 
void dfs_permutations(int seat_id)
{
 
	if (seat_id == n)//位置是从0开始的，如果将卡片放完一遍，则退出
	{
		for (int i = 0; i < n; i++)//打印每个位置上所放置的卡片号
		{
			cout << seat[i]<< " ";
		}
		cout << endl;
		return;  //需要回退到调用处
	}
 
    /****每个位置按顺序放牌****/
	for (int card_id = 0; card_id < n; card_id++)//将所有可能的卡片尝试放到当前位置上
	{
		//有重复的情况就要判断，前一个位置如果是和现在相同且没有访问过就跳过
		if(card_id>0 && card[card_id]==card[card_id-1] && mark[card_id-1]==0) continue; 
		 
		if (mark[card_id] == 0)//如果卡card_id还没有被使用
		{
			seat[seat_id] = card[card_id];//将卡片放到位置上
			mark[card_id] = 1;//标记为已使用  
			dfs_permutations(seat_id + 1);//进入下一步，在下一个位置再次执行
			mark[card_id] = 0;//!!!!!!!!!!!!!!!将尝试过（放到位置上）的卡片收回
		}		
	}
	return;//结束函数，返回到调用处
}
 
 
int main(int argc, char* argv[])
{
	sort(card,card+n);
	dfs_permutations(0);
	//system("pause");
	return 0;
}
```

## 
