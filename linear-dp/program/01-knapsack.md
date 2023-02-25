---
description: >-
  01 knapsack and complete knapsack are essentially the same thing, just with
  opposite orders of executing the volume iteration loop
---

# 01 and complete knapsack - sth wrong; can't use

Problem: Given N items each with volume v\[i] and value w\[i], a knapsack with volume M. What is the maximum value that can be put into the knapsack?

Naive solution: each item can either be chosen or not chosen, with a total of 2^N possibilities. Looping them all and calculate the value. Choose the maximum value. Complexity 2^N exponential

Linear dynamic programming: Consider in order each of the N items, regarding whether each one should be chosen for the knapsack or not. At any point of item i, use the first i items which have been analyzed as the first dimension, and their total volume j and the second dimension. Define f\[i]\[j] to be the max value so far with i items and volume j. Consider the transition from i-1 to i, based on whether the i-th item is chosen or not, f\[i]\[j] can be derived in two ways:

not choosing the i-th item: f\[i-1,j]

choosing the i-th item: f\[i-1,j-v\[i]]+w\[i] if j>=v\[i]

initial condition: f\[0,0]=0, and other f\[i,j]=-INF since we are getting max

target would be max{F\[N]\[j]} for j from 0 to M

```cpp
memset(f,0xcf,sizeof(f));
f[0][0]=0;
for(int i=1;i<=n;i++){
    for (int j=0;j<=m;j++) f[i][j]=f[i-1][j];
    for (int j=v[i];j<=m;j++) 
        f[i][j]=max(f[i][j],f[i-1][j-v[i]]+w[i]);
}
```

space and time complexity are both O(MN);

Looking at the transition equation above, we notice that at any loop level i, the state is only related to the i-1 loop level. So we only need to keep two adjacent levels of the f\[i]\[j] array in the for-loop at any time. This leads to using what's called "**rolling array**" for first optimization.  &#x20;

```cpp
int f[2][MAX_M+1];
memset(f,0xcf,sizeof(f));
f[0][0]=0;
for (int i=1;i<=n;i++){
    for (int j=0;j<=m;j++) f[i&1][j]=f[(i-1)&1][j];
    for (int j=v[i];j<=m;j++)
        f[i&1][j]=max(f[i&1][j], f[(i-1)&1][j-v[i]]+w[i];
}
int ans=0;
for (int j=0;j<=m;j++)ans=max(ans,f[n&1][j]);
```

i&1 is same as i%2; for odd i, i&1=1; for even i, i&1=0. So the DP states transit back and forth between f\[0]\[] and f\[1]\[].

space complexity is down to O(M)

Further notice that each loop of i did a copy of f\[i-1]\[] to f\[i]\[], which allows reducing the 2D array further down to 1D array f\[j], which represents the max value of using j volume when the outer loop is on the i-th item.

```
int f[MAX_M+1];
memset(f,0xcf,sizeof(f));
f[0]=0;
for (int i=1;i<=n;i++)
    for(int j=m;j>=v[i];j--) //backward looping
        f[j]=max(f[j],f[j-v[i]]+w[i]);
int ans=0;
for (int j=0;j<=m;j++) ans=max(ans,f[j]);
```

Note the reversed order looping of j. It means at any j:



1\. the latter part of the array f\[j\~M] is already in the i-th state, ie already considered the placement of the i-th item;

2\. the front part of the array f\[0\~j-1] is still at the stage of i-1, ie the i-th item hasn't been updated yet.

The reversed looping of j means we are using the f\[i-1]\[j] to get f\[i]\[j] values, which is required for the linear DP and makes sure the i-th item could be chosen only once. Going the other way (from low to high) would mean the i-th item could be used more than once.

Summary: **01 knapsack using 1D array requires using reversed decreasing order looping of the volume iteration.**



For complete knapsack problem, where the number of an item is unlimited, one can just use the increasing order of volume looping to allow for unlimited use of any i-th item.

```
int f[MAX_M+1];
memset(f,0xcf,sizeof(f));
f[0]=0;
for (int i=1;i<=n;i++)
    for(int j=v[i];j<=m;j++) //forward looping
        f[j]=max(f[j],f[j-v[i]]+w[i]);
int ans=0;
for (int j=0;j<=m;j++) ans=max(ans,f[j]); //sth wrong here - no need for loop?
```

Summary: **complete knapsack using 1D array requires using forward increasing order looping of the volume iteration.**

****

01 knapsack and complete knapsack are essentially the same thing, just with opposite orders of executing the volume iteration loop. The order of the volume looping determines how many times an item can be used.

****
