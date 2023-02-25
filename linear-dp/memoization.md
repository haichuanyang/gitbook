# memoization

```cpp
题目描述
给定一个R行C列的矩阵，表示一个矩形网格滑雪场。

矩阵中第 i 行第 j 列的点表示滑雪场的第 i 行第 j 列区域的高度。

一个人从滑雪场中的某个区域内出发，每次可以向上下左右任意一个方向滑动一个单位距离。

当然，一个人能够滑动到某相邻区域的前提是该区域的高度低于自己目前所在区域的高度。

下面给出一个矩阵作为例子：

1  2  3  4 5

16 17 18 19 6

15 24 25 20 7

14 23 22 21 8

13 12 11 10 9
在给定矩阵中，一条可行的滑行轨迹为24-17-2-1。

在给定矩阵中，最长的滑行轨迹为25-24-23-…-3-2-1，沿途共经过25个区域。

现在给定你一个二维矩阵表示滑雪场各区域的高度，请你找出在该滑雪场中能够完成的最长滑雪轨迹，并输出其长度(可经过最大区域数)。

输入格式
第一行包含两个整数R和C。

接下来R行，每行包含C个整数，表示完整的二维矩阵。

输出格式
输出一个整数，表示可完成的最长滑雪长度。

数据范围
1≤R,C≤300,
0≤矩阵中整数≤10000

输入样例：
5 5
1 2 3 4 5
16 17 18 19 6
15 24 25 20 7
14 23 22 21 8
13 12 11 10 9
输出样例：
25
//经过评论区网友fzj2007  提醒 修改了接收参数相反的BUG 2020 11 14

算法1
一开始 写的常规DFS
但是在某些数据 超时。果然没那么简单

代码

#include <iostream>
#include <vector>

using namespace std;

const int N = 310;
int n,m;
int maxlen = -1;
vector<vector<int>>  vv;

int addx[] = {1,-1,0,0};
int addy[] = {0,0,1,-1};

void dfs(int x,int y,int len)
{
    if(len > maxlen) maxlen = len;

    for(int i = 0;i < 4;i++){
        int newx = x + addx[i];
        int newy = y + addy[i];

        if(newx >=0 && newx < n && newy >=0 && newy < m &&
            vv[newx][newy] < vv[x][y])
        {
            dfs(newx,newy,len+1);        
        }
    }
}


int main()
{
    cin >> n >> m;

    for(int i = 0; i < n;i++){
        vector<int> v;
        vv.push_back(v);
        for(int j = 0;j< m;j++){
            int t =0;
            cin >> t;
            vv[i].push_back(t);    
        }
    }


    for(int i = 0; i < n;i++){
        for(int j = 0;j < m;j++){
            int len =0;
            dfs(i,j,len);
        }
    }

    cout << maxlen+1  << endl;

    return 0;
}

算法2
使用记忆化数组 记录每个点的最大滑动长度
遍历每个点作为起点
然后检测该点四周的点 如果可以滑动到其他的点
那么该点的最大滑动长度 就是其他可以滑到的点的滑动长度+1
dp[i][j] = max(dp[i][j-1], dp[i][j+1],dp[i-1][j],dp[i+1][j])

由于滑雪是必须滑到比当前低的点 所以不会存在一个点多次进入的问题
如果该点的dp[][] 不为初始化值 那么就说明计算过 不必再次计算。

按照上述公式 代码如下 AC

#include <iostream>
#include <algorithm>
using namespace std;

const int N = 310;
int R, C;

int arr[N][N];
int dp[N][N];

int maxlen = -1;

int addx[] = { 1,-1,0,0 };
int addy[] = { 0,0,1,-1 };

int  Dfs(int x, int y)
{
    if (dp[x][y] != 0) return dp[x][y];


    for (int i = 0; i < 4; i++) {
        int newx = x + addx[i];
        int newy = y + addy[i];

        if (newx >= 0 && newx < R && newy >= 0 && newy < C &&
            arr[newx][newy] < arr[x][y])
        {
            dp[x][y] = max(dp[x][y], Dfs(newx, newy) + 1);
        }
    }
    return dp[x][y];
}

int main()
{
    ios::sync_with_stdio(false);

    cin >> R >> C;

    for (int i = 0; i < R; i++) {
        for (int j = 0; j < C; j++) {
            cin >> arr[i][j];
        }
    }

    for (int i = 0; i < R; i++) {
        for (int j = 0; j < C; j++) {
            int len = Dfs(i, j);
            maxlen = max(maxlen, len);
        }
    }

    cout << maxlen + 1 << endl;

    return 0;
}


作者：itdef
链接：https://www.acwing.com/solution/content/5280/

//来自算法基础课

记忆化搜索
dfs+记忆化 = DP
DP的本质不就是爆搜的优化嘛
（也有国集大佬说本质是状态压缩的）

1.状态表示：

集合：所有从(i,j)这个点开始滑的路径长度
属性：max
2.状态计算：

就是搜索，每个点能不能向上下左右动

下面代码中的f[x][y] = max(f[x][y],dp(xx,yy)+1);
实际上就是向四个方向判断之后转移：
if(a[i-1][j]<now) f[i][j] = max(f[i][j],f[i-1][j]+1);//上
if(a[i+1][j]<now) f[i][j] = max(f[i][j],f[i+1][j]+1);//下
if(a[i][j-1]<now) f[i][j] = max(f[i][j],f[i][j-1]+1);//左
if(a[i][j+1]<now) f[i][j] = max(f[i][j],f[i][j+1]+1);//右
(bu)意外的收获：
DP没有后效性 <==> 抽象出来的图论模型是一个拓扑图
（众所周知DP其实就是一个DAG嘛）

CODE
#include<bits/stdc++.h>
#define read(x) scanf("%d",&x)
using namespace std;
const int N = 310;
int n,m,a[N][N],f[N][N];
int dx[4] = {-1,0,1,0};
int dy[4] = {0,1,0,-1};
int dp(int x,int y) {
    if(f[x][y]!=0) return f[x][y]; //记忆化的好处 

    f[x][y] = 1;//初始化 
    for(register int i=0; i<4; i++) {
        int xx = x+dx[i];
        int yy = y+dy[i];
        if(xx>=1&&xx<=n && yy>=1&&y<=m && a[x][y]>a[xx][yy])
            f[x][y] = max(f[x][y],dp(xx,yy)+1);
    }
    return f[x][y];
}

int main() {
    read(n),read(m);
    for(register int i=1; i<=n; i++)
        for(register int j=1; j<=m; j++)
            read(a[i][j]);
    int ans = 0;
    for(register int i=1; i<=n; i++)
        for(register int j=1; j<=m; j++)
            ans = max(ans,dp(i,j));
    printf("%d\n",ans);
    return 0;
}
这里再提供一种做法
priority_queue+bfs式的dp
这个dp的思路很清晰
跑的也很快
时间复杂度好像是O(n)？
我不会算时间复杂度诶

#include<iostream>
#include<algorithm>
#include<queue>
using namespace std;
struct node{
    int i,j,num;
};
struct cmp1{
    bool operator()(node x,node y){
        return x.num>y.num;
    }
};
priority_queue<node,vector<node>,cmp1>q;

int n,m,f[101][101],g[101][101],maxn;

void bfs() {
    while(!q.empty()){
        node tmp = q.top(); 
        int i = tmp.i;
        int j = tmp.j;
        int now = tmp.num;
        q.pop();

        if(g[i-1][j]<now) f[i][j]=max(f[i][j],f[i-1][j]+1);
        if(g[i+1][j]<now) f[i][j]=max(f[i][j],f[i+1][j]+1);
        if(g[i][j-1]<now) f[i][j]=max(f[i][j],f[i][j-1]+1);
        if(g[i][j+1]<now) f[i][j]=max(f[i][j],f[i][j+1]+1);
        if(maxn<f[i][j]) maxn=f[i][j];
    }
}

int main(){
    scanf("%d%d",&n,&m);
    for(int i=1;i<=n;i++)
        for(int j=1;j<=m;j++){
            f[i][j]=1;
            scanf("%d",&g[i][j]);
            node a;
            a.i=i;
            a.j=j;
            a.num=g[i][j];
            q.push(a);
    }
    bfs();
    printf("%d\n",maxn);
    return 0;
}

作者：Shadow
链接：https://www.acwing.com/solution/content/5629/

```
