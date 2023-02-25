# topological sort



* 给定一幅有向图，将所有的顶点排序，使得所有的有向边均从排在前面的元素指向排在后面的元素（或者说明无法做到这一点）





![12.3.jpg](https://cdn.acwing.com/media/article/image/2020/12/03/7340\_90ac75f835-12.3.jpg)

###

![12.3(2.jpg](https://cdn.acwing.com/media/article/image/2020/12/03/7340\_113a238c35-12.3\(2.jpg)

![12.3(3.jpg](https://cdn.acwing.com/media/article/image/2020/12/03/7340\_2005218c35-12.3\(3.jpg)

###

### Kahn算法

* 把所有入度为 00 的点入队
* 从队里拿出一个点，输出。将这个点所有出边的终点的入度减一。当发现新的入度为 00 的点，入队。
* 重复直到队列为空



![12.3(3.jpg](https://cdn.acwing.com/media/article/image/2020/12/03/7340\_2005218c35-12.3\(3.jpg)

### 例题：拓扑排序模版

题目： [拓扑排序模板](https://www.luogu.com.cn/problem/U107394)

C++ 代码：

```
#include <bits/stdc++.h>

using std::cin;
using std::cout;
using std::priority_queue;
using std::vector;

const int N = 1e5 + 5;

int n, m;
int in[N];
vector<int> adj[N];

void topologicalSort() {
    priority_queue<int> q;
    for (int i = 1; i <= n; ++i) {
        if (in[i] == 0) {
            q.push(-i);
        }
    }
    while (!q.empty()) {
        int u = -q.top(); q.pop();
        cout << u << " ";
        for (int i = 0; i < adj[u].size(); ++i) {
            int v = adj[u][i];
            in[v]--;
            if (in[v] == 0) {
                q.push(-v);
            }
        }
    }
}

int main() {
    cin >> n >> m;
    for (int i = 0; i < m; ++i) {
        int u, v;
        cin >> u >> v;
        adj[u].push_back(v);
        in[v]++;
    }

    topologicalSort();

    return 0;
}
```
