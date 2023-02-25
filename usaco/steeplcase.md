# steeplcase?

[https://blog.csdn.net/weixin\_30635053/article/details/98709859](https://blog.csdn.net/weixin\_30635053/article/details/98709859) steeplecase I

```
Acw329.围栏障碍训练场
思路：
算是比较明显的dpdp，小牛每次碰到一个围栏只有两种选择，第一个是向左走，第二个是向右走。

所以我们设f(i,0/1)f(i,0/1)表示在第ii个障碍，小牛碰到当前障碍向左走或者向右走到终点的答案。

那么答案显然是min(f(n,0)+abs(l(n)−s),f(n,1)+abs(r(n)−s))min(f(n,0)+abs(l(n)−s),f(n,1)+abs(r(n)−s))。

那么转移方程呢？假设说我现在在第ii个障碍上，我需要找到我往下走第一个碰到的围栏jj的情况来更新ii的答案。

如何快速找到下面的围栏？

我们需要快速的确定端点和以前的线段的交点，同时更新本次的左右端点的覆盖。

线段树区间修改+单点查询可以快速完成。

#include<bits/stdc++.h>
using namespace std;
typedef long long ll;
const int maxn = 3e4+10;
const int base = 1e5+1;
const int m = base*2;
int n, s;
int l[maxn], r[maxn];
ll f[maxn][2];

struct SegmentTree
{
    int l, r;
    int id;
    int laz;
    #define lson (p<<1)
    #define rson (p<<1|1)
    #define l(x) tree[x].l
    #define r(x) tree[x].r
    #define id(x) tree[x].id
    #define laz(x) tree[x].laz
}tree[m<<2];

void spread(int p)
{
    if(laz(p))
    {
        laz(lson) = laz(rson) = 1;
        id(lson) = id(rson) = id(p);
        laz(p) = 0;
    }
}

void build(int p, int l, int r)
{
    l(p) = l, r(p) = r;
    if(l == r) return;
    int mid = (l+r)>>1;
    build(lson, l, mid); build(rson, mid+1, r);
}

int ask(int p, int x)
{
    if(l(p) == r(p)) return id(p);
    int mid = (l(p)+r(p))>>1;
    spread(p);
    if(x <= mid) return ask(lson, x);
    else return ask(rson, x);
}

void change(int p, int L, int R, int v)
{
    if(L <= l(p) && r(p) <= R) {
        id(p) = v;
        laz(p) = 1;
        return;
    }
    spread(p);
    int mid = (l(p)+r(p))>>1;
    if(L <= mid) change(lson, L, R, v);
    if(mid < R) change(rson, L, R, v);
}

int main()
{
    scanf("%d%d", &n, &s);
    s += base;
    build(1, 1, m);
    //直接走回原点
    l[0] = r[0] = base;
    for(int i = 1; i <= n; i++)
    {
        scanf("%d%d", &l[i], &r[i]);
        l[i] += base; r[i] += base;
        int lv = ask(1, l[i]);
        int rv = ask(1, r[i]);
        f[i][0] = min(f[lv][0]+abs(l[lv]-l[i]), f[lv][1]+abs(r[lv]-l[i]));
        f[i][1] = min(f[rv][0]+abs(l[rv]-r[i]), f[rv][1]+abs(r[rv]-r[i]));
        change(1, l[i], r[i], i);
    }

    int ans = min(f[n][0]+abs(l[n]-s), f[n][1]+abs(r[n]-s));
    cout << ans << endl;
    return 0;
}

作者：zhaoxiaoyun
链接：https://www.acwing.com/solution/content/8857/

```
