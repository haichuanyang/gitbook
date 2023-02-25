---
description: bottom up is easier than up down
---

# number triangle

```cpp
#include<bits/stdc++.h>
using namespace std;

const int N=510,INF=0x3f3f3f3f;
int f[N][N];
int a[N][N];

int main(){
    int n;
    cin>>n;

    for(int i=1;i<=n;i++){
        for(int j=1;j<=i;j++){
            cin>>a[i][j];
        }
    }

    for(int i=1;i<=n;i++){             
        for(int j=0;j<=i+1;j++){          //因为有负数，所以应该将两边也设为-INF
            f[i][j]=-INF;
        }
    }

    f[1][1]=a[1][1];
    for(int i=2;i<=n;i++){
        for(int j=1;j<=i;j++){
            f[i][j]=a[i][j]+max(f[i-1][j-1],f[i-1][j]);
        }
    }

    int res=-INF;
    for(int i=1;i<=n;i++) res=max(res,f[n][i]);
    cout<<res<<endl;
}
也可以倒序dp，更简单些，因为倒序不需要考虑边界问题

#include<bits/stdc++.h>
using namespace std;

const int N=510;
int f[N][N];
int n;

int main(){
    cin>>n;
    for(int i=1;i<=n;i++){
        for(int j=1;j<=i;j++){
            cin>>f[i][j];
        }
    }

    for(int i=n;i>=1;i--){
        for(int j=i;j>=1;j--){
            f[i][j]=max(f[i+1][j],f[i+1][j+1])+f[i][j];
        }
    }
    cout<<f[1][1]<<endl;
}

作者：TaoZex
链接：https://www.acwing.com/solution/content/3485/

```

```
//bottom up is much easier
#include<iostream>
using namespace std;
const int N = 503;
int n;
int a[N][N];
int f[N];

int main()
{
    cin>>n;
    for(int i = 1;i<=n;i++)
        for(int j = 1;j<=i;j++)
            cin>>a[i][j];
    for(int j = 1;j<=n;j++)
        f[j] = a[n][j];
    for(int i = n - 1;i>=1;i--)
        for(int j = 1;j<=i;j++)
            f[j] = a[i][j] + max(f[j],f[j+1]);

    cout << f[1] << endl;
    return 0;
}
```

#### top down must initialize to -INF; end loop for answer

```
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 510, INF = 1e9;

int n;
int a[N][N];
int f[N][N];

int main()
{
    scanf("%d", &n);
    for (int i = 1; i <= n; i ++ )
        for (int j = 1; j <= i; j ++ )
            scanf("%d", &a[i][j]);

    for (int i = 0; i <= n; i ++ )
        for (int j = 0; j <= i + 1; j ++ )
            f[i][j] = -INF;

    f[1][1] = a[1][1];
    for (int i = 2; i <= n; i ++ )
        for (int j = 1; j <= i; j ++ )
            f[i][j] = max(f[i - 1][j - 1] + a[i][j], f[i - 1][j] + a[i][j]);

    int res = -INF;
    for (int i = 1; i <= n; i ++ ) res = max(res, f[n][i]);

    printf("%d\n", res);
    return 0;
}
```
