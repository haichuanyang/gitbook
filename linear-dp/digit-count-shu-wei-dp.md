---
description: find count of ways to get numbers within [x,y] satisfying condition
---

# digit count 数位 DP from advanced

[https://www.acwing.com/video/420/](https://www.acwing.com/video/420/) first 20 minutes has general method; first problem 1801finished; next [1802](https://www.acwing.com/file\_system/file/content/whole/index/content/1487725/) not done

prefix sum idea: f\[x,y]=f\[y]-f\[x-1]

tree idea:&#x20;

![](<../.gitbook/assets/Screen Shot 2020-11-26 at 8.09.47 AM.png>)

N-digit: a\[n-1], a\[n-2],a\[n-3]...a\[0]

a\[n-1] casework, highest bit choice has 2 cases: a) digit a\[n-1] itself b) digit from 0 to (a\[n-1]-1)

for b) case: k 1 filled, count of ways for filling the remaining digits, C\[n-1,k], C\[n-1,k-1]

a) need to divide further

AcWing 1081. 度的数量

###

###

* 计数问题

```cpp

//yxc basic class - 
#include <iostream>
#include <algorithm>
#include <vector>

using namespace std;

const int N = 10;

/*

001~abc-1, 999

abc
    1. num[i] < x, 0
    2. num[i] == x, 0~efg
    3. num[i] > x, 0~999

*/

int get(vector<int> num, int l, int r)
{
    int res = 0;
    for (int i = l; i >= r; i -- ) res = res * 10 + num[i];
    return res;
}

int power10(int x)
{
    int res = 1;
    while (x -- ) res *= 10;
    return res;
}

int count(int n, int x)
{
    if (!n) return 0;

    vector<int> num;
    while (n)
    {
        num.push_back(n % 10);
        n /= 10;
    }
    n = num.size();

    int res = 0;
    for (int i = n - 1 - !x; i >= 0; i -- )
    {
        if (i < n - 1)
        {
            res += get(num, n - 1, i + 1) * power10(i);
            if (!x) res -= power10(i);
        }

        if (num[i] == x) res += get(num, i - 1, 0) + 1;
        else if (num[i] > x) res += power10(i);
    }

    return res;
}

int main()
{
    int a, b;
    while (cin >> a >> b , a)
    {
        if (a > b) swap(a, b);

        for (int i = 0; i <= 9; i ++ )
            cout << count(b, i) - count(a - 1, i) << ' ';
        cout << endl;
    }

    return 0;
}


```

```cpp
//yxc advanced class DP first problem 1081
#include <cstring>
#include <iostream>
#include <algorithm>
#include <vector>

using namespace std;

const int N = 35;

int K, B;
int f[N][N];

void init()
{
    for (int i = 0; i < N; i ++ )
        for (int j = 0; j <= i; j ++ )
            if (!j) f[i][j] = 1;
            else f[i][j] = f[i - 1][j] + f[i - 1][j - 1];
}

int dp(int n)
{
    if (!n) return 0;

    vector<int> nums;
    while (n) nums.push_back(n % B), n /= B;

    int res = 0;
    int last = 0;
    for (int i = nums.size() - 1; i >= 0; i -- )
    {
        int x = nums[i];
        if (x)  // 求左边分支中的数的个数
        {
            res += f[i][K - last];
            if (x > 1)
            {
                if (K - last - 1 >= 0) res += f[i][K - last - 1];
                break;
            }
            else
            {
                last ++ ;
                if (last > K) break;
            }
        }

        if (!i && last == K) res ++ ;   // 最右侧分支上的方案
    }

    return res;
}

int main()
{
    init();

    int l, r;
    cin >> l >> r >> K >> B;

    cout << dp(r) - dp(l - 1) << endl;

    return 0;
}


```



```cpp
【提高课】数位DP总结
Arroganceの浓的头像Arroganceの浓

个人总结，可能不全面，蒟蒻见解，如有错误，请及时指出

一 使用
求满足一个性质的所有数时使用。

二 思路
把整个数拆成每一位，从最高位向最低位看。
设当前这一位为x，则当前位分成[0,x)和x两种情况

PS：为什么这里分成这一位为 x 和 <x 两种情况？
因为 <x 时后面可以任取，通常可以通过DP直接求出，这类情况就全部解决了。
为 x 时，后面的数有范围限制，不能任取。
第一种情况直接处理，剩下的就是当前位为x的情况，则继续看后面的数位，重复这个操作。
直到最后一位时，剩下的就是原数，判断是否符合条件即可。

中间由于性质可能需要记录last，一般表示为之前的最高位的某些性质。

三 模板
void init() // 根据题意做预处理。
{
    for(int i = 0; i <= 9; i ++) // 对第一位初始化
        f[1][i] = 1;

    // DP过程
}

int dp(int n)
{
    if(!n) return 1;
    vector<int> num;
    // 取出每一位数字，可以根据进制转化问题替换 10
    while(n) num.push_back(n % 10), n /= 10; 
    n = num.size();

    LL ans = 0;
    int last = 0;
    for(int i = n - 1; i >= 0; i --)
    {
        int x = num[i];

        // 分类讨论
    }

    return ans;
}
预处理通常为DP或组合数。
在分类讨论计算答案时，和预处理的DP过程类似。

四 trick（技巧）
再求区间[a,b]中满足性质的数时，可以转化为求[1,b]的答案减[1,a - 1]的答案。

前导0会产生影响时，先求出最高位不为0的所有情况，最后处理最高位为0的情况。
eg：339. 圆形数字、1083. Windy数

在分类讨论时，有点类似倒着DP，和预处理过程相反，两者状态转移方程相似。
eg:1086. 恨7不成妻

五 练习
1. 339. 圆形数字
主要问题在于前导零的处理

#include<iostream>
#include<vector>

using namespace std;

typedef long long LL;

const int N = 31;

int f[N][N]; // 一共有i位，取了j位0的方案数,当前位是k

void init()
{
    for(int i = 0; i < N; i ++)
        for(int j = 0; j <= i; j ++)
            if(j) f[i][j] = f[i - 1][j] + f[i - 1][j - 1];
            else f[i][j] = 1;
}

int dp(int n)
{
    if(!n) return 0;

    vector<int> num;
    while(n) num.push_back(n & 1), n >>= 1;
    n = num.size();
    int cnt = (n + 1) / 2;

    int ans = 0, last = 0; // 0 的个数
    for(int i = n - 2; i >= 0; i --)
    {
        int x = num[i];
        if(x == 1)
        {
            for(int j = max(cnt - last - 1, 0); j <= i; j ++)
                ans += f[i][j];
        }
        else last ++;
        if(!i && last >= cnt) ans ++;
    }

    for(int i = 1; i < n; i ++)
    {
        cnt = (i + 1) / 2;
        for(int j = cnt; j < i; j ++)
            ans += f[i - 1][j];
    }

    return ans;
}

int main()
{
    init();
    int a, b;
    cin >> a >> b;
    cout << dp(b) - dp(a - 1) << endl;

    return 0;
}
2. D. Checkpoints
2020年CCPC长春站的第二道签到题，可惜我和队友写了三小时。
有兴趣可以做做。
```
