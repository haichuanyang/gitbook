# shortest hamilton path

```cpp
题目描述
给定一张 nn 个点的带权无向图，点从 0∼n−10∼n−1 标号，求起点 00 到终点 n−1n−1 的最短 HamiltonHamilton 路径。 HamiltonHamilton 路径的定义是从 00 到 n−1n−1 不重不漏地经过每个点恰好一次。

输入格式
第一行输入整数 nn。

接下来 nn 行每行 nn 个整数，其中第 ii 行第 jj 个整数表示点 ii 到 jj 的距离（记为 a[i,j]a[i,j]）。

对于任意的 x,y,zx,y,z，数据保证 a[x,x]=0a[x,x]=0，a[x,y]=a[y,x]a[x,y]=a[y,x] 并且 a[x,y]+a[y,z]≥a[x,z]a[x,y]+a[y,z]≥a[x,z]。

输出格式
输出一个整数，表示最短 HamiltonHamilton 路径的长度。

数据范围
1≤n≤201≤n≤20
0≤a[i,j]≤1070≤a[i,j]≤107
输入样例：
5
0 2 4 5 1
2 0 6 5 3
4 6 0 8 3
5 5 8 0 5
1 3 3 5 0
输出样例：
18
算法
(状态压缩DP) O(n2)O(n2)
首先想下暴力算法，这里直接给出一个例子。
比如数据有 55 个点，分别是 0,1,2,3,40,1,2,3,4
那么在爆搜的时候，会枚举一下六种路径情况（只算对答案有贡献的情况的话）：

case 1: 0→1→2→3→4case 1: 0→1→2→3→4
case 2: 0→1→3→2→4case 2: 0→1→3→2→4
case 3: 0→2→1→3→4case 3: 0→2→1→3→4
case 4: 0→2→3→1→4case 4: 0→2→3→1→4
case 5: 0→3→1→2→4case 5: 0→3→1→2→4
case 6: 0→3→2→1→4case 6: 0→3→2→1→4
那么观察一下 case 1case 1 和 case 3case 3，可以发现，我们在计算从点 00 到点 33 的路径时，其实并不关心这两中路径经过的点的顺序，而是只需要这两种路径中的较小值，因为只有较小值可能对答案有贡献。
所以，我们在枚举路径的时候，只需要记录两个属性：当前经过的点集，当前到了哪个点。
而当前经过的点集不是一个数。观察到数据中点数不会超过 2020，我们可以用一个二进制数表示当前经过的点集。其中第 ii 位为 1/0 表示是/否经过了点 ii。
然后用闫式 dp 分析法考虑 dp
状态表示：f[state][j]f[state][j]。其中 statestate 是一个二进制数，表示点集的方法如上述所示。

集合：经过的点集为 statestate，且当前到了点 jj 上的所有路径。
属性：路径总长度的最小值
状态计算：假设当前要从点 kk 转移到 jj。那么根据 HamiltonHamilton 路径的定义，走到点 kk 的路径就不能经过点 jj，所以就可以推出状态转移方程f[state][j] = min{f[state ^ (1 << j)][k] + w[k][j]}
其中w[k][j]表示从点 kk 到点 jj 的距离，^表示异或运算。
state ^ (1 << j)是将 statestate 的第 jj 位改变后的值，即

如果 statestate 的第 jj 位是 11 那么将其改为 00
否则将 statestate 的第 jj 位改为 11
由于到达点 jj 的路径一定经过点 jj，也就是说当 statestate 的第 jj 位为 11 的时候，f[state][j]f[state][j] 才可以被转移，所以 state ^ (1 << j) 其实就是将 statestate 的第 jj 位改为 00，这样也就符合了 走到点 kk 的路径就不能经过点 jj 这个条件。

所有状态转移完后，根据 f[state][j]f[state][j] 的定义，要输出 f[111⋯11(n个1)][n−1]f[111⋯11(n个1)][n−1]。
那么怎么构造 nn 个 1 呢，可以直接通过 1 << n 求出 100⋯0(n个0)100⋯0(n个0)，然后减一即可。

时间复杂度
枚举所有 statestate 的时间复杂度是 (2n)O(2n)
枚举 jj 的时间复杂读是 (n)O(n)
枚举 kk 的时间复杂度是 (n)O(n)
所以总的时间复杂度是 (n22n)O(n22n)
C++ 代码
这里给出两个版本的代码。
第一个版本比较易懂，第二个版本的跑的更快（实测）。
读第二个版本的代码需要一定的位运算知识。
但是你如果能读懂第一个版本，读第二个应该也没啥问题。

版本一：直接按照上述状态转移方法做。

#include <stdio.h>
#include <string.h>

const int N = 20;
const int M = 1 << 20; // 一共最多有 20 个 1 种状态

int n;
int w[N][N];           // 存每两个点之间的距离
int f[M][N];           // 上述 f[state][j]

int main()
{
    scanf("%d", &n);
    for (int i = 0; i < n; i ++ )
        for (int j = 0; j < n; j ++ )
            scanf("%d", &w[i][j]);
    memset(f, 0x3f, sizeof f); // 由于要求最小值，所以这里将 f 初始化为正无穷会更好处理一些
    f[1][0] = 0;               // 因为题目要求从点 0 出发，所以这里要将 经过点集为 1，当前到达第 0 个点 的最短路径初始化为 0
    for (int state = 1; state < 1 << n; state ++ )   // 从 0 到 111...11 枚举所有 state
        if (state & 1)                               // state 必须要包含起点 1
            for (int j = 0; j < n; j ++ )            // 枚举所有 state 到达的点
                if (state >> j & 1)                  // 如果当前点集包含点 j，那么进行状态转移
                    for (int k = 0; k < n; k ++ )    // 枚举所有 k
                        if (state ^ 1 << j >> k & 1) // 如果从当前状态经过点集 state 中，去掉点 j 后，state 仍然包含点 k，那么才能从点 k 转移到点 j。
                                                     // 当然这个 if 也可以不加，因为如果 state 去掉第 j 个点后，state 不包含点 k 了，
                                                     // 那么 f[state ^ 1 << j][k] 必然为正无穷，也就必然不会更新 f[state][j]，所以去掉也可以 AC。
                            if (f[state ^ 1 << j][k] + w[k][j] < f[state][j]) // 由于 >> 和 << 的优先级要比 ^ 的优先级高，所以这里可以将 state ^ (1 << j) 去掉括号。
                                f[state][j] = f[state ^ 1 << j][k] + w[k][j];
    printf("%d\n", f[(1 << n) - 1][n - 1]);          // 最后输出 f[111...11][n-1]
    return 0;
}

版本二：在两次枚举 0∼n−10∼n−1 中所有点时，只枚举点集中包含的点。
这里偷个懒，只注释和上面有改动的地方。

#include <stdio.h>
#include <string.h>

const int N = 20;
const int M = 1 << N;

int n;
int w[N][N];
int f[M][N];
int log_2[M >> 1]; // 存 1 到 M / 2 中，所有 2 的整次幂的数以 2 为底的对数

int main()
{
    scanf("%d", &n);
    for (int i = 0; i < n; i ++ )
        for (int j = 0; j < n; j ++ )
            scanf("%d", &w[i][j]);

    for (int i = 0; i < 20; i ++ ) // 预处理 log_2
        log_2[1 << i] = i;

    memset(f, 0x3f, sizeof f);
    f[1][0] = 0;
    for (int state = 1; state < 1 << n; state ++ )
        if (state & 1)
            for (int t = state; t; t &= t - 1)     // 只枚举 state 所包含的点集
            {
                int j = log_2[t & -t];             // 将集合 t 中最小的点取出
                for (int u = state ^ t & -t; u; u &= u - 1) // 只枚举 state 中去掉点 j 的点集
                {
                    int k = log_2[u & -u];         // 将集合 u 中最小的点取出
                    if (f[state ^ t & -t][k] + w[k][j] < f[state][j])
                        f[state][j] = f[state ^ t & -t][k] + w[k][j];
                }
            }

    printf("%d\n", f[(1 << n) - 1][n - 1]);
    return 0;
}

作者：垫底抽风
链接：https://www.acwing.com/solution/content/15328/

```
