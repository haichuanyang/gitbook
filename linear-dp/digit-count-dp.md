# digit count DP





![计数问题1.png](https://cdn.acwing.com/media/article/image/2020/11/25/54324\_f127e5282e-%E8%AE%A1%E6%95%B0%E9%97%AE%E9%A2%981.png)

![计数问题2.png](https://cdn.acwing.com/media/article/image/2020/11/25/54324\_f9323b602e-%E8%AE%A1%E6%95%B0%E9%97%AE%E9%A2%982.png)

{% embed url="https://www.acwing.com/file_system/file/content/whole/index/content/1482577/" %}

```
#include <cmath>
#include <cstring>
#include <vector>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 10;

//f[i][j]表示在i位数中包含前导0的所有数字组合中，j的个数
//g[i]表示在i位数中不包含前导0的所有数字组合中，0的个数
int f[N][N], g[N];
int w[2][N], st;

void init()
{
    for(int i = 1; i < N; ++i)
        for(int j = 0; j < N; ++j)
            f[i][j] = pow(10, i - 1) + 10 * f[i - 1][j];

    g[1] = 1;
    for(int i = 2; i < N; ++i)
    {
        g[i] = 9 * f[i - 1][0] + g[i - 1];
    }
}

void dp(int u)
{
    if(u == 0)
    {
        w[st][0] = 1;
        return;
    }

    int v = u;
    vector<int> nums;
    while(v)
    {
        nums.push_back(v % 10);
        v /= 10;
    }

    //对最高位取0的情况下计算0个数做特殊处理
    w[st][0] += g[nums.size() - 1];
    for(int i = nums.size() - 1; i >= 0; --i)
    {
        int x = nums[i];
        for(int j = 0; j < x; ++j)
        {
            //在最高位取0的情况不对0个数进行计算，因为上面已经先预处理了
            //这一层循环是可以去掉的，因为j只决定了整个循环的循环次数，而不参与整个循环的计算
            for(int k = (i == nums.size() - 1 && j == 0); k < N; ++k)
            {
                w[st][k] += f[i][k];
            }
            //当前最高位（i+1）那个数字的次数
            if(i != nums.size() - 1 || j != 0) 
                w[st][j] += pow(10, i);
        }
        //当前最高位选取最大的那个数的次数
        if(i > 0) w[st][x] += u % (int)pow(10, i) + 1;  //+1是因为包含为0的情况（1999，后三位所包含的情况有0~999共一千种情况）
        else w[st][x] += 1;     //最后一位数的情况
    }
}

int main()
{
    init();
    int a, b;
    while(cin >> a >> b && (a != 0 || b != 0))
    {
        int sum = a + b;
        a = min(a, b);
        b = sum - a;
        dp(b);
        st = (st + 1) % 2;
        dp(a - 1);
        st = (st + 1) % 2;

        for(int i = 0; i < N; ++i)
            cout << w[0][i] - w[1][i] << ' ';
        cout << '\n';

        memset(w, 0, sizeof(w));
    }

    return 0;
}

作者：Chase.
链接：https://www.acwing.com/file_system/file/content/whole/index/content/1482577/

```
