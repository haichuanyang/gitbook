# monoqueue (Monotonic queue)

### monotonic queue monoqueue

```
//monoqueue
//model - find max/min within sliding window

	int hh = 0, tt = -1; //head and tail of queue
	for (int i = 0; i < n; i ++ )
	{
		while (hh <= tt && check_out(q[hh])) hh ++ ;  
		// 判断队头是否滑出窗口 
		//if == while since i increment 1 at a time
		// <= has higher precedence than &&
		while (hh <= tt && check(q[tt], i)) tt -- ;
		q[ ++ tt] = i;
	}
```

the queue is a double-ended queue here, with left and right

Problem: Given N integers (possible negative) find a subsequence of length <=M with largest sum. N and M <=3\*10^5.

blue book page 69, prefix sum

```
// monoqueue - used for looking for max/min within a 
//sliding window

int l = 1, r = 1;
q[1] = 0; // save choice j=0
for(int i = 1; i <= n; i++)
{
	while (l <= r && q[l] < i - m) l++; //step 1
	ans = max(ans, sum[i] - sum[q[l]]); //step 2
	while (l <= r && sum[q[r]] >= sum[i]) r--; //step 3
	q[++r] = i; //step 3

```

segment sum = S\[R]-S\[L-1]

need prefix sum S\[i] for right and S\[j] for left, for each i, find a j which makes S\[j] the smallest, this gives the largest segment sum.

step 1: check if queue left/front is more than m distance away from i, if so pop it out of queue;

step 2:  then the left/front of queue is the best choice j for the current i

step 3: keep deleting the right/end of queue until its sum is less than S\[i], and enqueue the i as a new element to the queue&#x20;

[https://labuladong.gitbook.io/algo-en/ii.-data-structure/monotonic\_queue](https://labuladong.gitbook.io/algo-en/ii.-data-structure/monotonic\_queue)







[https://leetcode.com/problems/shortest-subarray-with-sum-at-least-k/discuss/204290/Monotonic-Queue-Summary](https://leetcode.com/problems/shortest-subarray-with-sum-at-least-k/discuss/204290/Monotonic-Queue-Summary) python

[https://1e9.medium.com/monotonic-queue-notes-980a019d5793](https://1e9.medium.com/monotonic-queue-notes-980a019d5793) java

[https://medium.com/algorithms-and-leetcode/monotonic-queue-explained-with-leetcode-problems-7db7c530c1d6](https://medium.com/algorithms-and-leetcode/monotonic-queue-explained-with-leetcode-problems-7db7c530c1d6) python

```
5. 单调栈
	常见模型：找出每个数左边离它最近的比它大/小的数
	int tt = 0;
	for (int i = 1; i <= n; i ++ )
	{
		while (tt && check(q[tt], i)) tt -- ;
		stk[ ++ tt] = i;
	}
```

```
思路：

最小值和最大值分开来做，两个for循环完全类似，都做以下四步：

解决队首已经出窗口的问题;
解决队尾与当前元素a[i]不满足单调性的问题;
将当前元素下标加入队尾;
如果满足条件则输出结果;

需要注意的细节：

上面四个步骤中一定要先3后4，因为有可能输出的正是新加入的那个元素;
队列中存的是原数组的下标，取值时要再套一层，a[q[]];

算最大值前注意将hh和tt重置;

此题用cout会超时，只能用printf;

hh从0开始，数组下标也要从0开始。

# include <iostream>
using namespace std;
const int N = 1000010;
int a[N], q[N], hh, tt = -1;

int main()
{
    int n, k;
    cin >> n >> k;
    for (int i = 0; i < n; ++ i)
    {
        scanf("%d", &a[i]);
        if (hh<=tt && i - k + 1 > q[hh]) ++ hh;                  // 若队首出窗口，hh加1
        while (hh <= tt && a[i] <= a[q[tt]]) -- tt;    // 若队尾不单调，tt减1
        q[++ tt] = i;                                  // 下标加到队尾
        if (i + 1 >= k) printf("%d ", a[q[hh]]); 
        // i need to be greater than sliding window
        // size to allow enough elements in the 
        // sliding window
    }
    cout << endl;
    hh = 0; tt = -1;                                   // 重置！
    for (int i = 0; i < n; ++ i)
    {
        if (hh<=tt && i - k + 1 > q[hh]) ++ hh;
        while (hh <= tt && a[i] >= a[q[tt]]) -- tt;
        q[++ tt] = i;
        if (i + 1 >= k) printf("%d ", a[q[hh]]);
    }
    return 0;
}
```

这道题目,我们就维护两个队列,一个是最小值,一个是最大值.这里唯一的重点就是,每一次入队的时候,不需要管是不是比队头小,因为也许他现在小,但是在队头出队列后,他还在,而且是最小的值. 来自yxc老师 这道题目的时间限制卡得比较紧，需要用 O(n)O(n) 时间复杂度的算法来做。

这是一道单调队列的模板题，以求最小值为例：

我们从左到右扫描整个序列，用一个队列来维护最近 kk 个元素； 如果用暴力来做，就是每次都遍历一遍队列中的所有元素，找出最小值即可，但这样时间复杂度就变成 O(nk)O(nk) 了； 然后我们可以发现一个性质： 如果队列中存在两个元素，满足 a\[i] >= a\[j] 且 i < j，那么无论在什么时候我们都不会取 a\[i] 作为最小值了，所以可以直接将 a\[i] 删掉； 此时队列中剩下的元素严格单调递增，所以队头就是整个队列中的最小值，可以用 O(1)O(1) 的时间找到； 为了维护队列的这个性质，我们在往队尾插入元素之前，先将队尾大于等于当前数的元素全部弹出即可； 这样所有数均只进队一次，出队一次，所以时间复杂度是 O(n)O(n) 的。

comparison operators have higher precedence than logical operators

< > &&

[https://en.cppreference.com/w/cpp/language/operator\_precedence](https://en.cppreference.com/w/cpp/language/operator\_precedence)

