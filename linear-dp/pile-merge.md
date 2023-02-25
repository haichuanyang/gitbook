---
description: pile merging/segment DP needs prefix sum
---

# pile merge/segment DP

set is ways to merge piles i to j into one; attribute is min transition - consider penultimate merge, s=j-i+1 1, s; 2, s-1; 3, s-2; ....s-1, 1;

f\[i,j] = f\[i,k]+f\[k+1,j]+s\[j]-s\[i-1] k=i, to j-1 O(n^3)

ans=f\[1,n]

```
//segment length loop from small to big

s[i] +=s[i-1];  /get prefix sum first

for (int len=2;len<=n;len++) //segment length
    for (int i=1;i+len-1<=n;i++){ // segment left end
        int l=i,r=i+len-1;
        f[l][r]=1e8;
        for (int k=1;k<r;k++)
            f[l][r]=min(f[l][r], f[l][k]+f[k+1][r]+s[r]-s[l-1]);

    }
ans=f[1][n]
```

for segment DP, first loop segment length, second loop segment left end&#x20;
