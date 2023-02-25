---
description: offset technique can be easily expanded
---

# day3 offset method

p756 snake matrix p1102 horse moves p844 maze and any flood fill algorithm eg p687; p1421 usaco

```
//offset technique

#include <iostream>

using namespace std;

int m,n;

int const N = 110;

int f[N][N];

int main()
{
    cin >> n >> m;
    int dx[4] = { 0, 1, 0, -1};
    int dy[4] = { 1, 0, -1, 0};//准备两个数值表示当前行走方向 依此为 东-南-西-北-东-...(右-下-左-上-右-...) 
    int x = 1, y = 1,d = 0;//x,y表示从(1,1)点开始行走 ， d表示初始方向为东  
    for(int i = 1; i <= n * m; i ++)
    {
        if((x + dx[d] > n || y + dy[d] > m || y + dy[d] == 0) || (f[x + dx[d]][y + dy[d]]))//判断行走的下一个状态是否碰壁 
        //( 下移时是否碰越下界 || 右移时是否越右界  || 左移时是否越左界) || (若不改变移动方向 下一点是否已经到达过)
            d = (d + 1) % 4;//碰壁后换移动方向 
        f[x][y] = i;//标记当前到达点 
        x += dx[d];
        y += dy[d];//以当前方向(可能改变也可能未改变)移动一次 
    }
    for(int i = 1; i <= n; i ++)
    {
        for(int j = 1;j <= m; j ++)
            cout << f[i][j] << ' ';
        cout << endl;
    }//输出 
    return 0;
}


```

```
//利用 left right top bottom 四个变量 来表示 这个矩形的边界
//simulation method

#include <iostream>

using namespace std;
const int N = 105;

int a[N][N];
int n, m;

int main() {
    cin >> n >> m;

    int left = 0, right = m - 1, top = 0, bottom = n - 1;
    int k = 1;
    while (left <= right && top <= bottom) {
        for (int i = left ; i <= right; i ++) {
            a[top][i] = k ++;
        }
        for (int i = top + 1; i <= bottom; i ++) {
            a[i][right] = k ++;
        }
        for (int i = right - 1; i >= left && top < bottom; i --) {
            a[bottom][i] = k ++;
        }
        for (int i = bottom - 1; i > top && left < right; i --) {
            a[i][left] = k ++;
        }
        left ++, right --, top ++, bottom --;
    }
    for (int i = 0; i < n; i ++) {
        for (int j = 0; j < m; j ++) {
            cout << a[i][j] << " ";
        }
        cout << endl;
    }
    return 0;
}
```
