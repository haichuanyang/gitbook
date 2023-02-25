---
description: >-
  char is a data type in c++; no need to use char[2] for single characters such
  as 'E' or 'N'
---

# bronze stuck in rut

```
#include<bits/stdc++.h>
using namespace std;
struct cow{int x,y;int s,i;bool d;}N[52],E[52];
int n,cnt1=0,cnt2=0,ct1=0,ct2=0;char f;
bool cmp1(cow x,cow y){return x.i<y.i;}
bool cmp2(cow x,cow y){return x.y<y.y;}
bool cmp3(cow x,cow y){return x.x<y.x;}
int main(){
    scanf("%d",&n);
    for(int i=0;i<n;++i){
        cin>>f;
        if(f=='E')scanf("%d%d",&E[cnt1].x,&E[cnt1].y),E[cnt1].s=-1,E[cnt1++].i=i;
        else scanf("%d%d",&N[cnt2].x,&N[cnt2].y),N[cnt2].s=-1,N[cnt2].d=1,N[cnt2++].i=i;
    }
    sort(E,E+cnt1,cmp2);
    sort(N,N+cnt2,cmp3);
    for(int i=0;i<cnt1;++i){
        for(int j=0;j<cnt2;++j){
            if(N[j].x>E[i].x&&N[j].y<E[i].y){
                if(E[i].y-N[j].y<N[j].x-E[i].x&&N[j].d){E[i].s=N[j].x-E[i].x;break;}
                else if(E[i].y-N[j].y>N[j].x-E[i].x&&N[j].d)N[j].d=0,N[j].s=E[i].y-N[j].y;
            }
        }
    }
    sort(E,E+cnt1,cmp1);
    sort(N,N+cnt2,cmp1);
    for(int i=0;i<n;++i){
        if(ct1<cnt1&&E[ct1].i==i){if(E[ct1].s==-1)puts("Infinity");else printf("%d\n",E[ct1].s);++ct1;}
        else{if(N[ct2].s==-1)puts("Infinity");else printf("%d\n",N[ct2].s);++ct2;}
    }
    return 0;
}
```
