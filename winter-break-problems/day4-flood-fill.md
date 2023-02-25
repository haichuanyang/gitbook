# day4 flood fill

p1113 red and black, get connected sections

bfs dfs same; bfs more like human thought; need queue for code, but can get shortest distance o(nm); dfs more like machine thinking, could blow up stack, acwing 256MB; code shorter

```
// bfs
#include<iostream>
#include<cstring>
#include<queue>
#define x first
#define y second

using namespace std;
typedef pair<int,int> PII;

const int N=30;

char g[N][N];
int n,m;
int dx[4]={0,1,0,-1},dy[4]={1,0,-1,0};
bool st[N][N];

int bfs(int x,int y)
{
    int cnt=1;
    queue<PII> q;
    q.push({x,y});
    while(q.size())
    {
        PII t=q.front();
        q.pop();
        int x=t.x,y=t.y;
        for(int i=0;i<4;i++)
        {
            int a=x+dx[i],b=y+dy[i];
            if(a<0 || a>=n || b<0 || b>=m) continue;
            if(st[a][b]) continue;
            if(g[a][b]!='.') continue;
            st[a][b]=true;
            q.push({a,b});
            cnt++;
        }
    }
    return cnt;
}


int main()
{
    while(cin>>m>>n,n||m)
    {
        memset(st,0,sizeof st);
        for(int i=0;i<n;i++) scanf("%s",g[i]);
        int x,y,flag=0;
        for(int i=0;i<n;i++)
        {
            for(int j=0;j<m;j++)
                if(g[i][j]=='@')
                {
                    x=i,y=j;
                    flag=1;
                }
            if(flag) break;
        }
        cout<< bfs(x,y) <<endl;
    }
    return 0;
}

作者：Accepting
链接：https://www.acwing.com/solution/content/20337/


AcWing 1113. 红与黑：深度优先遍历详解    原题链接    简单
作者：    猿六 ,  2021-01-12 18:39:17 ,  阅读 371

15


11
深度优先遍历算法。

对于某个点执行如下算法：
1. 从该点出发，如果可以往上走就往上走，对上方点递归执行该算法。
2. 从该点出发，如果可以往右走就往右走，对右方点递归执行该算法。
3. 从该点出发，如果可以往下走就往下走，对下方点递归执行该算法。
4. 从该点出发，如果可以往左走就往左走，对左方点递归执行该算法。

//cpp
#include <cstring>
#include <iostream>
using namespace std;

const int N = 25;

int n, m;
char g[N][N];//存储地板


int dx[4] = {-1, 0, 1, 0}, dy[4] = {0, 1, 0, -1};//上右下左四个方向

int dfs(int x, int y)//深度优先遍历
{
    int cnt = 1;

    g[a][b] = '#';
    for (int i = 0; i < 4; i ++ )//走四个方向
    {
        int a = x + dx[i], b = y + dy[i];
        if (a < 0 || a >= n || b < 0 || b >= m) continue;
        if (g[a][b] != '.') continue;
        cnt += dfs(a, b);//如果可以走向某一个方向，对该方向上的点递归
    }

    return cnt;
}

int main()
{
    while (cin >> m >> n, n || m)
    {
        for (int i = 0; i < n; i ++ ) cin >> g[i];//一次读入一行

        int x, y;
        for (int i = 0; i < n; i ++ )
            for (int j = 0; j < m; j ++ )
                if (g[i][j] == '@')
                {
                    x = i;
                    y = j;
                }

        cout << dfs(x, y) << endl;
    }

    return 0;
}

作者：猿六
链接：https://www.acwing.com/solution/content/29185/
另一种递归思路。其实也是深度优先遍历
少开辟一个数组而已

#include <iostream>
using namespace std;

const int N = 30;
char a[N][N];
int n, m;

int find(int x = 3, int y = 1)
{
    int res = 0;
    if (x > 0 && a[x - 1][y] != '#')
    {
        a[x][y] = '!';
        if (a[x - 1][y] != '!')
        {
            res += 1;
            res += find(x - 1, y);
        }
    }
    if (x < n - 1 && a[x + 1][y] != '#')
    {
        a[x][y] = '!';
        if (a[x + 1][y] != '!')
        {
            res += 1;
            res += find(x + 1, y);
        }
    }
    if (y > 0 && a[x][y - 1] != '#')
    {
        a[x][y] = '!';

        if (a[x][y - 1] != '!')
        {
            res += 1;
            res += find(x, y -1 );
        }
    }
    if (y < m-1 && a[x][y + 1] != '#')
    {
        a[x][y] = '!';
        if (a[x][y + 1] != '!')
        {
            res += 1;
            res += find(x, y + 1);
        }
    }
    return res;
}

int main()
{
    while(true)
    {
    cin >> m >> n;
    if(n == 0 && m == 0) break;
    int posx, posy;
    for (int i = 0; i < n; i++)
    {
        for (int j = 0; j < m; j++)
        {
            cin >> a[i][j];
            if (a[i][j] == '@') posx = i, posy = j;
        }
    }
    int res = find(posx, posy);
    cout << res + 1<< endl;
    }
}

作者：猿六
链接：https://www.acwing.com/solution/content/29181/

//dfs
#include<iostream>
#include<cstring>

using namespace std;

const int N=30;

char g[N][N];
int n,m;
int dx[4]={0,1,0,-1},dy[4]={1,0,-1,0};
bool st[N][N];

int dfs(int x,int y)
{
    st[x][y]=true;
    int cnt=1;
    for(int i=0;i<4;i++)
    {
        int a=x+dx[i],b=y+dy[i];
        if(a<0 || a>=n || b<0 || b>=m) continue;
        if(st[a][b]) continue;
        if(g[a][b]=='#') continue;
        cnt+=dfs(a,b);
    }
    return cnt;
}


int main()
{
    while(cin>>m>>n,n||m)
    {
        memset(st,0,sizeof st);
        for(int i=0;i<n;i++) scanf("%s",g[i]);
        int x,y,flag=0;
        for(int i=0;i<n;i++)
        {
            for(int j=0;j<m;j++)
                if(g[i][j]=='@')
                {
                    x=i,y=j;
                    flag=1;
                }
            if(flag) break;
        }
        cout<< dfs(x,y) <<endl;
    }
    return 0;
}

作者：Accepting
链接：https://www.acwing.com/solution/content/20337/

```
