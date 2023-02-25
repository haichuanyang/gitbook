---
description: 全是讲题，思路+代码！做题做题; 知识点 - 没有模板； 全是讲题，思路+代码！做题做题，题目按知识点分类; 每次4-5 题
---

# 6basic 3 videos

### 龟速乘,&#x20;

```
//神奇O(1)快速乘 vs O(log y) 龟速乘

#include<bits/stdc++.h>
using namespace std;
typedef long long ll;
inline ll ksc(ll x,ll y,ll mod)
{
    return (x*y-(ll)((long double)x/mod*y)*mod+mod)%mod;     
}
int main()
{
    ll a, b, p;
    cin >> a >> b >> p;
    cout << ksc(a, b, p);
}
```

iostream and algorithm and bits/stdc++.h has many header files included which is slower; cstdio and cstring very small and fast; compile time is not included in usaco time limit, but improves user experience

topcoder C type 100 problems; 陈立杰



第六章 基础算法完成情况：0/12 包括位运算、递推与递归、前缀和与差分、二分、排序、RMQ等内容&#x20;

位运算 AcWing 90. 64位整数乘法336人打卡

递推与递归 AcWing 95. 费解的开关231人打卡 AcWing 97. 约数之和159人打卡 AcWing 98. 分形之城113人打卡&#x20;

前缀和与差分 AcWing 99. 激光炸弹199人打卡 AcWing 100. 增减序列169人打卡&#x20;

二分 AcWing 102. 最佳牛围栏184人打卡 AcWing 113. 特殊排序126人打卡&#x20;

排序 AcWing 105. 七夕祭124人打卡 AcWing 106. 动态中位数141人打卡 AcWing 107. 超快速排序143人打卡&#x20;

RMQ AcWing 1273. 天才的记忆



last video - 1:34:00 all over; rest is talk waste of time
