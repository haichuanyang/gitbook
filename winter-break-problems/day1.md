---
description: median is solution;
---

# day1 absolute value inequality/median

```
#include <algorithm>

using namespace std;

const int N = 100005;

int n, res;
int a[N];

int main()
{
    scanf("%d", &n);
    for (int i = 0; i < n; i ++ ) scanf("%d", &a[i]);

    sort(a, a + n); // sort O(lg n); 
    // non-sort nth_element(a, a+n/2, a+n); O(n)

    for (int i = 0; i < n; i ++ ) res += abs(a[i] - a[n >> 1]); 
    //a[i]-a[i/2] also works no abs()
    printf("%d\n", res);

    return 0;
}
该代码第 171行中，将 abs(a[i] - a[n >> 1]) 改为 abs(a[i] - a[i >> 1]) 也可以 AC

作者：垫底抽风
链接：https://www.acwing.com/solution/content/23136/

```

### nth\_element() can be used to get the median in O(n) time:

```
// C++ program to find the median of the vector 
#include <iostream> 
#include <algorithm> 
#include <vector> 
using namespace std; 
int main() 
{ 
    vector<int> v = { 3, 2, 10, 45, 33, 56, 23, 47, 60 }, i; 
  
    // Using std::nth_element with n as v.size()/2 + 1 
    std::nth_element(v.begin(), v.begin() + v.size() / 2, v.end()); 
  
    cout << "The median of the array is " << v[v.size() / 2]; 
  
    return 0; 
} 
```

![f.jpg](https://cdn.acwing.com/media/article/image/2020/10/23/30334\_87cef6d415-f.jpg)

2D  problem 3167, convex function 3-split

```
三分套三分
求z=x2+y2z=x2+y2的最小值，根据多元函数求偏导，二阶导数大于0，凹函数，存在最小值，有多元函数微分学味了。
固定x，三分求y，一般这样求多个变量的，一般可以先固定一个！

一行语句实现浮点数的四舍五入printf("%d",(int)(check(l) + 0.5)); // 浮点数四舍五入，学到了！
参考资料
时间复杂度O(logn∗logn∗n)O(logn∗logn∗n)
#include <iostream>
#include <cmath>

using namespace std;

const int N = 110;

#define eps 1e-4

int n;
int x[N],y[N];

double dis(double x1,double y1)
{
    double sum = 0.0;
    for(int i =0;i < n;i ++ )
        sum += sqrt((x[i] - x1)*(x[i] - x1) + (y[i] - y1)*(y[i] - y1));
    return sum;
}

double check(double x1) // 三分y，x和y都是凹函数，结构一样
{
    double l = 0.0,r = 10000.0;
    while(r - l > eps)
    {
        double mid = (l + r) / 2;
        double mmid = (mid + r) / 2;
        if(dis(x1, mid) > dis(x1, mmid)) l = mid;
        else r = mmid; // 这里是r = mmid;
    }
    return dis(x1,l);
}

int main()
{
    cin >> n;
    for(int i = 0;i < n;i ++ ) cin >> x[i] >> y[i];

    double l = 0.0,r = 10000.0;
    while(r - l > eps)  // 三分x
    {
        double mid = (l + r) / 2;
        double mmid = (mid + r) / 2;
        if(check(mid) > check(mmid)) l = mid;
        else r = mmid; // 这里是r = mmid;
    }

    printf("%d",(int)(check(l) + 0.5)); // 浮点数四舍五入，学到了！

    return 0;
}

作者：繁花似锦
链接：https://www.acwing.com/solution/content/29849/

```

10D - simulated annealing 模拟退火算法

problem 786 k-th element



plain C quicksort:



```
C 语言版
#include<stdio.h>
#define N 100010

int n, ans, temp, nums[N];

void quick_sort(int l, int r)
{
    if(l >= r) return;
    int key = nums[l + r >> 1], i = l - 1, j = r + 1;
    while(i < j)
    {
        while(nums[++i] < key);
        while(nums[--j] > key);
        if(i < j) temp = nums[i], nums[i] = nums[j], nums[j] = temp;
    }
    quick_sort(l, j);
    quick_sort(j + 1, r);

}

int main()
{
    scanf("%d", &n);
    for(int i = 0; i < n; i++) scanf("%d", &nums[i]);
    quick_sort(0, n - 1);
    for(int i = 0; i < n / 2; i ++) ans += (nums[n - i - 1] - nums[i]);
    printf("%d", ans);
}

作者：喝水大王
链接：https://www.acwing.com/solution/content/9863/

```

