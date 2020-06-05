## DFS之素数环【一维】

hdu1016 Prime Ring Problem（素数环）
【题意】

给出1-n的整数，看是否能填入环中，使相邻两个数的和为素数（1为起点）。

问题描述：将从1到n这n个整数围成一个圆环，若其中任意2个相邻的数字相加，结果均为素数，那么这个环就成为素数环。

n=20时，下面的序列就是一个素数环：

1 2 3 4 7 6 5 8 9 10 13 16 15 14 17 20 11 12 19 18

【解题思路】

其实这题和上面一题很像，只是多了一步判断素数，同样也是挨个遍历，因为是个环记得输出时要判断一下起点和终点的和是不是素数再输出。

```cpp
#include<bits/stdc++.h>
using namespace std;
int a[25],v[25],k=1,n;]
//判断a是否为素数,是返回1，不是返回0；对于大数的素数适合于其他方法
int isprime(int a)
{
    int flag=1;
    if(a==1)return 0;
    else if(a==2)return 1;
    else{
        for(int i=2;i<a;i++){
            if(a%i==0){
                flag=0;
                break;
            }
        }
        if(flag)return 1;
        else return 0;
    }
}
void dfs(int c)
{
    //a[i]是位置i放的数；dfs（c）是到位置c
    if(c==n && isprime(a[0]+a[c-1])){
        printf("%d",a[0]);
        for(int i=1;i<n;i++)printf(" %d",a[i]);
        printf("\n");
    }
    else{
        //i是遍历可以选择的数据，1-n；c才是下一个位置的选择
        for(int i=2;i<=n;i++){
            if(!v[i]){
                if(isprime(i+a[c-1])){
                    v[i]=1;
                    a[c]=i;
                    dfs(c+1);
                    v[i]=0;//标记复原！！！！供回退
                }
            }
        }
    }
}
int main()
{
    while(~scanf("%d",&n)){    
        memset(v,0,sizeof(v));
        memset(a,0,sizeof(a));
        printf("Case %d:\n",k++);
        a[0]=1;    
        dfs(1);
        printf("\n");
    }
    return 0;
}
--------------------- 

```

