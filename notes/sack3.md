# sack3









```
基础课背包问题的总结（写给自己看的）

1.01 背包问题
N 个数 V 容量
选个i物品放入N,V的背包且满足j<V
对于第i个物品只有两种状态放和不放
不放 f[i-1][j]
放 f[i-1][j-v[i]]+w[i]

两者选择最大即为当前i个物品的最大值

所以 状态转移方程为 f[i][j]=max(f[i-1][j],f[i-1][j-v[i]]+w）

利用滚动数组 优化  
因为 每次都用到的是上一次的状态 
所以 可以取消第一维 
需要对方程做等值变换
每次更新f[i][j] 的时候因为是上一次的状态 所以实际上 f[i][j]==f[i-1][j]
因为每次更新的时候 f[i][j] 都还不存在

若直接删去一维会导致 等式不对等   
f[i-1][j-v[i]]=f[j-v[i]]
还需要把j 从最大开始枚举 因为从最大开始就不存在i-1的问题

因为状态都变成了i开始

暴力写法


for(int i=1;i<=n;i++)
    for(int j=0;j<=m;j++)
    {
        f[i][j]=f[i-1][j];
        if(j>=v[i]) f[i][j]=max(f[i][j],f[i-1][j-v[i]]+w[i]);
    }
cout<<f[n][m];
return 0;
} ``
一维化简后的

for(int i=1;i<=n;i++)
    for(int j=m;j>=v[i];j--)
    {
         f[j]=max(f[j],f[j-v[i]]+w[i]);
    }
cout<<f[m];
return 0;
}

2. 完全背包问题
新增 物品有无数个

状态转移变成 从选i个物品 选k个 放到j大小的背包

所以状态转移方程为f[i, j] = max(f[i - 1, j], f[i, j - k * v] + k * w)
因为多了一个k 所以枚举的时候需要多一重循环

优化可以对等式进行优化    f[i][j]=max(f[-1][j],f[i-1][j-v]+w,f[i-1][j-2v]+2w,f[i-1][j-3v]+3w...f[i-1][j-k*v]+k*w)
                          f[i][j-v]=max(       f[i-1][j-v],f[i-1][j-2v]+W,f[[i-1][j-3v]+2w,...f[i-1][j-k*v]+k*w)

所以状态转移方程变为** f[i][j]=max(f[i[j],f[i][j-v[i]]+w[i]);**

因为每次都是用的 第i个状态所以可以从小到大枚举
 所以最终方程为  f[j]=max(f[j],f[j-v[i]+w[i]])
 int main()
{
    int n,m;
    cin>>n>>m;
    for(int i=1;i<=n;i++) cin>>v[i]>>w[i];

    for(int i=1;i<=n;i++)
    for(int j=v[i];j<=m;j++)
    {
        f[j]=max(f[j],f[j-v[i]]+w[i]);
    }
    cout<<f[m];
    return 0;
}
3. 多重背包问题
可以设置为有每i个物品有k个

多重背包可以转化为01背包问题 状态表示 依旧为f[i][j]
集合为 选i个物品满足容量j 限制条件为k个

只需要在01背包的基础上增加一个k 把每次选或不选变成 每次第k个i物品选或不选

状态转移方程为 f[i][j]=max(f[i][j],f[i][j-kv[i]+w[i]k]);

#include<iostream>
using namespace std;

const int N=110;

int f[N],w,v,s;

int main()
{
    int n,m;
    cin>>n>>m;
    for(int i=1;i<=n;i++)
    {
        cin>>v>>w>>s;
        for(int j=m;j>=1;j--)
        for(int k=1;k<=s&&k*v<=j;k++)
            f[j]=max(f[j],f[j-k*v]+w*k);
    }

    cout<<f[m];
    return 0;
}
使用二进制对多重进行优化
若i物品有s件则挑选的组合为2的n次方 因为对于物品的个数都有选或不选两种方法
所以每个物品的数量可以利用二进制进行分解 进行分解之后 就转化成一个 01背包问题 然后用01背包求解

#include<iostream>
#include<cstdio>
#include<algorithm>
#include<vector>
using namespace std;

const int N=2010;
int f[N],s;

struct good
{
    int v,w;
};

int main()
{
    vector<good> lyt;
    int m,n;
    cin>>m>>n;
    for(int i=1;i<=m;i++)
    {
        int v, w, s;
        cin>>v>>w>>s; 
        for(int j=1;j<=s;j<<=1)
        {
            lyt.push_back({j*v,j*w});
            s-=j;
        }
        if(s>0) lyt.push_back({s*v,s*w});
    }
    for( auto zy : lyt )
    {
        for(int i=n;i>=zy.v;i--)
        f[i]=max(f[i],f[i-zy.v]+zy.w);
    }
    cout<<f[n];

}
```
