# longest common increasing sequence

```cpp
//yxc code
(DP,线性DP,前缀和) O(n2)O(n2)
这道题目是AcWing 895. 最长上升子序列和AcWing 897. 最长公共子序列的结合版，在状态表示和状态计算上都是融合了这两道题目的方法。

状态表示：

f[i][j]代表所有a[1 ~ i]和b[1 ~ j]中以b[j]结尾的公共上升子序列的集合；
f[i][j]的值等于该集合的子序列中长度的最大值；
状态计算（对应集合划分）：

首先依据公共子序列中是否包含a[i]，将f[i][j]所代表的集合划分成两个不重不漏的子集：

不包含a[i]的子集，最大值是f[i - 1][j]；
包含a[i]的子集，将这个子集继续划分，依据是子序列的倒数第二个元素在b[]中是哪个数：
子序列只包含b[j]一个数，长度是1；
子序列的倒数第二个数是b[1]的集合，最大长度是f[i - 1][1] + 1；
…
子序列的倒数第二个数是b[j - 1]的集合，最大长度是f[i - 1][j - 1] + 1；
如果直接按上述思路实现，需要三重循环：

for (int i = 1; i <= n; i ++ )
{
    for (int j = 1; j <= n; j ++ )
    {
        f[i][j] = f[i - 1][j];
        if (a[i] == b[j])
        {
            int maxv = 1;
            for (int k = 1; k < j; k ++ )
                if (a[i] > b[k])
                    maxv = max(maxv, f[i - 1][k] + 1);
            f[i][j] = max(f[i][j], maxv);
        }
    }
}
然后我们发现每次循环求得的maxv是满足a[i] > b[k]的f[i - 1][k] + 1的前缀最大值。
因此可以直接将maxv提到第一层循环外面，减少重复计算，此时只剩下两重循环。

最终答案枚举子序列结尾取最大值即可。

时间复杂度
代码中一共两重循环，因此时间复杂度是 O(n2)O(n2)。

C++ 代码
#include <cstdio>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 3010;

int n;
int a[N], b[N];
int f[N][N];

int main()
{
    scanf("%d", &n);
    for (int i = 1; i <= n; i ++ ) scanf("%d", &a[i]);
    for (int i = 1; i <= n; i ++ ) scanf("%d", &b[i]);

    for (int i = 1; i <= n; i ++ )
    {
        int maxv = 1;
        for (int j = 1; j <= n; j ++ )
        {
            f[i][j] = f[i - 1][j];
            if (a[i] == b[j]) f[i][j] = max(f[i][j], maxv);
            if (a[i] > b[j]) maxv = max(maxv, f[i - 1][j] + 1);
        }
    }

    int res = 0;
    for (int i = 1; i <= n; i ++ ) res = max(res, f[n][i]);
    printf("%d\n", res);

    return 0;
}

作者：yxc
链接：https://www.acwing.com/solution/content/4955/

```

```cpp
//lyd code
笔记

LIS：最长上升子序列
f[i]表示以a[i]为结尾的最长上升子序列的长度

LCS：最长公共子序列
f[i,j]表示a[1~i]和b[1~j]的最长公共子序列的长度
f[i,j]=max(f[i-1][j], f[i][j-1])
if (a[i]==b[j]) f[i,j]=max(f[i][j], f[i-1][j-1]+1);

for (int i = 1; i <= n; i++) f[i][0]=0;
b[0] = -(1<<30);
for (int i = 1; i <= n; i++)
    for (int j = 1; j <= m; j++) {
        f[i][j] = f[i-1][j];
        if (a[i]==b[j]) {
            for (int k = 0; k < j; k++)
                if (b[k] < a[i]) // b[j] == a[i]
                    f[i][j] = max(f[i][j], f[i-1][k]+1);
        }
    }
int ans = 0;
for (int j = 1; j <= m; j++)
    ans = max(ans, f[n][j]);

决策候选集合S——用一个int维护
j,k的关系
j变化时

j=1 k=0~0 b[k]<a[i]
j=2 k=0~1 b[k]<a[i]
j=3 k=0~2 b[k]<a[i]
j=4 k=0~3 b[k]<a[i]
j增大1时至多有一个新的决策（就是j-1）进入候选集合，
并且之前的决策合法性不变

a[i] = 6
b = {0, 3, 2, 7, 5}

S={}
j=1 k={0}  新决策0即j-1进来了 S={0}
j=2 k={0,1} 新决策1即j-1进来了  S=S 并集 {1}={0,1}
j=3 k={0,1,2}
j=4 k={0,1,2}
j=5 k={0,1,2,4}  新决策4即j-1进来了

需要维护一个集合S
支持把一个新的决策j-1插入集合S
求集合S中下标对应的“f[i-1][k]+1”值的max

int S;
b[j-1]<a[i]时 S=max(S, f[i-1][j-1]+1); // j-1就是新决策k


for (int i = 1; i <= n; i++) {
    int S = 0;
    for (int j = 1; j <= m; j++) {
        f[i][j] = f[i-1][j];
        // int k = j - 1; // 最优决策
        if (b[j-1] < a[i]) {
            S = max(S, f[i-1][j-1] + 1);
        }
        if (a[i]==b[j]) {
            f[i][j] = max(f[i][j], S);
        }
    }
}
标程

#include<iostream>
#include<cstring>
#include<algorithm>
using namespace std;
const int N = 3010;
int a[N],b[N];
int f[N][N]; 
int n;

int main()
{
    cin >> n;
    for(int i = 1;i <= n;i ++) cin >> a[i];
    for(int i = 1;i <= n;i ++) cin >> b[i];
    b[0] = -(1 << 30);

    for(int i = 1;i <= n;i ++)
    {
        int maxs = 0; 
        for(int j = 1;j <= n;j ++)
        {
            f[i][j] = f[i - 1][j];
            if(b[j - 1] < a[i])
                maxs = max(maxs,f[i - 1][j - 1] + 1);
            if(a[i] == b[j])
                f[i][j] = max(f[i][j] ,maxs);
        }
    }

    int res = 0;
    for(int i = 1;i <= n;i ++)
        res = max(res,f[n][i]);
    cout << res << endl;
    return 0;
}


```
