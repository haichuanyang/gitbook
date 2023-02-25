---
description: 219 problems; 52 videos
---

# algo advanced

### 龟速乘,&#x20;

```
//神奇O(1)快速乘

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

### RMQ - 1 problem 1273

range max (or min) query - segment tree can also solve it; query only no edit; st list/jump skip list

1273, f\[i,j] = max of starting at i, 2^j sized segment&#x20;

f\[i,j]=max{f\[i,j-1], f\[i+2^(j-1), j-1]&#x20;

O(nlogn)

for \[L, R], f\[L,k], f\[R-2^k-1,k], [log base switch/change formula](https://www.rapidtables.com/math/algebra/Logarithm.html)

no editing supported; shorter code

segment tree - can edit, longer code

```cpp
#include <cstdio>
#include <cstring>
#include <algorithm>
#include <cmath>

using namespace std;

const int N = 200010, M = 18;

int n, m;
int w[N];
int f[N][M];

void init()
{
    for (int j = 0; j < M; j ++ )
        for (int i = 1; i + (1 << j) - 1 <= n; i ++ )
            if (!j) f[i][j] = w[i];
            else f[i][j] = max(f[i][j - 1], f[i + (1 << j - 1)][j - 1]);
            // 1<< j was wrong!
}

int query(int l, int r)
{
    int len = r - l + 1;
    int k = log(len) / log(2);

    return max(f[l][k], f[r - (1 << k) + 1][k]);
}

int main()
{
    scanf("%d", &n);
    for (int i = 1; i <= n; i ++ ) scanf("%d", &w[i]);

    init();

    scanf("%d", &m);
    while (m -- )
    {
        int l, r;
        scanf("%d%d", &l, &r);
        printf("%d\n", query(l, r));
    }

    return 0;
}

```

### flood fill

4-connected: with common edge

8-connected: with common point

1097 connected area counting

```cpp
//dfs
#include<bits/stdc++.h>
using namespace std;
int n,m,ans,a[1010][1010];
char Map[1010][1010];
int dx[8]={1,0,-1,0,1,-1,1,-1},dy[8]={0,1,0,-1,1,-1,-1,1};//方向数组
void dfs(int x,int y)
{
    Map[x][y]='.';
    for(int i=0;i<8;i++)
        if(Map[x+dx[i]][y+dy[i]]=='W')
            dfs(x+dx[i],y+dy[i]);
}
int main()
{
    scanf("%d %d",&n,&m);
    for(int i=1;i<=n;i++)
        for(int j=1;j<=m;j++)
            cin>>Map[i][j];
    for(int i=1;i<=n;i++)
        for(int j=1;j<=m;j++)
            if(Map[i][j]=='W'){//找到“ W ”.
                dfs(i,j);
                ans++;//统计答案
            }
    printf("%d\n",ans);
    return 0;
}

作者：yingzhaoyang
链接：https://www.acwing.com/solution/content/5859/

```

```cpp
//yxc bfs
#include <cstring>
#include <iostream>
#include <algorithm>

#define x first
#define y second

using namespace std;

typedef pair<int, int> PII;

const int N = 1010, M = N * N;

int n, m;
char g[N][N];
PII q[M];
bool st[N][N];

void bfs(int sx, int sy)
{
    int hh = 0, tt = 0;
    q[0] = {sx, sy};
    st[sx][sy] = true;

    while (hh <= tt)
    {
        PII t = q[hh ++ ];

        for (int i = t.x - 1; i <= t.x + 1; i ++ )
            for (int j = t.y - 1; j <= t.y + 1; j ++ )
            {
                if (i == t.x && j == t.y) continue;
                if (i < 0 || i >= n || j < 0 || j >= m) continue;
                if (g[i][j] == '.' || st[i][j]) continue;

                q[ ++ tt] = {i, j};
                st[i][j] = true;
            }
    }
}

int main()
{
    scanf("%d%d", &n, &m);
    for (int i = 0; i < n; i ++ ) scanf("%s", g[i]);

    int cnt = 0;
    for (int i = 0; i < n; i ++ )
        for (int j = 0; j < m; j ++ )
            if (g[i][j] == 'W' && !st[i][j])
            {
                bfs(i, j);
                cnt ++ ;
            }

    printf("%d\n", cnt);

    return 0;
}

```



![城堡问题.png](https://cdn.acwing.com/media/article/image/2020/01/21/1099\_c443eb1a3b-%E5%9F%8E%E5%A0%A1%E9%97%AE%E9%A2%98.png)







```
//yxc flood fill 2 castle problem


#include <cstring>
#include <iostream>
#include <algorithm>

#define x first
#define y second

using namespace std;

typedef pair<int, int> PII;

