## DFS之油田【二维】

Sample Input

```cpp
Sample Input
1 1 
* 
3 5 
*@*@* 
**@** 
*@*@* 
1 8 
@@****@* 
5 5 
****@ 
*@@*@ 
*@**@ 
@@@*@ 
@@**@ 
0 0
Sample Output
0 1 2 2

```



Sample Output
0 1 2 2

题意：就是给你一个地图，找出所有不相连（八个方向）的@组合有多少个

解题思路：先对整个地图进行遍历，找到一个入口，然后用DFS深搜，相连的@都找到，并且将其标记为*（这样做的好处可以避免重新用visit对遍历过的地图进行标记）

【解题思路】

计算油田数，连通（周围8块都算连通）的油田算一块，@代表油田。

只需遍历每一个点看其是否是油田，如果是并且没有被标记过则进入搜索，即搜索周围8个位置，搜索出口是遍历出界或已被标记或不是油田

```cpp
#include<bits/stdc++.h>
using namespace std;
char a[105][105],v[105][105];
int n,m;
//这样深搜是：不满足条件（出界||访问过||不是油田）就退出，也就是搜索终止；满足条件也就是，界内的@，才有资格继续搜索，这样散开后，同一块的油田将会一次性标记为visited！！！！，下一次深搜就不会访问这里了
void dfs(int x,int y)
{
    if(x<0 || y<0 || x>=n ||y>=m)return;
    if(a[x][y]=='*' || v[x][y])return;
    v[x][y]=1;//作用就是给满足要求的地块打标
    dfs(x-1,y-1);
    dfs(x-1,y);
    dfs(x-1,y+1);
    dfs(x,y-1);
    dfs(x,y+1);
    dfs(x+1,y-1);
    dfs(x+1,y);
    dfs(x+1,y+1);
}
int main()
{
    while(~scanf("%d%d",&n,&m) && n && m){
        memset(v,0,sizeof(v));
        memset(a,0,sizeof(a));
        getchar();//吃回车
        for(int i=0;i<n;i++)scanf("%s",a[i]);
        int cnt=0;
        for(int i=0;i<n;i++){
            for(int j=0;j<m;j++){
                if(a[i][j]=='@' && !v[i][j]){
                    dfs(i,j);
                    cnt++;
                }
            }
        }
        printf("%d\n",cnt);
    }
    return 0;
}
--------------------- 

```

## 
