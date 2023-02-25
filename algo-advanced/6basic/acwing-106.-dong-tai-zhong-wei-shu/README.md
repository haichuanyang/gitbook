---
description: 对顶堆=大根堆+小根堆
---

# 106. 动态中位数

```
AcWing 106. 动态中位数    原题链接    困难
作者：    秦淮岸灯火阑珊 ,  2019-01-22 15:58:07 ,  阅读 1785

18


5
题目描述
依次读入一个整数序列，每当已经读入的整数个数为奇数时，输出已读入的整数构成的序列的中位数。

输入格式
第一行输入一个整数P，代表后面数据集的个数，接下来若干行输入各个数据集。

每个数据集的第一行首先输入一个代表数据集的编号的整数。

然后输入一个整数M，代表数据集中包含数据的个数，M一定为奇数，数据之间用空格隔开。

数据集的剩余行由数据集的数据构成，每行包含10个数据，最后一行数据量可能少于10个，数据之间用空格隔开。

输出格式
对于每个数据集，第一行输出两个整数，分别代表数据集的编号以及输出中位数的个数（应为数据个数加一的二分之一），数据之间用空格隔开。

数据集的剩余行由输出的中位数构成，每行包含10个数据，最后一行数据量可能少于10个，数据之间用空格隔开。

输出中不应该存在空行。

数据范围
1≤P≤10001≤P≤1000
1≤M≤99991≤M≤9999
样例
输入样例：
3 
1 9 
1 2 3 4 5 6 7 8 9 
2 9 
9 8 7 6 5 4 3 2 1 
3 23 
23 41 13 22 -3 24 -31 -11 -8 -7 
3 5 103 211 -311 -45 -67 -73 -81 -99 
-33 24 56
输出样例：
1 5
1 2 3 4 5
2 5
9 8 7 6 5
3 12
23 23 22 22 13 3 5 5 3 -3 
-7 -3
对顶堆
这是一个很有用的算法作者第一次学到，具体思路大致是，开两个堆，一个是大根堆，一个是小根堆，然后小于中位数的都放在大根堆，大于中位数的都放在小根堆，如果说，一个堆的个数大于了当前序列的1212，那么就将多余的数移过去，直到两个堆数量相等。
C++ 代码

#include <iostream>
#include <algorithm>
#include <cstdio>
#include <cstring>
#include <queue>
#include <vector>
using namespace std;
struct cmp1
{
    bool operator ()(int &a,int &b)
    {
        return a>b;//小根堆，不是大根堆
    }
};
priority_queue <int,vector<int>, cmp1> q1,kong1;
priority_queue <int> q2,kong2;
void init()
{
    int t,x,n,now;
    cin>>t;
    while(t--)
    {
        cin>>x>>n;
        cout<<x<<" "<<(n+1)/2<<endl;
        q1=kong1;
        q2=kong2;
        int cnt=0;
        for (int i=1;i<=n;i++)
        {
            cin>>now;
            if(q1.empty())
                q1.push(now);
            else
            {
                if(now>q1.top()) 
                    q1.push(now);
                else 
                    q2.push(now);
                while(q1.size()<q2.size())
                {
                    q1.push(q2.top());
                    q2.pop();
                }
                while(q1.size()>q2.size()+1)
                {
                    q2.push(q1.top());
                    q1.pop();
                }
            }
            if (i&1)
            {
                cnt++;
                cout<<q1.top()<<" ";
                if (!(cnt%10))
                    cout<<endl;
            }
        }
        puts("");
    }
}
int main()
{
    init();
    return 0;
}

作者：秦淮岸灯火阑珊
链接：https://www.acwing.com/solution/content/838/

```

```
//yxc ; vector.h is included in queue.h!

#include <cstdio>
#include <cstring>
#include <algorithm>
#include <queue>

using namespace std;

int main()
{
    int T;
    scanf("%d", &T);
    while (T -- )
    {
        int n, m;
        scanf("%d%d", &m, &n);
        printf("%d %d\n", m, (n + 1) / 2);

        priority_queue<int> down;
        priority_queue<int, vector<int>, greater<int>> up;

        int cnt = 0;
        for (int i = 1; i <= n; i ++ )
        {
            int x;
            scanf("%d", &x);

            if (down.empty() || x <= down.top()) down.push(x);
            else up.push(x);

            if (down.size() > up.size() + 1) up.push(down.top()), down.pop();
            if (up.size() > down.size()) down.push(up.top()), up.pop();

            if (i % 2)
            {
                printf("%d ", down.top());
                if ( ++ cnt % 10 == 0) puts("");
            }
        }

        if (cnt % 10) puts("");
    }

    return 0;
}


```
