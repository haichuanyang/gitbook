# steeplecase II



```
正解
扫描线+set裸题（好像还在ZROI讲过，可能当时自闭了没听到）

直接对每一个端点按先x后y排序

依次加进去，如果当前点是起始点，就把它在set里面lower_bound一下，找到与它相邻的两条线段，判断一下是否相交，然后把当前线段加进set

如果当前点是终止点，就把它弹出set，并判一下与它相邻的两条线段是否相交

找到了第一组相交线段时就直接退出，用上面的方法判断一下谁是解。

std：

#include <iostream>
#include <fstream>
#include <vector>
#include <set>
#include <algorithm>
using namespace std;
typedef long long LL;
 
int N;
double x;
struct Point { LL x, y; int segindex; };
struct Segment { Point p, q; int index; };
 
bool operator< (Point p1, Point p2) { return p1.x==p2.x ? p1.y<p2.y : p1.x<p2.x; }
 
// Intersection testing (here, using a standard "cross product" trick)
int sign(LL x) { if (x==0) return 0; else return x<0 ? -1 : +1; }
int operator* (Point p1, Point p2) { return sign(p1.x * p2.y - p1.y * p2.x); }
Point operator- (Point p1, Point p2) { Point p = {p1.x-p2.x, p1.y-p2.y}; return p; }
bool isect(Segment &s1, Segment &s2)
{
  Point &p1 = s1.p, &q1 = s1.q, &p2 = s2.p, &q2 = s2.q;
  return ((q2-p1)*(q1-p1)) * ((q1-p1)*(p2-p1)) >= 0 && ((q1-p2)*(q2-p2)) * ((q2-p2)*(p1-p2)) >= 0;
}
 
// What is the y coordinate of segment s when evaluated at x?
double eval(Segment s) {
  if (s.p.x == s.q.x) return s.p.y;
  return s.p.y + (s.q.y-s.p.y) * (x-s.p.x) / (s.q.x-s.p.x);
}
bool operator< (Segment s1, Segment s2) { return s1.index != s2.index && eval(s1)<eval(s2); }
bool operator== (Segment s1, Segment s2) { return s1.index == s2.index; }
 
int main(void)
{
  ifstream fin ("jump.in");
  vector<Segment> S;
  vector<Point> P;
 
  fin >> N;
  for (int i=0; i<N; i++) {
    Segment s;
    fin >> s.p.x >> s.p.y >> s.q.x >> s.q.y;
    s.index = s.p.segindex = s.q.segindex = i;
    S.push_back(s);
    P.push_back(s.p); P.push_back(s.q);
  }
  sort (P.begin(), P.end());
 
  set<Segment> active;
  int ans1, ans2; // sweep across scene to locate just one intersecting pair...
  for (int i=0; i<N*2; i++) {
    ans1 = P[i].segindex; x = P[i].x;
    auto it = active.find(S[ans1]);
    if (it != active.end()) {
      // segment already there -- we've reached its final endpoint.  check intersection with the
      // pair of segments that becomes adjacent when this one is deleted.
      auto after = it, before = it; after++;
      if (before != active.begin() && after != active.end()) {
	before--;
	if (isect(S[before->index], S[after->index]))
	  { ans1 = before->index; ans2 = after->index; break; }
      }
      active.erase(it);
    }
    else {
      // new segment to add to active set.
      // check for intersections with only the segments directly above and below (if any)
      auto it = active.lower_bound(S[ans1]);
      if (it != active.end() && isect(S[it->index], S[ans1])) { ans2 = it->index; break; }
      if (it != active.begin()) { it--; if (isect(S[it->index], S[ans1])) { ans2 = it->index; break; } }
      active.insert(S[ans1]);
    }
  }
 
  if (ans1 > ans2) swap (ans1, ans2);
  int ans2_count = 0;
  for (int i=0; i<N; i++)
    if (S[i].index != ans2 && isect(S[i], S[ans2])) ans2_count++;
 
  ofstream fout ("jump.out");
  fout << (ans2_count > 1 ? ans2+1 : ans1+1) << "\n";
  return 0;
}

```

