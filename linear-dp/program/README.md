---
description: dp
---

# knapsack

**state representation = set + attribute**

**state computation is set partition -> state transition equation**

loop is always items/groups first, volumes second, choices third

initialization:&#x20;

method 1. f\[i]=0 answer is just f\[m]

method 2. f\[0]=0, f\[i]=-INF for i!=0, then answer is max{f\[0....m]}

### 9. group knapsack - i groups each can only take 1 item

set:  choosing from the first i groups, with volume less than j

attribute: max value = f\[i,j]

state transition f\[i,j] = max(f\[i-1,j], f\[i-1, j-v\[i, k]]+w\[i,k])

kth item from group i has v\[i,k] and w\[i,k]

group knapsack video 6:06 explains looping:

loop -&#x20;

if using the previous level values, loop from big to small;

if using the current level values, loop from small to big

reason is iterating from big to small downward will leave the smaller part of the values unchanged thus remaining as previous loop-level values; this makes using 1D array possible.

```
#include <iostream>
#include <algorithm>
using namespace std;
const int N=110;

int n,m;
int v[N][N],w[N][N],s[N];
int f[N];

int main(){
    cin>>n>>m;
    for (int i=1;i<=n;i++){
        cin>>s[i];
        for(int j=0;i<s[i];j++)
            cin>>v[i][j]>>w[i][j];  
    }
    //input above
    
    for(int i=1;i<=n;i++) //groups for items
        for (int j=m;j>=0;j--) //volume from big to small
            for (int k=0;k<s[i];k++) //choice within group
                if(v[i][k]<=j)
                        f[j]=max(f[j],f[j-v[i][k]]+w[i][k]);
                    
                    
    cout<<f[m]<<endl;
    return 0;
     
}
```

TLE code - j should be i!!!!!! very hard to catch; ij confusion disaster

### 10. knapsack with dependency - hard

tree shape DP - it's a whole tree; plus group knapsack

set - all possible values of using i as root of its subtree

f\[i,j] = max value using item i subtree within volume j;&#x20;

every node i is a group which allows choosing only one maximum value; 0-1 knapsack&#x20;

state transition = tree recursion dfs + group knapsack

dfs() get all possible values  and volumes for each node - each volume makes a group

```
#include <iostream>
#include <algorithm>
using namespace std;
const int N=110;
int n,m;
int h[N],e[N],ne[N],idx;
int v[N],w[N],f[N][N];

void add(int a,int b){
    e[idx]=b,ne[idx]=h[a],h[a]=idx++;
}
void dfs(int u){
    for(int i=h[u];i!=-1;i=ne[i]){
        int son=e[i];
        dfs(son);
        for(int j=m-v[u];j>=0;j--) 
        //must choose u; so leave space for v[u]
        //using different volumes as groups
            for(int k=0;k<=j;k++)
                f[u][j]=max(f[u][j],f[u][j-k]+f[son][k]);
    }
    
    //handle root u volume below
    for(int i=m;i>=v[u];i--) f[u][j]=f[u][i-v[u]]+w[u];
    //if volume is available for v[u], then add its value
    
    for(int i=0;i<v[u];i++) f[u][i]=0;  
    //if volume of u is bigger than available, can't choose
}

int main(){
    memset(h,-1,sizeof h);
    cin>>n>>m;
    int root;
    for (int i=1;i<=n;i++){
        int p;
        cin>>v[i]>>w[i]>>p;
        if(p==-1)root=i;
        else add(p,i);
    } //input data
    
    dfs(root); //calculate
    
    cout<<f[root][m]<<endl;
    //may not use all of m volume
    return 0;
}
```

initialized to 0; meaning volume is at most m, may not use all of m volume.

### 11. count number of ways reaching maximum value

idea is open another array to count number of ways of getting volume j with max value



f\[j] means volume is exactly at j; initialize to -INF; f\[0]=0; g\[0]=1;

```

for(int i=0; i<n;i++)
int v,w;
cin>>v>>w;

for(int j=m;j>=v;j--){
    int t=max(f[j],f[j-v]+w);
    int s=0;
    if(t==f[j])s+=g[j];
    if(t==f[j-v]+w)s+=g[j-v];
    if(s>mod)s-=mod;
    f[j]=t;
    g[j]=s;   
}
//count number of ways at the same time of getting max

//count result for the final result max
int maxw=0;
for(int i=0;i<=m;i++) maxw=max(maxw,f[i]);
int res=0;
for(int i=0;i<=m;i++)
    if(maxw==f[i]){
        res+=g[i];
        if(res>=mod) res-=mod;
    }
    cout<<res<<endl;
    return 0;
```

### 12. output the result list of items giving max result

dictionary ordered first item

idea is to get the max value then reverse engineering the items list; must keep 2D array f\[i]\[j]

f\[n]\[m] == f\[n-1]\[m] -> not using n-1

f\[n]\[m] == f\[n-1,m-v\[i]]+w\[i] -> using n-1

could be both true which means using n-1 or not using it gives same result

iterate backwards - should choose item 1 if either way is the same

```
cin>>n>>m;
for(int i=1;i<=n;i++) cin>>v[i]>>w[i];

for(int i=n;i>=1;i--)
    for(int j=0;j<=m;j++) //order doesn't matter for 2D
    {
        f[i][j]=f[i+1][j];
        if(j>=v[i])f[i][j]=max(f[i][j],f[i+1][j-v[i]]+w[i]);
    }
    
    
int vol=m;
for(int i=1;i<=n;i++)
    if(f[i][vol]==f{i+1][vol-v[i]]+w[i]){
    cout<<i<<' ';vol-=v[i];
    }
```

### 8. 2D cost knapsack

![](<../../.gitbook/assets/2D knapsack.png>)

### 7. mix knapsack

![](<../../.gitbook/assets/mixed knapsack.png>)

### 6. multiple knapsack III sliding window with monotonic queue, largest range, hardest

![](<../../.gitbook/assets/knapsack highest with monotonic queue.png>)

### 5. multi knapsack with binary optimization

![](<../../.gitbook/assets/knapsack with binary optimization.png>)

### 4 multi knapsack basic - data range 100; can be treated as enlarged version of 01 knapsack, with added loop of s\[i]

```
#include<bits/stdc++.h>
using namespace std;
const int N=101;
int n,m;
int f[N];

int main(){
    cin>>n>>m;
    for(int i=0;i<n;i++){
        int v,w,s;
        cin>>v>>w>>s;
        
        for (int j=m;j>=0;j--)
            for (int k=1;k<=s && k*v<=j; k++)
                f[j]=max(f[j],f[j-k*v]+k*w);
    }
    cout<<f[m]<<endl;
    return 0;
}
```

typing save version:

```
#include <bits/stdc++.h>
using namespace std;
main()
{
    int dp[1005],n,t,v,w,s;
    cin>>n>>t;
    while(n--)
    {
    cin>>v>>w>>s;
    for(int i=1;i<=s;i++)
    for(int j=t;j>=w;j--)
    dp[j]=max(dp[j],dp[j-w]+v);
    }
    cout<<dp[t];
}
```