const int N = 55, M = N * N;

int n, m;
int g[N][N];
PII q[M];
bool st[N][N];

int bfs(int sx, int sy)
{
    int dx[4] = {0, -1, 0, 1}, dy[4] = {-1, 0, 1, 0};

    int hh = 0, tt = 0;
    int area = 0;

    q[0] = {sx, sy};
    st[sx][sy] = true;

    while (hh <= tt)
    {
        PII t = q[hh ++ ];
        area ++ ;

        for (int i = 0; i < 4; i ++ )
        {
            int a = t.x + dx[i], b = t.y + dy[i];
            if (a < 0 || a >= n || b < 0 || b >= m) continue;
            if (st[a][b]) continue;
            if (g[t.x][t.y] >> i & 1) continue;

            q[ ++ tt] = {a, b};
            st[a][b] = true;
        }
    }

    return area;
}

int main()
{
    cin >> n >> m;
    for (int i = 0; i < n; i ++ )
        for (int j = 0; j < m; j ++ )
            cin >> g[i][j];

    int cnt = 0, area = 0;
    for (int i = 0; i < n; i ++ )
        for (int j = 0; j < m; j ++ )
            if (!st[i][j])
            {
                area = max(area, bfs(i, j));
                cnt ++ ;
            }

    cout << cnt << endl;
    cout << area << endl;

    return 0;
}


```

```cpp
//yxc floodfill 3 peaks and valleys
#include <cstring>
#include <iostream>
#include <algorithm>

#define x first
#define y second

using namespace std;

typedef pair<int, int> PII;

const int N = 1010, M = N * N;

int n;
int h[N][N];
PII q[M];
bool st[N][N];

void bfs(int sx, int sy, bool& has_higher, bool& has_lower)
{
    int hh = 0, tt = 0;
    q[0] = {sx, sy};
    st[sx][sy] = true;

    while (hh <= tt)
    {
        PII t = q[hh ++ ];

        for (int i = t.x - 1; i <= t.x + 1; i ++ )
            for (int j = t.y - 1; j <= t.y + 1; j ++ )
            {
                if (i == t.x && j == t.y) continue;
                if (i < 0 || i >= n || j < 0 || j >= n) continue;
                if (h[i][j] != h[t.x][t.y]) // 山脉的边界
                {
                    if (h[i][j] > h[t.x][t.y]) has_higher  = true;
                    else has_lower = true;
                }
                else if (!st[i][j])
                {
                    q[ ++ tt] = {i, j};
                    st[i][j] = true;
                }
            }
    }
}

int main()
{
    scanf("%d", &n);

    for (int i = 0; i < n; i ++ )
        for (int j = 0; j < n; j ++ )
            scanf("%d", &h[i][j]);

    int peak = 0, valley = 0;
    for (int i = 0; i < n; i ++ )
        for (int j = 0; j < n; j ++ )
            if (!st[i][j])
            {
                bool has_higher = false, has_lower = false;
                bfs(i, j, has_higher, has_lower);
                if (!has_higher) peak ++ ;
                if (!has_lower) valley ++ ;
            }

    printf("%d %d\n", peak, valley);

    return 0;
}

作者：yxc
链接：https://www.acwing.com/activity/content/code/content/130795/

```

basic DP 3 video skip first 40 minutes  - wrong way! beware mistakes everywhere

flood fill is in advanced class

```
第六章 基础算法完成情况：0/12
包括位运算、递推与递归、前缀和与差分、二分、排序、RMQ等内容
第五章 数学知识完成情况：0/35
包括筛质数、分解质因数、快速幂、约数个数、欧拉函数、同余、矩阵乘法、组合计数、高斯消元、容斥原理、概率与数学期望、博弈论等内容
第四章 高级数据结构完成情况：0/21
包括并查集、树状数组、线段树、可持久化数据结构、平衡树、AC自动机等内容
第三章 图论完成情况：0/58
包括单源最短路的建图方式、单源最短路的综合应用、单源最短路的扩展应用、Floyd算法、最小生成树、最小生成树的扩展应用、负环、差分约束、最近公共祖先、强连通分量、双连通分量、二分图、欧拉回路和欧拉路径、拓扑排序等内容
第二章 搜索完成情况：0/25
包括Flood Fill、最短路模型、多源BFS、最小步数模型、双端队列广搜、双向广搜、A*、DFS之连通性模型、DFS之搜索顺序、DFS之剪枝与优化、迭代加深、双向DFS、IDA*等内容
第一章 动态规划完成情况：0/68
包括数字三角形模型、最长上升子序列模型、背包模型、状态机、状态压缩DP、区间DP、树形DP、数位DP、单调队列优化DP、斜率优化DP等内容
```
