---
description: coloring method and hungarian matching
---

# bipartite graph

bipartite = 2 colorable = no odd number of cycles

a graph is bipartite if and only if it is two-colorable.

```cpp
//color 1 and 2, 0 = not colored yet

#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 100010, M = 200010;

int n, m;
int h[N], e[M], ne[M], idx;
int color[N];

void add(int a, int b)
{
    e[idx] = b, ne[idx] = h[a], h[a] = idx ++ ;
}

//set node u to color c and recurse its neighbors
// setting neighbors to the other color 3-c
bool dfs(int u, int c)
{
    color[u] = c;

    for (int i = h[u]; i != -1; i = ne[i])
    {
        int j = e[i];
        if (!color[j])
        {
            if (!dfs(j, 3 - c)) return false;
        }
        else if (color[j] == c) return false;
    }

    return true;
}

int main()
{
    scanf("%d%d", &n, &m);

    memset(h, -1, sizeof h);

    while (m -- )
    {
        int a, b;
        scanf("%d%d", &a, &b);
        add(a, b), add(b, a);
    }

    bool flag = true;
    for (int i = 1; i <= n; i ++ )
        if (!color[i])
        {
            if (!dfs(i, 1))
            {
                flag = false;
                break;
            }
        }

    if (flag) puts("Yes");
    else puts("No");

    return 0;
}


```

bfs

```cpp
/*
代码思路
颜色 1 和 2 表示不同颜色, 0 表示 未染色
定义queue是存PII，表示 <点编号, 颜色>,
同理，遍历所有点, 将未染色的点都进行bfs
队列初始化将第i个点入队, 默认颜色可以是1或2
while (队列不空)
每次获取队头t, 并遍历队头t的所有邻边
若邻边的点未染色则染上与队头t相反的颜色，并添加到队列
若邻边的点已经染色且与队头t的颜色相同, 则返回false
*/
#include <iostream>
#include <algorithm>
#include <cstring>

using namespace std;
const int N = 1e5 + 10, M = 2e5 + 10;
typedef pair<int, int> PII;

int e[M], ne[M], h[N], idx;
int n, m;
int st[N];

void add(int a, int b){
    e[idx] = b, ne[idx] = h[a], h[a] = idx ++;
}

bool bfs(int u){
    int hh = 0, tt = 0;
    PII q[N];
    q[0] = {u, 1};
    st[u] = 1;

    while(hh <= tt){
        auto t = q[hh ++];
        int ver = t.first, c = t.second;

        for (int i = h[ver]; i != -1; i = ne[i]){
            int j = e[i];

            if(!st[j])
            {
                st[j] = 3 - c;
                q[++ tt] = {j, 3 - c};
            }
            else if(st[j] == c) return false;
        }
    }

    return true;
}

int main(){
    scanf("%d%d", &n, &m);

    memset(h, -1, sizeof h);
    while(m --){
        int a, b;
        scanf("%d%d", &a, &b);
        add(a, b), add(b, a); // 无向图
    }

    int flag = true;
    for(int i = 1; i <= n; i ++) {
        if (!st[i]){
            if(!bfs(i)){
                flag = false;
                break;
            }
        }
    }

    if (flag) puts("Yes");
    else puts("No");
    return 0;
}

作者：gyh
链接：https://www.acwing.com/solution/content/5281/
来源：AcWing
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

A graph containing **odd number of cycles** or **Self loop**  is **Not Bipartite.**

{% embed url="https://www.geeksforgeeks.org/bipartite-graph/" %}

****[**https://cp-algorithms.com/graph/bipartite-check.html**](https://cp-algorithms.com/graph/bipartite-check.html)****

hungarian matching

[https://brilliant.org/wiki/hungarian-matching/](https://brilliant.org/wiki/hungarian-matching/)
