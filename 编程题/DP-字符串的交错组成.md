# DP-字符串的交错组成

题目描述

> 给定三个字符串str1、str2 和aim,如果aim包含且仅包含来自str1和str2的所有字符，而且在aim中属于str1的字符之间保持原来在str1中的顺序属于str2的字符之间保持原来在str2中的顺序，那么称aim是str1和str2的交错组成。实现一个函数，判断aim是否是str1和str2交错组成，如果是请输出“YES”，否则输出“NO”。

示例1

输入

```
AB
12
1AB2
```

输出

```
YES
```

```cpp
#include <iostream>
#include <vector>
#include <string>
#include <cmath>
using namespace std;
bool isCross1(string str1, string str2, string aim)
{
    //基本条件不满足就false
    if(aim.size() != str1.size() + str2.size())
        return false;
    //dp[][]
    vector<vector<bool> > dp(str1.size() + 1, vector<bool>(str2.size() + 1));
    
    /*
    dp[i][j]指aim[i+j-1]能否由str1[i]和str2[j]组成，分析其可能来自哪些子问题：
    dp[i-1][j]=true && aim[i+j-1]==str1[i-1]==>dp[i][j]=true
    dp[i][j-1]=true && aim[i+j-1]==str2[j-1]==>dp[i][j]=true
    其他情况都是false
    */
    
    dp[0][0] = true;
    for(int i = 1; i <= str1.size(); ++i)
        if(str1[i - 1] == aim[i - 1])
            dp[i][0] = true;
        else
            break;
    for(int j = 1; j <= str2.size(); ++j)
        if(str2[j - 1] == aim[j - 1])
            dp[0][j] = true;
        else
            break;

    for(int i = 1; i <= str1.size(); ++i)
        for(int j = 1; j <= str2.size(); ++j)
        if((dp[i - 1][j] && str1[i - 1] == aim[i + j - 1])
        || (dp[i][j - 1] && str2[j - 1] == aim[i + j - 1]))
        dp[i][j] = true;
    return dp[str1.size()][str2.size()];
}
bool isCross2(string str1, string str2, string aim)
{
    if(aim.size() != str1.size() + str2.size())
        return false;
    string longstr = str1.size() >= str2.size() ? str1 : str2;
    string shortstr = longstr == str1 ? str2 : str1;
    vector<bool> dp(shortstr.size() + 1);
    dp[0] = true;
    for(int i = 1; i < shortstr.size() + 1; ++i)
        if(shortstr[i - 1] == aim[i - 1])
            dp[i] = true;
        else
            break;

    for(int i = 1; i <= longstr.size(); ++i)
    {
        dp[0] = dp[0] && longstr[i - 1] == aim[i - 1];
        for(int j  = 1; j <= shortstr.size(); ++j)
        {
            if((dp[j] && longstr[i - 1] == aim[i + j -1])
                || (dp[j - 1] && shortstr[j - 1] == aim[i + j - 1]))
                dp[j] = true;
            else
                dp[j] = false;
        }
    }
    return dp[shortstr.size()];
}
int main()
{
    string str1 = "AB";
    string str2 = "12";
    string aim =  "1AB2";
    cout << isCross1(str1, str2, aim) << endl;
    cout << isCross2(str1, str2, aim) << endl;
}

```

