## DFS之全排列【一维】

处理思路：
思考：这样思考问题：N＝3，代表了有1、2、3个空位置和写有A、B、C的卡片，我们需要将卡片放到空位置上，并且每个位置只能放一张卡片，现在我们需要找出这3张卡片的所有不同摆放方法。
约定一个顺序：如在位置1的时候，我们手里有A、B、C三张卡片，需要考虑应该先放哪张卡片。但是既然是要找到所有的可能，所以三种可能都需要尝试,我们可以约定一个先后顺序 A -> B -> C。

根据约定顺序可以放置卡片了，可以得到:A-> 1；B-> 2；C-> 3，这时我们已经得到了一种全排 (A,B,C)。而现在我们实际上已经走到了一个并不存在的位置4（结束位置，但是我们排列并没有结束）**;现在我们需要立即回到位置3，取回卡片C，然后尝试是否还能尝试放入其它卡片**（当然是按照第一步我们的约定A->B->C顺序），结果显然是我们手里并没有其它可以放入的卡片；我们需要继续回退一步来到位置2，取出卡片B，然后继续尝试是否能尝试放入其它卡片（当然是按照第一步我们的约定A->B->C顺序），这时我们发现可以放入卡片C；放入卡片后，我们向前一步来到位置3，尝试放入卡片（当然是按照第一步我们的约定A->B->C顺序），这时我们发现可以放入卡片B；放完卡片，我们继续向前一步来到了并不存在的位置4；这时我们得到了另一种全排 (A, C, B)；按照上述步骤重复下去，就可以得到所有的全排。

```cpp
#include<iostream>
using namespace std;
using std::cout;
using std::cin;
 
int n = 3;
int seat[3] = { 0 };//三个位置，所放的数字先全部用0初始化
int mark[3] = { 0 };//分别标记0，1，2三个数组是否放置在了位置上，如已经放置值用1表示，没放置用0。
 
 
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
		if (mark[card_id] == 0)//如果卡card_id还没有被使用
		{
			seat[seat_id] = card_id;//将卡片放到位置上
			mark[card_id] = 1;//标记为已使用  
			dfs_permutations(seat_id + 1);//进入下一步，在下一个位置再次执行
			mark[card_id] = 0;//!!!!!!!!!!!!!!!将尝试过（放到位置上）的卡片收回
		}		
	}
	return;//结束函数，返回到调用处
}
 
 
int main(int argc, char* argv[])
{
	dfs_permutations(0);
	system("pause");
	return 0;
}
--------------------- 

```

## 字符串的排列

输入一个字符串，打印其所有排列；如输入abc=》abc\acb\bac\cab\cba

如何分治减治？

思路：**递归法，问题转换为先固定第一个字符，求剩余字符的排列；求剩余字符排列时跟原问题一样。**

(1) **遍历出所有可能出现在第一个位置的字符**（即：依次将第一个字符同后面所有字符交换）；

(2) **固定第一个字符，求后面字符的排列**（即：在第1步的遍历过程中，插入递归进行实现）。

需要注意的几点：

(1) 先确定**递归结束的条件**，例如本题中可设begin == str.size() - 1; 

(2) 形如 **aba** 或 **aa** 等特殊测试用例的情况，vector在进行push_back时是不考虑重复情况的，需要自行控制；

(3) 输出的排列可能**不是按字典顺序排列**的，可能导致无法完全通过测试用例，考虑输出前排序，或者递归之后取消复位操作。



```cpp
void Permutation_sub(char* pstr, char* begin){
    if(*pbegin == '\0'){
        printf("%s\n",pstr);
    }
    
    for(char* pch = begin;*pch!='\0',pch++){
        swap(*pch,*pbegin);//人人都可以当头
        Permutation(pstr,begin+1);
        swap(*pch,*pbegin);
        
    }
}
```



```cpp
class Solution {
public:
    
    vector<string> Permutation(string str) {
        vector<string> ans;
        if(str.empty()) return ans;
        Permutation_sub(str,ans,0);
        
         // 此时得到的result中排列并不是字典顺序，可以单独再排下序
        sort(ans.begin(),ans.end());
        return ans;
    }
    
    void Permutation_sub(string pstr, vector<string> &ans, int step){
        if(step == pstr.size()-1){
            // 如果result中不存在str，才添加；避免aa和aa重复添加的情况
            if(find(ans.begin(),ans.end(),pstr)==ans.end()) 
                  ans.push_back(pstr);
        }
        else{
            for(int i=step;i<pstr.size();++i){
                swap(pstr[i],pstr[step]);//人人都可以当头
                Permutation_sub(pstr, ans,step+1);
                swap(pstr[i],pstr[step]);
            }
        }
        
    }    
  
    
    void swap(char &fir,char &sec)
    {
        char temp = fir;
        fir = sec;
        sec = temp;
    }

};
```

