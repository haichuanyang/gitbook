# tree shape DP

```cpp
(树形DP) O(n)O(n)
题解里的人都建图了啊，但是这题明明可以不用这么麻烦的……

本题稍加思索即可得出为树形dp。

每个人只有两种状态，则设dp[0][i]dp[0][i]为第ii个人不来，他的下属所能获得的最大快乐值；dp[1][i]dp[1][i]为第ii个人来，他的下属所能获得的最大快乐值。

所以容易推出状态转移方程：

dp[0][i]=∑u=sonsmax(dp[1][u],dp[0][u])dp[0][i]=∑u=sonsmax(dp[1][u],dp[0][u])当前节点不选，那么子节点随意

dp[1][i]=∑u=sonsdp[0][u]+happy[i]dp[1][i]=∑u=sonsdp[0][u]+happy[i]当前节点选，子节点不能选

分析可得，每个人的状态要在下属的状态更新完了才能更新，所以用类似拓扑的方法，只记录每个子节点的父亲，最后从所有入度为00的点开始跑就行了。在更新每个子节点时顺便让父节点加上自己的权值，最后访问父节点时权值已经更新好了，就可以省去建图的麻烦。

C++ 代码
#include <iostream>
#include <cstdio>
using namespace std;
int dp[2][6010];//dp解释见上
int f[2][6010];//f[0]为父亲，f[1]为高兴值
int ind[6010];//入度
int vis[6010];//访问标记
int root;//树的根
void dfs(int u){//递归从后往前更新
    if(!u) return;
    vis[u]=1;//已访问
    root=u;//最后一个访问到的一定是根，所以一直更新根就行了
    dp[0][f[0][u]]+=max(dp[1][u]+f[1][u],dp[0][u]);//给父亲更新
    dp[1][f[0][u]]+=dp[0][u];
    ind[f[0][u]]--;//更新完一个子节点
    if(!ind[f[0][u]]) dfs(f[0][u]);//在所有子节点更新后再更新（入度为0）
}
int main(){
    int n=0;
    scanf("%d",&n);
    for(int i=1;i<=n;i++)
        scanf("%d",&f[1][i]);
    int a,b;
    for(int i=1;i<n;i++){
        scanf("%d%d",&a,&b);
        f[0][a]=b;//保存节点信息
        ind[b]++;
    }
    for(int i=1;i<=n;i++)
        if(!vis[i]&&!ind[i])//没有被访问过，没有入度，说明是叶子节点
            dfs(i);
    printf("%d\n",max(dp[0][root],dp[1][root]+f[1][root]));//取根节点两种方案的最大值
    return 0;
}

作者：Jayfeather
链接：https://www.acwing.com/solution/content/4757/

```
