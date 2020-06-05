## DFS遍历——图解过程



注：

存储结构不确定，遍历结果不唯一
其中一种DFS序列：DFS(G,v1) = (v1,v2,v3,v6,v5,v7,v4,v8,v9)
小结

从图解和伪代码中我们可以看出，DFS其实就是找准一条路径（我们可以暂且这样认为），一直走下去（深度优先），直到满足条件（这属于运气好了）或者走投无路、走不下去（撞南墙了）。如果没有找到相应的结果状态，那么就回退一步（回溯），再进行下一步找路径，再撞再回溯，一直到找到目标状态或者所有情况都遍历完，程序结束

## DFS递归算法框架

```cpp
/*
该DFS 框架以2D 坐标范围为例，来体现DFS 算法的实现思想。
*/
#include<cstdio>
#include<cstring>
#include<cstdlib>
using namespace std;
const int maxn=100;
bool vst[maxn][maxn]; // 访问标记
int map[maxn][maxn]; // 坐标范围
int dir[4][2]={0,1,0,-1,1,0,-1,0}; // 方向向量，(x,y)周围的四个方向

bool CheckEdge(int x,int y) // 边界条件和约束条件的判断
{
    if(!vst[x][y] && ...) // 满足条件
        return 1;
    else // 与约束条件冲突
        return 0;
}

void dfs(int x,int y)
{
    vst[x][y]=1; // 标记该节点被访问过
    //判断满足要求否
    if(map[x][y]==G) // 出现目标态G
    {
        ...... // 做相应处理
    return;
    }
    //不满足尝试下一步
    for(int i=0;i<4;i++)
    {
        if(CheckEdge(x+dir[i][0],y+dir[i][1])) // 按照规则生成下一个节点
        dfs(x+dir[i][0],y+dir[i][1]);
    }
    return; // 没有下层搜索节点，回溯
}
int main()
{
    ......
    return 0;

}
```

<https://blog.csdn.net/qq_39826163/article/details/81261338>
