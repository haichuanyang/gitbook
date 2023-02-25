---
description: >-
  state representation; state space; set; recursion DP same as memoization
  search
---

# linear DP

```cpp
// LIS II
#include<iostream>
#include<algorithm>
#include<vector>
using namespace std;
int main(void) {
    int n; cin >> n;
    vector<int>arr(n);
    for (int i = 0; i < n; ++i)cin >> arr[i];

    vector<int>stk;//模拟堆栈
    stk.push_back(arr[0]);

    for (int i = 1; i < n; ++i) {
        if (arr[i] > stk.back())//如果该元素大于栈顶元素,将该元素入栈
            stk.push_back(arr[i]);
        else//替换掉第一个大于或者等于这个数字的那个数
            *lower_bound(stk.begin(), stk.end(), arr[i]) = arr[i];
    }
    cout << stk.size() << endl;
    return 0;
}
/*
例 n: 7
arr : 3 1 2 1 8 5 6

stk : 3

1 比 3 小
stk : 1

2 比 1 大
stk : 1 2

1 比 2 小
stk : 1 2

8 比 2 大
stk : 1 2 8

5 比 8 小
stk : 1 2 5

6 比 5 大
stk : 1 2 5 6

stk 的长度就是最长递增子序列的长度

*/

作者：233
链接：https://www.acwing.com/solution/content/3783/

```

### &#x20;&#x20;

### LIS - penultimate is key

f\[i] is longest subsequenece ending in i;

transition: consider the previous element before i, it can be

0,1,2,3...i-1   = use j to represent it, it will be f\[j]

f\[i]=max{f\[j]+1} where j=0,1,2...i-1

complexity=state number times transition number O(n^2)

use penultimate number for considering transition

```
for (int i=1;i<=n;i++){
    f[i]=1; //just a[i] itself length 1
    for(int j=1;j<i;j++) //no 0
        if(a[j]<a[i]) f[i]=max(f[i],f[j]+1);
}
int res=0;
for (inti=1;i<=n;i++) res=max(res,f[i]);
```

#### to get whole history of transition, using an array to keep track of choices:

```cpp
int g[N];
for (int i=1;i<=n;i++){
    f[i]=1; //just a[i] itself length 1
    for(int j=1;j<i;j++) //no 0
        if(a[j]<a[i]) //f[i]=max(f[i],f[j]+1);
            if(f[i]<f[j]+1) {f[i]=f[j]+1;g[i]=j;}
}
//int res=0;
//for (inti=1;i<=n;i++) res=max(res,f[i]);
int k=1;
for (int i=1;i<=n;i++)
    if(f[k]<f[i]) k=i;
printf("%d\n", f[k]); //length result

for(int i=0, len=f[k]; i<len;i++){
    //wrong here - k=g[i];
    printf("%d ", a[k]);
    k=g[k] //g[k] is previous choice leading to k
} 
//can only get reverse order??????
```

g\[k] stores the previous element leading to k; only one previous element is known at any time, so only reverse order is possible; can't get the proper order knowing only one previous element at a time!!

```cpp
#include<bits/stdc++.h>
using namespace std;
#define fir(i,a,b) for(int i=a; i<=b;i++)
const int N=1010;

int a[N], f[N], g[N];
int n,m;

int main(){
    cin>>n;
    for(int i=1;i<=n;i++)cin>>a[i];
    
    for (int i=1;i<=n;i++){
        f[i]=1; //just a[i] itself length 1
        for(int j=1;j<i;j++) //no 0
            if(a[j]<a[i]) //f[i]=max(f[i],f[j]+1);
                if(f[i]<f[j]+1) {f[i]=f[j]+1;g[i]=j;}
    }


    int k=1;
    for (int i=1;i<=n;i++)
        if(f[k]<f[i]) k=i;
    printf("%d\n", f[k]); //length result
    
    for(int i=0, len=f[k]; i<len;i++){
        //k=g[i];
        printf("%d ", a[k]);
        k=g[k];
    }

}
```

