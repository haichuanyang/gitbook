# day13 pattern finding

p754 matrix square

```
如题看看就好....
找了一个比较简单的规律....

#include <iostream>
#include <algorithm>

using namespace std;

int n;

int main()
{
    while (cin >> n)
    {
        for (int i = 0; i < n; i ++ )
        {
            for(int j = 0; j < n; j ++ )
                cout << abs(i - j) + 1 << ' ';  // 规律

            cout << endl;
        }

        if (n) cout << endl;
    }

    return 0;
} 

作者：ltk
链接：https://www.acwing.com/solution/content/6638/

```

```
时间复杂度:O(n2)如果学过线性代数的话,这道题的规律就显而易见了,可以当作对称矩阵来解题对于对称矩阵:
A=⎛⎝⎜⎜⎜⎜a11a21⋮an1a12a22⋮an2⋯⋯⋮⋯a1na2n⋮ann⎞⎠⎟⎟⎟⎟有aij=aji时间复杂度:O(n2)如果学过线性代数的话,这道题的规律就显而易见了,可以当作对称矩阵来解题对于对称矩阵:A=(a11a12⋯a1na21a22⋯a2n⋮⋮⋮⋮an1an2⋯ann)有aij=aji
对称矩阵百度百科

写法一(上三角形)
#include<iostream>
using namespace std;
const int N=110;
int a[N][N];
int main()
{
    int n;
    scanf("%d",&n);
    int k=1;
    while(n)
    {
        for(int i=1;i<=n;i++)
        {
            for(int j=i;j<=n;j++)//上三角形
            {
                if(i-1 != j-1) a[i-1][j-1]=a[j-1][i-1]=k;
                else a[i-1][j-1]=k;
                k++;
            }
            k=1;
        }
        for(int i=1;i<=n;i++)
        {
            for(int j=1;j<=n;j++)
            {
                printf("%d ",a[i-1][j-1]);
            }
            printf("\n");
        }
        printf("\n");
        scanf("%d",&n);
    }
}
写法二(上三角形+下三角形)
#include<iostream>
using namespace std;
int main()
{
    int n;
    int k;
    scanf("%d",&n);
    while(n)
    {
        for(int i=1;i<=n;i++)
        {
            k=i;
            for(int j=1;j<=i;j++)//下三角形
            {
                printf("%d ",k);
                k--;
            }
            k=2;
            for(int j=i+1;j<=n;j++)//上三角形
            {
                printf("%d ",k);
                k++;
            }
            printf("\n");
        }
        printf("\n");
        scanf("%d",&n);
    }
}
写法三(下三角形)
#include<iostream>
using namespace std;
const int N=110;
int a[N][N];
int main()
{
    int n;
    scanf("%d",&n);
    int k;
    while(n)
    {
        for(int i=1;i<=n;i++)
        {
            k=i;
            for(int j=1;j<=i;j++)//下三角形
            {
                if(i-1 != j-1) a[i-1][j-1]=a[j-1][i-1]=k;
                else a[i-1][j-1]=k;
                k--;
            }
        }
        for(int i=1;i<=n;i++)
        {
            for(int j=1;j<=n;j++)
            {
                printf("%d ",a[i-1][j-1]);
            }
            printf("\n");
        }
        printf("\n");
        scanf("%d",&n);
    }
}

作者：Struggle
链接：https://www.acwing.com/solution/content/30867/

```
