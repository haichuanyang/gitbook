# sliding window w/ queue

```
给定一个大小为n≤106的数组。

有一个大小为k的滑动窗口，它从数组的最左边移动到最右边。

您只能在窗口中看到k个数字。

每次滑动窗口向右移动一个位置。

以下是一个例子：

该数组为[1 3 -1 -3 5 3 6 7]，k为3。

窗口位置	最小值	最大值
[1 3 -1] -3 5 3 6 7	-1	3
1 [3 -1 -3] 5 3 6 7	-3	3
1 3 [-1 -3 5] 3 6 7	-3	5
1 3 -1 [-3 5 3] 6 7	-3	5
1 3 -1 -3 [5 3 6] 7	3	6
1 3 -1 -3 5 [3 6 7]	3	7
您的任务是确定滑动窗口位于每个位置时，窗口中的最大值和最小值。

输入格式

输入包含两行。

第一行包含两个整数n和k，分别代表数组长度和滑动窗口的长度。

第二行有n个整数，代表数组的具体数值。

同行数据之间用空格隔开。

输出格式

输出包含两个。

第一行输出，从左至右，每个位置滑动窗口中的最小值。

第二行输出，从左至右，每个位置滑动窗口中的最大值。

输入样例：

8 3
1 3 -1 -3 5 3 6 7
输出样例：

-1 -3 -3 -3 3 3
3 3 5 5 6 7
代码

#include <iostream>
#include <cstdio>

using namespace std;

const int N = 1e6 + 10;

int a[N], q[N]; // 数组a存放值，队列q存放下标

int main()
{
    int n, k;
    scanf("%d%d", &n, &k);
    for (int i = 0; i < n; i ++ ) cin >> a[i];

    //查找最小值
    int hh = 0, tt = -1;
    for (int i = 0; i < n; i ++ )
    {
        //判断队头是否滑出窗口
        if (tt >= hh && q[hh] < i - k + 1) hh ++ ; // 队列不为空
        // 窗口长度为k，右端下标为i，左端下标(队头)应该大于等于 i - k + 1
        // 用if不用while是因为每次窗口只会滑动一格，每次最多只有一位不在窗口内

        // 【队列内的数据从队头到队尾是单调递增的】要剔除较大数需要从队尾出队
        // 应该从队尾向队头逐步判断队列内元素与当前遍历的数的大小关系
        // 要考虑上一轮新加入的元素在队列中的大小情况
        /* 如果从队头向队尾判断，
        可能会导致比当前遍历对象更大的值无法被删除，
        在下次遍历时成为队头元素并被误认为是最小值
        --------------------------------------------
        比如 -3  5  3  6  7
        【-3  5】 3  6  7 遍历到3时
        1.从队头向队尾判断
        -3小于3 队列中无元素出队，3被加入队列
        -3 【 5  3 】 6  7 下一轮遍历6时，队头元素小于6，无元素出队
        而此时的队头元素是5，若选择队头作为最小值，就出错了
        --------------------
        2.从队尾向队头判断
        5大于3，5出队，-3小于3，3被加入队列
        -3 【 3 】 6  7 下一轮遍历6时，队头元素为3是最小值，正确
        */
        while (tt >= hh && a[i] <= a[q[tt]]) tt -- ; // 注意队列不为空的条件
        // 队尾元素大于等于当前遍历对象时，队尾出队

        q[ ++ tt] = i; // 将当前遍历对象添加进队列
        // 添加元素要在输出之前，因为可能队列中的所有元素都小于当前遍历对象
        // 从而导致队列为空，此时新添加进队列的元素成为队头
        if (i >= k - 1) printf("%d ", a[q[hh]]); // 注意到只有当窗口完全包含数据时才开始输出

        // for (int i = hh; i <= tt; i ++ ) cout << q[i] << " ";
        // cout << endl;
    }
    puts("");

    //查找最大值
    hh = 0, tt = -1; // 重新初始化队头和队尾
    for (int i = 0; i < n; i ++ )
    {
        if (tt >= hh && q[hh] < i - k + 1) hh ++ ;
        while (tt >= hh && a[q[tt]] <= a[i]) tt -- ;
        q[ ++ tt] = i;
        if (i >= k - 1) printf("%d ", a[q[hh]]);
    }
    puts("");

    return 0;
}
```