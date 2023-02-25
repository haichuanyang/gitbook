# usaco training from acwing

虫洞那题判断环和基环树判断环的方法是一样的hh

simulation type problem is easiest -&#x20;

unordered map action is O(1) map is O(lgn)

```cpp
AcWing 1359. 城堡【并查集】
滑稽_ωﾉ的头像滑稽_ωﾉ
2小时前
题目描述
一个网格图相邻的格子之间可能有墙壁，左上右下是否有墙壁用一个二进制数的第0123位表示，相邻的格子之间没有墙壁则视为同一个房间

问有几个房间，最大的房间多大，删掉一面墙后最大的房间多大，应该删掉哪面墙。

1≤N,M≤50
输入样例
7 4
11 6 11 6 3 10 6
7 9 6 13 5 15 5
1 10 12 7 13 7 5
13 11 10 8 10 12 13
输出样例
5
9
16
4 1 E
并查集
四个方向的偏移量和对应的字母按“左上右下”的顺序存下来

读入数据的时候直接用并查集合并连通区域

就最后判断拆哪面墙的时候的优先级要求有点离谱

时间复杂度 O(nm)
C++ 代码
#include<cstdio>
#include<cstring>
#include<algorithm>

using namespace std;

const int N = 2510, M = 100010;

int p[N], sz[N];
int find(int x)
{
    if(p[x] != x)  p[x] = find(p[x]);
    return p[x];
}

int n, m;
int dx[4] = {0, -1, 0, 1};
int dy[4] = {-1, 0, 1, 0};
char dir[5] = "WNES";

int main()
{
    scanf("%d%d", &m, &n);
    for(int i = 0; i < n * m; i ++)  p[i] = i, sz[i] = 1;

    for(int i = 0; i < n; i ++)
        for(int j = 0; j < m; j ++)
        {
            int s;
            scanf("%d", &s);

            int a = i * m + j;
            for(int k = 0; k < 4; k ++)
                if(!(s >> k & 1))
                {
                    int x = i + dx[k], y = j + dy[k], b = x * m + y;
                    int pa = find(a), pb = find(b);
                    if(pa != pb)
                    {
                        sz[pa] += sz[pb];
                        p[pb] = pa;
                    }
                }
        }

    int cnt = 0, res = 0;
    for(int i = 0; i < n * m; i ++)
        if(p[i] == i)
        {
            cnt ++;
            res = max(res, sz[i]);
        }
    printf("%d\n%d\n", cnt, res);

    res = 0;
    int rx, ry;
    char rd;
    for(int j = 0; j < m; j ++)                 //  从西往东
        for(int i = n - 1; i >= 0; i --)        //  从南到北
        {
            int a = i * m + j;
            for(int k = 1; k <= 2; k ++)        //  北、东
            {
                int x = i + dx[k], y = j + dy[k];
                if(x >= 0 and x < n and y >= 0 and y < m)
                {
                    int b = x * m + y;
                    int pa = find(a), pb = find(b);
                    if(pa != pb and sz[pa] + sz[pb] > res)
                    {
                        res = sz[pa] + sz[pb];
                        rx = i, ry = j, rd = dir[k];
                    }
                }
            }
        }
    printf("%d\n%d %d %c\n", res, rx + 1, ry + 1, rd);
    return 0;
}
```

```
1 入门
1.1 介绍
1.2 提交解决方案、任务类型、特殊问题
1.3 完全搜索
1.4 贪心、制定解决方案
1.5 更多搜索技巧
1.6 二进制数
2 更大的挑战
2.1 图论、Flood Fill算法
2.2 数据结构、动态规划
2.3 习题
2.4 最短路
3 更巧妙的技法
3.1 生成树
3.2 背包问题
3.3 欧拉通路
3.4 计算几何
4 高级算法和高难度训练
4.1 优化
4.2 网络流 - network flow!!!
4.3 高精度计算
4.4 习题
5 严峻的挑战
5.1 凸包
5.2 习题
5.3 启发法
5.4 习题
5.5 习题
6 竞赛练习
6.1 习题
6.2 习题
6.3 习题
6.4 习题
6.5 习题

```



