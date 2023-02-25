# castle

```
scanf做法
#include<cstdio>
int main(){
    double f1, f2;
    scanf("%lf %lf", &f1, &f2);
    double res = ( f1 * 3.5 + f2 * 7.5) / ( 3.5 + 7.5);
    printf("MEDIA = %.5lf",res);
}
float进度不够,要double

题解AcWing 1359. 城堡[BFS or DFS]
adnil8130的头像adnil8130
6小时前
题目描述
太长了。。看原题吧hh

样例
输入：
7 4
11 6 11 6 3 10 6
7 9 6 13 5 15 5
1 10 12 7 13 7 5
13 11 10 8 10 12 13

输出：
5
9
16
4 1 E
算法1
(BFS or DFS) O(mn)
先用FloodFill算法遍历图，对每一个格子进行染色，每一次FloodFill完要记录该种颜色的面积；
然后枚举每个方格每个方向，如果墙两边颜色不同，就检查一下两种颜色是否合起来为最大；
BFS DFS都实现了一下。

时间复杂度
Flood Fill遍历全图 O(mn)；
检查每一堵墙时间复杂度 O(4mn)；
总时间复杂度O(mn)
参考文献
C++ 代码
#include <iostream>
#include <cstdio>
#include <cstring>
#include <algorithm>
#include <queue>

using namespace std;
const int N = 55;
int n, m, graph[N][N], visited[N][N]; // graph是每个格子墙的状态，visited是每一个格子的颜色
int color, areas[N * N], dr[] = {0, -1, 0, 1}, dc[] = {-1, 0, 1, 0}; // areas是每种颜色的面积
char direct[] = {'W', 'N', 'E', 'S'};

// dfs版本Flood Fill
void dfs(int r, int c, int &cnt){
    visited[r][c] = color; // 染色
    ++cnt; // 数数
    for (int d = 0; d < 4; ++d)
        if ((graph[r][c] >> d & 1) == 0){
            int nr = r + dr[d], nc = c + dc[d];
            if (nr >= 1 && nr <= m && nc >= 1 && nc <= n && visited[nr][nc] == -1)
                dfs(nr, nc, cnt);
        }
}

// bfs版本flood fill
void bfs(int r, int c, int &cnt){
    typedef pair<int, int> PII;
    queue<PII> Q; Q.push({r, c});
    visited[r][c] = color; // 染色
    ++cnt; // 数数

    while (Q.size()){
        PII r_c = Q.front(); Q.pop();
        int r = r_c.first, c = r_c.second;
        for (int d = 0; d < 4; ++d)
            if ((graph[r][c] >> d & 1) == 0){
                int nr = r + dr[d], nc = c + dc[d];
                if (nr >= 1 && nr <= m && nc >= 1 && nc <= n && visited[nr][nc] == -1){
                    ++cnt; // 数数
                    Q.push({nr, nc});
                    visited[nr][nc] = color; // 染色
                }
            }
    }
}

struct Wall{
    int r, c, d;
};

int main(){
    // 读取数据
    scanf("%d %d\n", &n, &m);
    for (int i = 1; i <= m; ++i)
        for (int j = 1; j <= n; ++j)
            scanf("%d\n", &graph[i][j]);

    // 所有颜色初始化为-1，代表未被遍历过
    memset(visited, -1, sizeof visited);
    // 对每一个没遍历过的进行flood fill
    for (int i = 1; i <= m; ++i)
        for (int j = 1; j <= n; ++j)
            if (visited[i][j] == -1){
                int cnt =  0;
                // bfs(i, j, cnt);
                dfs(i, j, cnt);
                areas[color++] = cnt;
            }

    // 输出颜色数
    printf("%d\n", color);

    // 输出最大面积数
    int max_area = 0;
    for (int i = 0; i < color; ++i)
        max_area = max(max_area, areas[i]);
    printf("%d\n", max_area);

    // 遍历每个格子，查看能merge成的最大面积
    int merge_max_area = 0;
    Wall wall = {m, 1, 1};
    // 细节：根据存在多组情况解的优先级，我们从左下角向右上角遍历，同时遍历方向为北->东->南->西
    for (int c = 1; c <= n; ++c)
        for (int r = m; r >= 1; --r)
            for (int d = 1; d <= 4; ++d){
                int nr = r + dr[d % 4], nc = c + dc[d % 4];
                if (nr >= 1 && nr <= m && nc >= 1 && nc <= n && visited[r][c] != visited[nr][nc]){
                    if (areas[visited[r][c]] + areas[visited[nr][nc]] > merge_max_area){
                        merge_max_area = areas[visited[r][c]] + areas[visited[nr][nc]];
                        wall = {r, c, d};
                    } 
                }
            }

    // 输出结果
    printf("%d\n", merge_max_area);
    printf("%d %d %c\n", wall.r, wall.c, direct[wall.d]);

    return 0;
}
```