```
题解 P5428 【[USACO19OPEN]Cow Steeplechase II】
posted on 2019-08-16 07:38:27 | under 题解 |  1  
这道题一看可知是要求出相连的的最前面两个，因为最多3个连在一起，甚至有时候3个也不行，所以求出相连两条即可退出。

首先想到n^{2}复杂度做法，但明显超时，需要优化，所以采取扫描线进行优化至nlog(n)。

现将每个点（注意，分开了，但要标记属于那条线）按照x轴从小到大排序，x坐标相同的按y坐标从大到小。用set进行存储，

在左端点出现后，让这条线进入集合，与两边的线进行检查是否相交，注意，set有排序功能，所以你要重载小于，要注意比较方式，求出相邻两条边在同一x坐标y坐标大小，因为这个x坐标会随扫描线移动。

在右端点出现后，让这条线出集合，因为扫描线已经扫过了。这是还要检查一遍左右两条线是否相交

，因为左右可能变化了。

注:这道题需熟练用于set的各种函数。

比较也需要技巧：对立叉乘实验



p1和p2是两条线段，且左端点处于同一位置（0,0）右端点分别为(x1,y1),(x2,y2),

线段p1和p2的叉乘结果为p1Xp2=x1y2-x2y1

如若结果大于0，则p2在p1的顺时针180度以内，如果小于零，在逆时针180度以内。

要判断是否交叉，可以取p1线上一端点连上p2线的两个端点，得到两条条线，再分别与p1进行叉乘实验。

再可以取p2线上一端点连上p1线的两个端点，得到两条条线，再分别与p2进行叉乘实验。

就可以知道是否p1两点在p2两边,p2两点在p1两边。

就能得到答案了 上代码：

#include<cstdio>
#include<algorithm>
#include<set>
using namespace std;
#define LL long long
#define M 100005
void read(LL &x){
    x=0;LL f=1;char c=getchar();
    while(c<'0'||c>'9'){
        if(c=='-')
            f=-f;
        c=getchar();
    } 
    while(c>='0'&&c<='9'){
        x=(x<<1)+(x<<3)+c-'0';
        c=getchar();
    }
    x*=f;
    return ;
}
double x;
LL n,ans1,ans2;
struct edge{
    LL x,y,id;
    bool operator <(edge b)const{
        return x==b.x?y<b.y:x<b.x;
    }
}t[M<<1];
struct node{
    edge a,b;
    LL id;
    bool operator ==(node k)const{
        return  id==k.id;
    }
}p[M];
double eval(node op){
    if(op.a.x==op.b.x)
        return op.a.y;
    return op.a.y+(op.b.y-op.a.y)*(x-op.a.x)/(op.b.x-op.a.x);
}
bool operator <(node u,node v){
    return u.id!=v.id&&eval(u)<eval(v);
}
set<node>s;
LL sum[M];
LL sign(LL x){
    if(!x)
        return 0ll;
    if(x>0)
        return +1ll;
    return -1ll;
}
LL operator* (edge u,edge v){
    return sign(u.x*v.y-u.y*v.x);
}
edge operator- (edge u,edge v){
    edge k1={u.x-v.x,u.y-v.y};
    return k1; 
}
bool cmp(node u,node v){
    edge &a1=u.a,&a2=v.a,&b1=u.b,&b2=v.b;
    return ((b2-a1) * (b1-a1)) * ((b1-a1) * (a2-a1))>=0&&((b1-a2) * (b2-a2)) * ((b2-a2) * (a1-a2))>=0;
}
int main(){
    read(n);
    for(int i=1;i<=n;i++){
        read(p[i].a.x),read(p[i].a.y),read(p[i].b.x),read(p[i].b.y);
        t[(i<<1)-1]=p[i].a;
        t[i<<1]=p[i].b;
        p[i].a.id=p[i].b.id=p[i].id=t[(i<<1)-1].id=t[i<<1].id=i;
    }
    sort(t+1,t+1+(n<<1));
    for(int i=1;i<=n<<1;i++){
        set<node>::iterator it;
        LL k=t[i].id;
        ans1=k;
        x=t[i].x;
        it=s.find(p[k]);
        if(it!=s.end()){
            set<node>::iterator a_it=it,b_it=it;b_it++;
            if(a_it!=s.begin()&&b_it!=s.end()){
                a_it--;
                if(cmp(p[a_it->id],p[b_it->id])){
                    ans1=a_it->id,ans2=b_it->id;
                    break;
                }
            }
            s.erase(it);
        }else{
            set<node>::iterator new_it=s.lower_bound(p[k]);
            if(new_it!=s.end()&&cmp(p[new_it->id],p[k])){
                ans2=new_it->id;
                break;
            }
            if(new_it!=s.begin()){
                new_it--;
                if(cmp(p[new_it->id],p[k])){
                    ans2=new_it->id;
                    break;
                }
            }
            s.insert(p[k]);
        }
    }
    LL ans=0;
    if(ans1>ans2)
        swap(ans1,ans2);
    for(int i=1;i<=n;i++)
        if(p[i].id!=ans2&&cmp(p[i],p[ans2]))
            ans++;
    printf("%lld",ans>1?ans2:ans1);
    return 0;
}
```

[https://www.luogu.com.cn/blog/wyl123456/solution-p5428](https://www.luogu.com.cn/blog/wyl123456/solution-p5428) sol



[https://blog.csdn.net/c20180602\_csq/article/details/99655074](https://blog.csdn.net/c20180602\_csq/article/details/99655074) sol
