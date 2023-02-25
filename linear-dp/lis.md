# LIS

```cpp
最长上升子序列解题报告
给定一个长度为N的数列(w[N])，求数值严格单调递增的子序列的长度最长是多少。

样例
输入格式
第一行包含整数N。

第二行包含N个整数，表示完整序列。

输出格式
输出一个整数，表示最大长度。

数据范围
1 ≤ N ≤ 1000，
−1e9 ≤ 数列中的数 ≤ 1e9

输入样例：

7
3 1 2 1 8 5 6
输出样例：

4
算法1
(动态规划) O(n2)O(n2)
状态表示：f[i]表示从第一个数字开始算，以w[i]结尾的最大的上升序列。(以w[i]结尾的所有上升序列中属性为最大值的那一个)

状态计算（集合划分）：j∈(0,1,2,..,i-1), 在w[i] > w[j]时，
f[i] = max(f[i], f[j] + 1)。
有一个边界，若前面没有比i小的，f[i]为1（自己为结尾）。

最后在找f[i]的最大值。

时间复杂度
O(n2)O(n2) 状态数(nn) * 转移数(nn)
C++ 代码
#include <iostream>

using namespace std;

const int N = 1010;

int n;
int w[N], f[N];

int main() {
    cin >> n;
    for (int i = 0; i < n; i++) cin >> w[i];

    int mx = 1;    // 找出所计算的f[i]之中的最大值，边算边找
    for (int i = 0; i < n; i++) {
        f[i] = 1;    // 设f[i]默认为1，找不到前面数字小于自己的时候就为1
        for (int j = 0; j < i; j++) {
            if (w[i] > w[j]) f[i] = max(f[i], f[j] + 1);    // 前一个小于自己的数结尾的最大上升子序列加上自己，即+1
        }
        mx = max(mx, f[i]);
    }

    cout << mx << endl;
    return 0;
}
算法2
(动态规划 + 二分) O(nlogn)O(nlogn)
状态表示：f[i]表示长度为i的最长上升子序列，末尾最小的数字。(长度为i的最长上升子序列所有结尾中，结尾最小min的) 即长度为i的子序列末尾最小元素是什么。

状态计算：对于每一个w[i], 如果大于f[cnt-1](下标从0开始，cnt长度的最长上升子序列，末尾最小的数字)，那就cnt+1，使得最长上升序列长度+1，当前末尾最小元素为w[i]。 若w[i]小于等于f[cnt-1],说明不会更新当前的长度，但之前末尾的最小元素要发生变化，找到第一个 大于或等于 (这里不能是大于) w[i]，更新以那时候末尾的最小元素。

f[i]一定以一个单调递增的数组，所以可以用二分法来找第一个大于或等于w[i]的数字。

时间复杂度
O(nlogn)O(nlogn) 状态数(nn) * 转移数(lognlogn)
C++ 代码
#include <iostream>

using namespace std;

const int N = 1010;
int n, cnt;
int w[N], f[N];

int main() {
    cin >> n;
    for (int i = 0 ; i < n; i++) cin >> w[i];

    f[cnt++] = w[0];
    for (int i = 1; i < n; i++) {
        if (w[i] > f[cnt-1]) f[cnt++] = w[i];
        else {
            int l = 0, r = cnt - 1;
            while (l < r) {
                int mid = (l + r) >> 1;
                if (f[mid] >= w[i]) r = mid;
                else l = mid + 1;
            }
            f[r] = w[i];
        }
    }
    cout << cnt << endl;
    return 0;
}

作者：VictorWu
链接：https://www.acwing.com/solution/content/4807/

```
