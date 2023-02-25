---
description: different from book code 非递归实现组合型枚举
---

# non-recursion version of recursion

[https://leetcode-cn.com/problems/combinations/solution/fei-di-gui-shi-xian-zu-he-xing-mei-ju-by-cheng-zha/](https://leetcode-cn.com/problems/combinations/solution/fei-di-gui-shi-xian-zu-he-xing-mei-ju-by-cheng-zha/)

非递归实现组合型枚举

[https://blog.nowcoder.net/n/2eb0fa40d9904d0b9df4773c5fc94332](https://blog.nowcoder.net/n/2eb0fa40d9904d0b9df4773c5fc94332)



```cpp
题目描述
从 1~n 这 n 个整数中随机选出 m 个，输出所有可能的选择方案。 n > 0, 0 ≤ m ≤ n, n + ( n − m ) ≤ 25。

输入描述:
两个整数n，m。

输出描述:
按照从小到大的顺序输出所有方案，每行1个。
首先，同一行内的数升序排列，相邻两个数用一个空格隔开。其次，对于两个不同的行，对应下标的数一一比较，字典序较小的排在前面（例如1 3 9 12排在1 3 10 11前面）。

思路：
这道题与  这道题完全一致，只是使用递归与不使用的区别，然而这两题使用不使用都能AC，建议参考我写的  这道题的代码是可以依据递归的深搜化为3步进行

1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
void dfs(int u, int sum, int state) {
    //0：
    if (sum + n - u < m) return;
    if (sum == m) {
        for (int i = 0; i < n; i++)
            if (state >> i & 1)
                cout << i + 1 << " ";
        cout << endl;
        return;
    }
    dfs(u + 1, sum + 1, state | (1 << u));
    //1：
    dfs(u + 1, sum, state);
    //2：
}
这里就不多解释了
完整C++版AC代码（非递归）

1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
19
20
21
22
23
24
25
26
27
28
29
30
31
32
33
34
35
36
37
38
39
40
41
42
43
44
45
46
47
48
49
50
51
52
#include <iostream>
#include <stack>
using namespace std;
 
struct State {
    int pos, u, sum, state;//pos是你遍历到程序的哪一步了
};
 
int main() {
    int n, m;
    cin >> n >> m;
    stack<State> stk;//栈记录好你遍历分支的哪一个部分
    stk.push({ 0,0,0,0 });
 
    while (stk.size()) {
 
        auto t = stk.top();
        stk.pop();
 
 
        if (t.pos == 0) {
 
            if (t.sum + n - t.u < m) continue;
 
            if (t.sum == m) {
                for (int i = 0; i < n; i++)
                    if (t.state >> i & 1)
                        cout << i + 1 << " ";
                cout << endl;
                continue;
            }
 
            t.pos = 1;
            stk.push(t);
            stk.push({ 0,t.u + 1,t.sum + 1,t.state | 1 << t.u });
            continue;
 
        }
        if (t.pos == 1) {
 
            t.pos = 2;
            stk.push(t);
            stk.push({ 0,t.u + 1,t.sum,t.state });
            continue;
 
        }
        else {
            continue;
        }
    }
    return 0;
}
```





```cpp
非递归实现组合型枚举（状态压缩）
 2020-06-27 
枚举递归
题目描述：
从 1~n 这 n 个整数中随机选出 m 个，输出所有可能的选择方案。n>0,0≤m≤n, n+(n?m)≤25。

输入描述：
两个整数n，m。

输入描述：
按照从小到大的顺序输出所有方案，每行1个。

首先，同一行内的数升序排列，相邻两个数用一个空格隔开。其次，对于两个不同的行，对应下标的数一一比较，字典序较小的排在前面（例如1 3 9 12排在1 3 10 11前面）。

样例：
输入：
5 3

输出：
1 2 3
1 2 4
1 2 5
1 3 4
1 3 5
1 4 5
2 3 4
2 3 5
2 4 5
3 4 5

思路：
采取状态压缩（可以枚举所有选与不选的情况）记录输出的序列的情况

代码：
使用栈模拟dfs的非递归版本：
1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
19
20
21
22
23
24
25
26
27
28
29
30
31
32
33
#include<cstdio>
#include<stack>
#include<iostream>
using namespace std;
struct state {
    int pos,num,a;//第pos位，有num个数字，用二进制状态压缩进a
};
stack<state> s;
int n,m;
int main() {
    scanf("%d%d",&n,&m);
    s.push((state) {0,0,0});
    while(!s.empty()) {
        state a=s.top();
        s.pop();
        if (a.num+n-a.pos<m) continue;
        if (a.num==m) {
            for (int i=0; i<n; i++)
                if ((a.a>>i)&1) printf("%d ",i+1);
            cout<<endl;
            continue;
        }
        if (a.pos<n) {
            s.push((state) {
                a.pos+1,a.num,a.a
            });
            s.push((state) {
                a.pos+1,a.num+1,a.a|(1<<a.pos)
            });//题目是从小到大输出，所以小的要放在后面，先从栈中取出。这与dfs不太一样
        }
    }
    return 0;
}
dfs版本：
1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
19
20
21
22
23
24
25
26
#include<iostream>
using namespace std;
int n, m;

void dfs(int cur, int sum, int state) {//sum表示当前选了多少个数,state表示选和不选的一种状态

    if (sum + n - cur < m) return;

    if (sum == m) {

        for (int i = 0; i < n; i++)
            if (state >> i & 1)
                cout << i + 1 << " ";

        cout << endl;
        return;
    }
    dfs(cur + 1, sum + 1, state | (1 << cur));
    dfs(cur + 1, sum, state);
}

int main() {
    cin >> n >> m;
    dfs(0, 0, 0);
    return 0;
}
```



[https://www.codenong.com/cs106973304/](https://www.codenong.com/cs106973304/)