```cpp
#include<cstdio>
#include<cstring>

const int N = 100010, mod = 47;

char a[N], b[N];

int main()
{
    scanf("%s%s", a, b);

    int x = 1, y = 1;
    for(int i = 0; a[i]; i ++)  x = x * (a[i] - 'A' + 1) % mod;
    for(int i = 0; b[i]; i ++)  y = y * (b[i] - 'A' + 1) % mod;
    if(x == y)  puts("GO");
    else  puts("STAY");
    return 0;
}

// yxc
#include <iostream>

using namespace std;

int get(string s)
{
    int res = 1;
    for (auto c: s)
        (res *= c - 'A' + 1) %= 47;
    return res;
}

int main()
{
    string a, b;
    cin >> a >> b;

    if (get(a) == get(b)) puts("GO");
    else puts("STAY");

    return 0;
}

```

```cpp
O(n3)O(n3)
#include <cstdio>
#include <cstring>

int n,res[11];
char str[11][16];
char s[16];

int main()
{
    scanf("%d",&n);
    for(int i=1;i<=n;i++)scanf("%s",str[i]);
    for(int i=1;i<=n;i++)
    {
        int cnt,id,x;
        scanf("%s",s);
        for(id=1;strcmp(str[id],s);id++);
        scanf("%d%d",&x,&cnt);
        if(!cnt)continue;
        res[id]-=x-x%cnt;
        for(int k=0;k<cnt;k++)
        {
            scanf("%s",s);
            int ids;
            for(ids=1;strcmp(str[ids],s);ids++);
            res[ids]+=x/cnt;
        }
    }
    for(int i=1;i<=n;i++)printf("%s %d\n",str[i],res[i]);
    return 0;
}
O(n2) HashO(n2) Hash
#include <cstdio>
#include <cstring>

const int M=1000999;

int n,res[11];
int g[11],id[M];
char str[11][16],s[16];

int get(char str[])
{
    int res=0;
    for(int i=0;str[i];i++)res=(res*31+str[i])%M;
    return res;
}

int main()
{
    scanf("%d",&n);
    for(int i=1;i<=n;i++)
    {
        scanf("%s",str[i]);
        id[get(str[i])]=i;
    }
    for(int i=1;i<=n;i++)
    {
        int cnt,x;
        scanf("%s%d%d",s,&x,&cnt);
        if(!cnt)continue;
        res[id[get(s)]]-=x-x%cnt;
        for(int k=0;k<cnt;k++)
        {
            scanf("%s",s);
            res[id[get(s)]]+=x/cnt;
        }
    }
    for(int i=1;i<=n;i++)printf("%s %d\n",str[i],res[i]);
    return 0;
}

#include<bits/stdc++.h>
using namespace std;
int n;
const int N = 11;
string name[N];
int main(){
    cin>>n;
    map<string,int>v;
    for(int i=1;i<=n;i++){
        cin>>name[i];
        v[name[i]] = 0;
    }
    for(int i=1;i<=n;i++){
        string x;
        cin>>x;
        int m,t;
        cin>>m>>t;
        if(t == 0 || m == 0) continue;
        int k = m/t;
        for(int j=1;j<=t;j++){
            string a;
            cin>>a;
            v[a]+=k;
        }
        v[x]-=k*t;
    }
    for(int i=1;i<=n;i++) cout<<name[i]<<" "<<v[name[i]]<<endl;
    return 0;
}

//yxc
#include <iostream>
#include <cstring>
#include <algorithm>

using namespace std;

const int N = 10;

int n;
string names[N];
unordered_map<string, int> person;

int main()
{
    cin >> n;
    for (int i = 0; i < n; i ++ ) cin >> names[i];

    for (int i = 0; i < n; i ++ )
    {
        string name;
        int money, cnt;
        cin >> name >> money >> cnt;
        if (cnt)
        {
            person[name] -= money / cnt * cnt;
            for (int j = 0; j < cnt; j ++ )
            {
                string p;
                cin >> p;
                person[p] += money / cnt;
            }
        }
    }

    for (int i = 0; i < n; i ++ )
        cout << names[i] << ' ' << person[names[i]] << endl;

    return 0;
}


```

```cpp
#include<cstdio>
#include<cstring>

using namespace std;

int cnt[7];

int days[13] = {0, 31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31};
int get(int yy, int mm)
{
    if(mm == 2 and (yy % 4 == 0 and yy % 100 != 0 or yy % 400 == 0))  return 29;
    return days[mm];
}

int main()
{
    int n;
    scanf("%d", &n);

    int week = 13 % 7;
    for(int year = 1900; year <= 1900 + n - 1; year ++)
    {
        for(int month = 1; month <= 12; month ++)
        {
            cnt[week] ++;
            week = (week + get(year, month)) % 7;
        }
    }
    printf("%d ", cnt[6]);
    for(int i = 0; i < 6; i ++)  printf("%d ", cnt[i]);
    return 0;
}

```
