# integer partition

```cpp
AcWing 900. 整数划分 integer partition

1
完全背包解法
状态表示：
f[i][j]表示只从1~i中选，且总和等于j的方案数

状态转移方程:
f[i][j] = f[i - 1][j] + f[i][j - i];

#include <iostream>
#include <algorithm>

using namespace std;

const int N = 1010, mod = 1e9 + 7;

int n;
int f[N];

int main()
{
    cin >> n;

    f[0] = 1;
    for (int i = 1; i <= n; i ++ )
        for (int j = i; j <= n; j ++ )
            f[j] = (f[j] + f[j - i]) % mod;

    cout << f[n] << endl;

    return 0;
}
其他算法
状态表示：
f[i][j]表示总和为i，总个数为j的方案数

状态转移方程：
f[i][j] = f[i - 1][j - 1] + f[i - j][j];

#include <iostream>
#include <algorithm>

using namespace std;

const int N = 1010, mod = 1e9 + 7;

int n;
int f[N][N];

int main()
{
    cin >> n;

    f[1][1] = 1;
    for (int i = 2; i <= n; i ++ )
        for (int j = 1; j <= i; j ++ )
            f[i][j] = (f[i - 1][j - 1] + f[i - j][j]) % mod;

    int res = 0;
    for (int i = 1; i <= n; i ++ ) res = (res + f[n][i]) % mod;

    cout << res << endl;

    return 0;
}


链接：https://www.acwing.com/activity/content/code/content/62496/
math 4 - discrete math
PIE - principle of inclusion and exclusion
venn diagram  a1+a2+a3 - a1^a2 - a2^a3 - a1^a3 + a1^a2^a3
combinatorial identity
https://baike.baidu.com/item/%E7%BB%84%E5%90%88%E6%81%92%E7%AD%89%E5%BC%8F/4175086

number of p multiples under n is floor(n/p)

game theory - nim game, sg function; directed graph game;

return a=b; first assign b to a, then return a!

https://stackoverflow.com/questions/39983769/what-does-assignment-operator-means-in-return-statements-like-return-t

https://www.acwing.com/solution/content/13191/
https://www.acwing.com/solution/content/3929/

math 3 - gauss elimination
combination numbers same as DP idea: C(a, b) = C(a-1,b-1) + C(a-1, b)

mod=1e9+7 first 10 digit number which fits in unsigned long long, also a prime
https://www.geeksforgeeks.org/modulo-1097-1000000007/#:~:text=10%5E9%2B7%20fulfills%20both,order%20to%20prevent%20possible%20overflows.
math 3 1:05:00

watch the last half of each video
watch videos in reversed order from back to front

bluebook - 0x70 on STL easy
set has only one data type; map has two, it's key-value pair.

0x14 hash - collision

STL库总结 https://www.acwing.com/file_system/file/content/whole/index/content/1475038/

逆元 https://www.cnblogs.com/judge/p/9383034.html
https://blog.csdn.net/ACdreamers/article/details/8220787
https://zhuanlan.zhihu.com/p/100587745
https://www.cnblogs.com/linyujun/p/5194184.html

data type sizes-
https://www.acwing.com/file_system/file/content/whole/index/content/1475865/

lucas theorem
cartalan numbers

c++ format string specifier, just use c++ reference, too much details not relevant
sprintf() , fprintf() scanf/sscanf/fscanf
http://www.cplusplus.com/reference/cstdio/printf/

https://alexandra-yang.gitbook.io/notes/

python manim.py p/old/PeriodicTable/PeriodicTable.py
/Users/rsdata/manim

My websites:
https://sites.google.com/site/mathcounts2020
https://sites.google.com/view/procoding
https://sites.google.com/view/theknowledge-lewis
https://sites.google.com/iu.edu/programming

nlp stock
https://github.com/yiaktan/NLP-Stock-Prediction.git

jupyter can save as python .py
stopped at which takes quite some CPU
python Text\ PreProcessing.py

https://haiyang1.gitbook.io/hy/program
https://haiyang123.gitbook.io/hy/
https://app.gitbook.com/@haiyang1/s/hy/program

```
