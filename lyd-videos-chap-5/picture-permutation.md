# ordered 5-row permutation

```cpp
// yxc code
算法
(DP,线性DP) O(k(Nk)k)O(k(Nk)k)
核心性质：

从高到低依次安排每个同学的位置，那么在安排过程中，当前同学一定占据每排最靠左的连续若干个位置，且从后往前每排人数单调递减。否则一定不满足“每排从左到右身高递减，从后到前身高也递减”这个要求。

DP的核心思想是用集合来表示一类方案，然后从集合的维度来考虑状态之间的递推关系。

受上述性质启发，状态表示为：

f[a][b][c][d][e]代表从后往前每排人数分别为a, b, c, d, e的所有方案的集合, 其中a >= b >= c >= d >= e；
f[a][b][c][d][e]的值是该集合中元素的数量；
状态计算对应集合的划分，令最后一个同学被安排在哪一排作为划分依据，可以将f[a][b][c][d][e]划分成5个不重不漏的子集：

当a > 0 && a - 1 >= b时，最后一个同学可能被安排在第1排，方案数是f[a - 1][b][c][d][e]；
当b > 0 && b - 1 >= c时，最后一个同学可能被安排在第2排，方案数是f[a][b - 1][c][d][e]；
当c > 0 && c - 1 >= d时，最后一个同学可能被安排在第3排，方案数是f[a][b][c - 1][d][e]；
当d > 0 && d - 1 >= e时，最后一个同学可能被安排在第4排，方案数是f[a][b][c][d - 1][e]；
当e > 0时，最后一个同学可能被安排在第5排，方案数是f[a][b][c][d][e - 1]；
时间复杂度
由均值不等式，最坏情况下共有 (Nk)k(Nk)k 状态，计算每个状态需要 O(k)O(k) 的计算量，因此总时间复杂度是 O(k(Nk)k)O(k(Nk)k)。

C++ 代码
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

typedef long long LL;

const int N = 31;

int n;
LL f[N][N][N][N][N];

int main()
{
    while (cin >> n, n)
    {
        int s[5] = {0};
        for (int i = 0; i < n; i ++ ) cin >> s[i];
        memset(f, 0, sizeof f);
        f[0][0][0][0][0] = 1;

        for (int a = 0; a <= s[0]; a ++ )
            for (int b = 0; b <= min(a, s[1]); b ++ )
                for (int c = 0; c <= min(b, s[2]); c ++ )
                    for (int d = 0; d <= min(c, s[3]); d ++ )
                        for (int e = 0; e <= min(d, s[4]); e ++ )
                        {
                            LL &x = f[a][b][c][d][e];
                            if (a && a - 1 >= b) x += f[a - 1][b][c][d][e];
                            if (b && b - 1 >= c) x += f[a][b - 1][c][d][e];
                            if (c && c - 1 >= d) x += f[a][b][c - 1][d][e];
                            if (d && d - 1 >= e) x += f[a][b][c][d - 1][e];
                            if (e) x += f[a][b][c][d][e - 1];
                        }
        cout << f[s[0]][s[1]][s[2]][s[3]][s[4]] << endl;
    }

    return 0;
}


```

```cpp
 //lyd code 
 //https://github.com/lydrainbowcat/tedukuri/blob/master/%E9%85%8D%E5%A5%97%E5%85%89%E7%9B%98/%E4%BE%8B%E9%A2%98/0x50%20%E5%8A%A8%E6%80%81%E8%A7%84%E5%88%92/0x51%20%E7%BA%BF%E6%80%A7DP/Mr.%20Young's%20Picture%20Permutations/POJ2279%20Mr.%20Young's%20Picture%20Permutations.cpp
// https://www.acwing.com/activity/content/code/content/590685/
//5D array!!
#include <cstdio>
#include <cstring>
#include <iostream>
#define ll long long
using namespace std;
int n[6], k;

void work() {
    for (int i = 1; i <= k; i++) cin >> n[i];
    while (k < 5) n[++k] = 0;
    ll f[n[1]+1][n[2]+1][n[3]+1][n[4]+1][n[5]+1];
    memset(f, 0, sizeof(f));
    f[0][0][0][0][0] = 1;
    for (int i = 0; i <= n[1]; i++)
        for (int j = 0; j <= n[2]; j++)
            for (int k = 0; k <= n[3]; k++)
                for (int l = 0; l <= n[4]; l++)
                    for (int m = 0; m <= n[5]; m++) {
                        if (i < n[1]) f[i+1][j][k][l][m] += f[i][j][k][l][m];
                        if (j < n[2] && i > j) f[i][j+1][k][l][m] += f[i][j][k][l][m];
                        if (k < n[3] && j > k) f[i][j][k+1][l][m] += f[i][j][k][l][m];
                        if (l < n[4] && k > l) f[i][j][k][l+1][m] += f[i][j][k][l][m];
                        if (m < n[5] && l > m) f[i][j][k][l][m+1] += f[i][j][k][l][m];
                    }
    cout << f[n[1]][n[2]][n[3]][n[4]][n[5]] << endl;
}

int main() {
    while (cin >> k && k) work();
    return 0;
}


```
