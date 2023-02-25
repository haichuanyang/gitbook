# lyd videos chap 5 DP

&#x20;state space - state set

[https://github.com/lydrainbowcat/tedukuri](https://github.com/lydrainbowcat/tedukuri)

DP - optimal substructure, subproblem repeat, no backwardation

DP must follow a certain order, and with a series of stages; **order and stage matters**

5201 01背包 + 完全背包 + 例题

5202 例题 Jury Compromise

include ways to print the path of choices -

lyd code [https://www.acwing.com/activity/content/activity\_person/content/49527/3/](https://www.acwing.com/activity/content/activity\_person/content/49527/3/)

```
#include<iostream>
#include<cstdio>
#include<cstring>
#include<algorithm>
using namespace std;
int n, m, d[205], p[205];
int f[21][801]; //向右平移400
bool choose[205][21][801];
void print(int i, int j, int k) {
    if (j==0) return;
    if (choose[i][j][k+400]) {
        print(i-1,j-1,k-(d[i]-p[i]));
        printf(" %d", i);
    } else {
        print(i-1,j,k);
    }
}
int main() {
    int T = 0;
    while (cin>>n>>m && n) {
        for(int i=1;i<=n;i++)
            scanf("%d%d",&p[i],&d[i]);
        memset(f, 0xcf, sizeof(f));
        f[0][0+400]=0;
        for(int i=1;i<=n;i++)
            for(int j=m;j;j--)
                for(int k=-20*m;k<=20*m;k++) {
                    choose[i][j][k+400] = false; // 默认不选
                    if (k-(d[i]-p[i])<-20*m || k-(d[i]-p[i])>20*m) continue;
                    if (f[j][k+400] < f[j-1][k-(d[i]-p[i])+400]+d[i]+p[i]) { // 选
                        f[j][k+400] = f[j-1][k-(d[i]-p[i])+400]+d[i]+p[i];
                        choose[i][j][k+400] = true;
                    }
                }
        int delta=1<<30;
        int raw_delta;
        int sum = 0;
        int ansk;
        for(int k=-20*m;k<=20*m;k++)
            if (f[m][k+400] >= 0 && (abs(k)<delta || abs(k)==delta && f[m][k+400]>sum)) // f[m][k]合法 
                delta=abs(k), raw_delta=k, sum=f[m][k+400], ansk=k;
        // d-p=raw_delta
        // d+p=sum
        printf("Jury #%d\n", ++T);
        printf("Best jury has value %d for prosecution and value %d for defence:\n", (sum-raw_delta)/2, (sum+raw_delta)/2);
        print(n, m, ansk);
        puts("\n");
    }
}

作者：lydrainbowcat
链接：https://www.acwing.com/activity/content/code/content/590760/

```

```
#old code

#include <iostream>
#include <cstdio>
#include <cstring>
#include <algorithm>
#include <vector>
using namespace std;
int f[25][805]; // 前i个人，选了j个，辩方总分与控方总分的差为k=-400~400（平移到0~800）
int d[205][25][805]; // 记路径还是要三维，不然后面的更新可能会覆盖掉之前的记录
int n, m, a[205], b[205], suma, sumb, T;
vector<int> c;

void get_path(int i, int j, int k) {
	if (j == 0) return;
	int last = d[i][j][k];
	get_path(last - 1, j - 1, k - (a[last] - b[last]));
	c.push_back(last);
	suma += a[last], sumb += b[last];
}

int main() {
	while (cin >> n >> m && n) {
		for (int i = 1; i <= n; i++) scanf("%d%d", &a[i], &b[i]);
		memset(f, 0xcf, sizeof(f)); // -INF
		f[0][400] = 0; // f[0][0]第三维平移400
		for (int i = 1; i <= n; i++) {
			for (int j = 0; j <= m; j++) // 不选i
				for (int k = 0; k <= 800; k++) d[i][j][k] = d[i - 1][j][k];
			for (int j = m; j; j--) // 选i
				for (int k = 0; k <= 800; k++) {
					if (k - (a[i] - b[i]) < 0 || k - (a[i] - b[i]) > 800) continue; // 超出范围
					if (f[j][k] < f[j - 1][k - (a[i] - b[i])] + a[i] + b[i]) {
						f[j][k] = f[j - 1][k - (a[i] - b[i])] + a[i] + b[i];
						d[i][j][k] = i;
					}
				}
		}
		int ans = 0;
		for (int k = 0; k <= 400; k++) {
			if (f[m][400 + k] >= 0 && f[m][400 + k] >= f[m][400 - k]) {
				ans = k + 400;
				break;
			}
			if (f[m][400 - k] >= 0) {
				ans = 400 - k;
				break;
			}
		}
		c.clear();
		suma = sumb = 0;
		get_path(n, m, ans);
		printf("Jury #%d\n", ++T);
		printf("Best jury has value %d for prosecution and value %d for defence:\n", suma, sumb);
		for (int i = 0; i < c.size(); i++) printf(" %d", c[i]);
		printf("\n\n");
	}
}
```

```
笔记

f[a1,a2,a3,a4,a5]表示每行分别站了a1,a2,a3,a4,a5个人的方案数

f[0][0][0][0][0] = 1;
for (int a1 = 0; a1 <= N[1]; a1++)
    for (int a2 = 0; a2 <= N[2]; a2++)
        ...
            for (int a5 = 0; a5 <= N[5]; a5++) {
                if (f[a1][a2][a3][a4][a5] == 0) continue;
                if (a1 < N[1]) f[a1+1][a2][a3][a4][a5] += f[a1][a2][a3][a4][a5];
                if (a2 < N[2] && a1 > a2) f[a1][a2+1][a3][a4][a5] += f[a1][a2][a3][a4][a5];
                if (a3 < N[3] && a2 > a3) f[a1][a2][a3+1][a4][a5] += f[a1][a2][a3][a4][a5];
                ...
            }
f[N[1]][N[2]]....[N[5]]
标程

#include <cstdio>
#include <cstring>
#include <iostream>
#define ll long long
using namespace std;
int n[6], k;

void work() {
    for (int i = 1; i <= k; i++) cin >> n[i];
    while (k < 5) n[++k] = 0;
    ll f[n[1]+1][n[2]+1][n[3]+1][n[4]+1][n[5]+1];
    memset(f, 0, sizeof(f));
    f[0][0][0][0][0] = 1;
    for (int i = 0; i <= n[1]; i++)
        for (int j = 0; j <= n[2]; j++)
            for (int k = 0; k <= n[3]; k++)
                for (int l = 0; l <= n[4]; l++)
                    for (int m = 0; m <= n[5]; m++) {
                        if (i < n[1]) f[i+1][j][k][l][m] += f[i][j][k][l][m];
                        if (j < n[2] && i > j) f[i][j+1][k][l][m] += f[i][j][k][l][m];
                        if (k < n[3] && j > k) f[i][j][k+1][l][m] += f[i][j][k][l][m];
                        if (l < n[4] && k > l) f[i][j][k][l+1][m] += f[i][j][k][l][m];
                        if (m < n[5] && l > m) f[i][j][k][l][m+1] += f[i][j][k][l][m];
                    }
    cout << f[n[1]][n[2]][n[3]][n[4]][n[5]] << endl;
}

int main() {
    while (cin >> k && k) work();
    return 0;
}

作者：lydrainbowcat
链接：https://www.acwing.com/activity/content/code/content/590685/

```
