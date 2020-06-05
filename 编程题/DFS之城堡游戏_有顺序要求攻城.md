# wjw和ly的双人游戏

【题意】

> 有N个城堡，每个城堡都有宝物，每局游戏中允许他们攻破M个城堡并且获得奖励，但是由于地图的限制，有些城堡不能直接攻破，要攻破这些城堡必须先攻破其他特定的城堡，计算获得的最多宝物数量。

【解题思路】

> 还是依次遍历，每当达到特定城堡数时判断一下是否可行，若可行则更新ans。

【代码】

```cpp
using namespace std;
int f[15],a[15],v[15];
int n,m,ans;
int find()//计算当满足城堡数量时的宝物数量
{
    int sum=0,cnt=0;
    for(int i=1;i<=n;i++){
        if(v[i]){
            if(!f[i] || v[f[i]]){
                sum+=a[i];
                cnt++;
            }
        }
    }
    if(cnt==m)return sum;
    else return 0;//若这些城堡中有城堡 需要攻击的前城堡没被攻击过 则不能计算入内 所以返回0
}
void dfs(int x,int c)
{
    if(c==m){
        ans=max(ans,find());
        return;
    }
    if(x==n+1)return;
    v[x]=1;
    dfs(x+1,c+1);//攻击当前城堡
    v[x]=0;
    dfs(x+1,c);//不攻击当前城堡
}
int main()
{
    while(~scanf("%d%d",&n,&m) && n || m){
        for(int i=1;i<=n;i++)scanf("%d%d",&f[i],&a[i]);
        ans=0;
        memset(v,0,sizeof(v));
        dfs(0,0);
        printf("%d\n",ans);
    }
    return 0;
}
```

