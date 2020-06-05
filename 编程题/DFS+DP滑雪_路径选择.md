# DFS+DP:滑雪

**题目：**

Michael喜欢滑雪百这并不奇怪， 因为滑雪的确很刺激。可是为了获得速度，滑的区域必须向下倾斜，而且当你滑到坡底，你不得不再次走上坡或者等待升降机来载你。Michael想知道载一个区域中最长底滑坡。区域由一个二维数组给出。数组的每个数字代表点的高度。下面是一个例子 

> 1  2  3  4 5
>
> 16 17 18 19 6
>
> 15 24 25 20 7
>
> 14 23 22 21 8
>
> 13 12 11 10 9

一个人可以从某个点滑向上下左右相邻四个点之一，当且仅当高度减小。在上面的例子中，一条可滑行的滑坡为24-17-16-1。当然25-24-23-...-3-2-1更长。事实上，这是最长的一条。
**输入**

输入的第一行表示区域的行数R和列数C(1 <= R,C <= 100)。下面是R行，每行有C个整数，代表高度h，0<=h<=10000。
**输出**

输出最长区域的长度。

> 25



**分析：**

> 已知是一个二维数组arr\[row][col];求一个线路的长度，这个线路是根据arr的选择的；模型大致可以归结为：在arr\[][]中选择合适的线路，这种遍历所有线路然后选择一个最合适的，像极了dfs。
>
> dfs用于枝剪，就是尝试每一步，那么这里的步，很明显就是arr的每个点咯：dfs(int x, int y);要知道每一步都会经过dfs遍历
>
> 然后需要一个dp\[i][j]来维护每一个点作为起点的话到底部的长度。

```cpp
# include <iostream>
# include <cstring>
 
using namespace std;
 
const int maxn = 105;
int dp[maxn][maxn];//用来维护要求的东西
int sum[maxn][maxn];
int dir[][2] = {0, 1, 0 , -1, 1, 0, -1, 0};
int r, c, maxx;
 
void dfs(int x, int y)
{
    //如果这个点计算过到底层的结果那就return掉，相对于visited。
    if(dp[x][y])
    {
        return ;
    }
    
    //以(x,y)为起点，遍历四个方向
    dp[x][y] = 1;//如果我上下左右都不行那么我当前的最优解就是1
    for(int i = 0; i < 4; i++)
    {
        int tx = x + dir[i][0];
        int ty = y + dir[i][1];
        //如果撞南墙或者不满足条件就跳过
        if((tx < 0 || tx >= r || ty < 0 || ty >= c) || sum[x][y] <= sum[tx][ty])
        {
            //为什么是continue，明细是为了走下一个方向，这个方向不行，等其他方向走完就return了
            continue;
        }
        //深搜可以走的下一个节点，并且维护目前节点的dp
        dfs(tx, ty);//走完这部其实相对于知道了（tx,ty）到底部的举例
        dp[x][y] = max(dp[x][y], dp[tx][ty] + 1);//更新值，我的上一步加+1==我的动态dp[x][y]和当前dp[x][y]比较找最大的
    }
    return ;
}
 
int main(int argc, char *argv[])
{
   while(cin >> r >> c)
   {
       memset(dp, 0, sizeof(dp));
       for(int i = 0; i < r; i++)
       {
           for(int j = 0; j < c; j++)
           {
               cin >> sum[i][j];
           }
       }
       
       //遍历每个点作为起点的情况，然后维护max
       //其实我觉得前面做表时候就可以找出最高点的坐标，然后直接dfs
       maxx = 0;
       for(int i = 0; i < r; i++)
       {
           for(int j = 0; j < c; j++)
           {
               dfs(i, j);
               maxx = max(maxx, dp[i][j]);
           }
       }
       cout << maxx << endl;
   }
 
    return 0;
}

```

