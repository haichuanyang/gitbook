---
description: simulation, iterate, enumerate
---

# day 10 iteration 递推



* coin flipping 2 at a time

n coins fliping two at a time, find number ways from start state to end state

n coins allows n-1 operations, every operation only 0 or 1 times( more than 2 is repeating), order doesn't matter;&#x20;

consider first coin - if it's different, then must do first operation; after first coin, consider second coin and same logic applies.

whole sequence of operation is fixed,  so no need to search/sort; looks like DFS but it's recursion

expansion: p1208 recursion o(n) p95 2D enumerate first line state o(n\*2^n); p208 ultimate switch problem - Gauss elimination o(n^6)

switch problem has generally 2 methods - iterate/enumerate; gauss elimination

```
如果通过每次翻转两枚相邻硬币，能从状态一变为状态二，则两个状态之间必定有偶数个不同状态的硬币。

模拟法：

从最左侧开始遍历，如果该位置硬币状态与目标不同，就翻动该位置和该位置后面的两枚硬币。

因为题目说了有解，所以遍历到倒数第二枚的时候，所有硬币状态就与目标相同了。

这个方法也有点贪心的思路，每次追求当前位置状态与目标状态一致。我还是喜欢称它为模拟法。

//cpp
#include <iostream>
#include <string>
using namespace std;
const int N = 110;//用不到，看范围就直接写了

int main()
{
    string s1, s2;//s1:初始状态，s2:目标状态
    int cnt = 0;//记录翻转次数
    cin >> s1 >> s2;
    for (int i = 0; i < s1.size() - 1; i++)
    {
        if (s1[i] != s2[i])//第 i 个位置上状态不同，就翻转该位置和后一个位置硬币
        {
            cnt++;//翻转一次硬币
            if (s1[i] == '*') s1[i] = 'o';//翻转 i 位置上的硬币
            else s1[i] = '*';
            if (s1[i+1] == 'o') s1[i+1] = '*';//翻转 i + 1 位置上的硬币
            else s1[i+1] = 'o';
        }
    }
    cout << cnt;//输出翻转次数
}
时间复杂度：遍历了一边输出，时间复杂度是 O(n)。
该思路时间复杂度已是最优，要使起始状态变为目标状态，至少遍历一边来进行判断，时间复杂度最少是 O(n)。
空间复杂度：没有开辟与输入输出有关的空间，空间复杂度是O(1)。

作者：猿六
链接：https://www.acwing.com/solution/content/30255/

```
