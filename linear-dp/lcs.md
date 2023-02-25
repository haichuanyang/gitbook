# LCS

我觉得这题的状态分成两半考虑比较方便，按两个序列末尾的字符是不是相等来区分。



![问题分析.PNG](https://cdn.acwing.com/media/article/image/2020/02/06/28466\_6610da5048-%E9%97%AE%E9%A2%98%E5%88%86%E6%9E%90.PNG)

如果两个字符相等，就可以直接转移到f\[i-1]\[j-1]，不相等的话，两个字符一定有一个可以抛弃，可以对f\[i-1]\[j],f\[i]\[j-1]两种状态取max来转移。



![状态转移.PNG](https://cdn.acwing.com/media/article/image/2020/02/06/28466\_e2c0e13048-%E7%8A%B6%E6%80%81%E8%BD%AC%E7%A7%BB.PNG)

```cpp
#include <iostream>
using namespace std;
const int N = 1010;
int n, m;
char a[N], b[N];
int f[N][N];
int main() {
  cin >> n >> m >> a + 1 >> b + 1;
  for (int i = 1; i <= n; i++) {
    for (int j = 1; j <= m; j++) {
      if (a[i] == b[j]) {
        f[i][j] = f[i - 1][j - 1] + 1;
      } else {
        f[i][j] = max(f[i - 1][j], f[i][j - 1]);
      }
    }
  }
  cout << f[n][m] << '\n';
  return 0;
}

作者：yuechen
链接：https://www.acwing.com/solution/content/8111/

```



LCS - given 2 strings a and b, find the longest common subsequence

state sets - 2D for 2 strings

f\[i]\[j] - common subsequence using first i char of a and first j char of b, max length

transition - if including a\[i] or b\[j] (00, 01, 10,11)

f\[i]\[j] = max(f\[i-1]\[j-1], f\[i]\[j-1],f\[i-1]\[j], f\[i-1,j-1]+1}

f\[i]\[j-1] may or may not include a\[i] but it's an overset of including a\[i] set, for getting max, ok to use&#x20;

set is common subsequence using first i of a and first j of b attribute is max length

transition: a\[i] b\[j] use or not: 00 01 10 11

f\[i,j]=max f\[i-1,j-1], f\[i]\[j-1], f\[i-1]\[j],f\[i-1,j-1]+1

f\[i-1,j-1] is included as part of f\[i]\[j-1]! so no need

for max/min can overlap sets/allows repeating parts

```cpp
char a[N],b[N]; // not int a[N]!!!

for(int i=1;i<=n;i++)
    for (int j=1;j<=m;j++)
        {
            f[i][j]=max(f[i-1][j], f[i][j-1]);
            if(a[i]==b[j]) f[i][j]=max(f[i][j], f[i-1][j-1]+1);
        }
res=f[n][m];
```
