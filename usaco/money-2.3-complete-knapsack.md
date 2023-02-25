# money  2.3 complete knapsack

```
#include <iostream>
using namespace std;

typedef long long LL; 

const int N = 10010;
int n, m;
LL f[N];

int main() // 完全背包问题求解总方案数
{
    cin >> n >> m;
    f[0] = 1; 
    for (int i = 0; i < n; ++i) {
        int v;
        cin >> v;
        for (int j = v; j <= m; ++j) {
            f[j] += f[j - v];
        }
    }
    cout << f[m] << endl;
    return 0;
}

作者：Fool_one
链接：https://www.acwing.com/solution/content/28495/

```

```
AcWing 1021. 货币系统    原题链接    简单
作者：    wyc1996 ,  2020-11-28 10:42:03 ,  阅读 48

1


题目描述
给你一个n种面值的货币系统，求组成面值为m的货币有多少种方案。

算法：完全背包
时间复杂度O(N∗MN∗M)
需要注意
(1)状态表示为体积恰好为j的方案的数量,需要对f[0]进行初始化
(2)答案可能会爆int,需要用long long 存储答案
C++代码
#include<iostream>

using namespace std;
typedef long long LL;
const int N=20,M=3010;
int w[N];
LL f[M];

int main()
{
    int n,m;
    cin>>n>>m;

    f[0]=1;
    for(int i=1;i<=n;i++){
        scanf("%d",&w[i]);
        for(int j=w[i];j<=m;j++){
            f[j]+=f[j-w[i]];
        }
    }
    cout<<f[m]<<endl;
    return 0;
}

作者：wyc1996
链接：https://www.acwing.com/solution/content/26104/

```
