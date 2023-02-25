# day11 hashtable or double pointer

```
AcWing 1532. 找硬币--双指针和哈希两种解法：思路 + 详尽代码注释   

两数之和的变种。

方法一：双指针
排序后使用双指针。
左指 l 针指向数组头，右指针 r 指向数组尾。
如果 a[l] + a[r] > m , 则 r 左移。
如果 a[l] + a[r] < m , 则 l 右移。
如果 a[l] + a[r] = m , 则找到解。
如果指针相遇还没有出现 a[l] + a[r] = m ,则无解。

时间复杂度：排序 O(nlogn)，双指针O(n),总时间复杂度 O(nlogn)。

//cpp
#include <iostream>
#include <algorithm>
using namespace std;

const int N = 100010;
int a[N];

int main()
{
    int n, m;
    int v1 = -1, v2 = -1;
    cin >> n >> m;
    for (int i = 0; i < n; i++)
    {
        cin >> a[i];
    }
    sort(a, a + n);//双指针需要先排序
    int l = 0, r = n - 1;//l 左指针，r 右指针
    while (l < r)//指针未相遇就一直执行
    {
        if (a[l] + a[r] == m)//找到答案
        {
            v1 = a[l];
            v2 = a[r];
            break;
        }
        else if (a[l] + a[r] > m)//两数之和大于 m, 则右指针左移
            r--;
        else//两数之和小于 m, 则做指针右移
            l++;
    }
    if (v1 >= 1)//v1 大于 1 说明找到了解
        cout << v1 << " " << v2;
    else
        cout << "No Solution";
}
方法二：
使用集合、
用一个哈希集合存储硬币。
对于每一枚硬币 x，判断在集合中是否存在 y，使得 x + y = m。
如果存在，则是一组解，判断该解的小面值硬币是否小于 v1， 如果是，则存入 v1, v2。

如果存在解，则输出。否则无解。

时间复杂度：一次循环，所以为 O(n) 。空间复杂度:开辟了一个堆，所以为 O(n)。

//cpp，参考yxc
#include <iostream>
#include <algorithm>
#include <unordered_set>
using namespace std;

int main()
{
    int n, m;
    int v1 = 1e5, v2 = -1;//v1 设置一个较大的值
    cin >> n >> m;
    unordered_set<int> S;//存储数据的堆
    while (n--)
    {
        int x;
        cin >> x;
        int y = m - x;//x + y = m,对于输入的 x,在堆中查看有没有 y。 
        if (S.count(y))//如果有，则 x + y = m，是一组解
        {
            S.insert(x);
            if (x > y)//判断该解的小面值是否小于 v1，如果是，则把该解存入 v1, v2.
                swap(x, y);
            if (x < v1)
            {
                v1 = x;
                v2 = y;
            }
        }
        else
            S.insert(x);
    }
    if (v1 < 1e5)//如果存在解
        cout << v1 << " " << v2;
    else
        cout << "No Solution";
}

关于双指针，可以看下这篇总结
https://mp.weixin.qq.com/s/4-4-9GoVqQ8sjguMzcZkcg

作者：猿六
链接：https://www.acwing.com/solution/content/30456/

```
