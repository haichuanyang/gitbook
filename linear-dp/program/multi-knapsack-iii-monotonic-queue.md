---
description: hardest knapsack dp
---

# multi knapsack III - monotonic queue

another video explanation [https://www.acwing.com/video/1954/](https://www.acwing.com/video/1954/)



```
算法1
(单调队列优化) O(NV)O(NV)
一共 n 类物品，背包的容量是 m

每类物品的体积为v, 价值为w，个数为s

我们先来回顾一下传统的dp方程

dp[i][j] 表示将前 i 种物品放入容量为 j 的背包中所得到的最大价值
dp[i][j] = max(不放入物品 i，放入1个物品 i，放入2个物品 i, ... , 放入k个物品 i)
这里 k 要满足：k <= s, j - k*v >= 0

不放物品  i = dp[i-1][j]
放k个物品 i = dp[i-1][j - k*v] + k*w

dp[i][j] = max(dp[i-1][j], dp[i-1][j-v] + w, dp[i-1][j-2*v] + 2*w,..., dp[i-1][j-k*v] + k*w)
实际上我们并不需要二维的dp数组，适当的调整循环条件，我们可以重复利用dp数组来保存上一轮的信息

我们令 dp[j] 表示容量为j的情况下，获得的最大价值
那么，针对每一类物品 i ，我们都更新一下 dp[m] --> dp[0] 的值，最后 dp[m] 就是一个全局最优值

dp[m] = max(dp[m], dp[m-v] + w, dp[m-2*v] + 2*w, dp[m-3*v] + 3*w, ...)

接下来，我们把 dp[0] --> dp[m] 写成下面这种形式
dp[0], dp[v],   dp[2*v],   dp[3*v],   ... , dp[k*v]
dp[1], dp[v+1], dp[2*v+1], dp[3*v+1], ... , dp[k*v+1]
dp[2], dp[v+2], dp[2*v+2], dp[3*v+2], ... , dp[k*v+2]
...
dp[j], dp[v+j], dp[2*v+j], dp[3*v+j], ... , dp[k*v+j]
显而易见，m 一定等于 k*v + j，其中  0 <= j < v
所以，我们可以把 dp 数组分成 j 个类，每一类中的值，都是在同类之间转换得到的
也就是说，dp[k*v+j] 只依赖于 { dp[j], dp[v+j], dp[2*v+j], dp[3*v+j], ... , dp[k*v+j] }

因为我们需要的是{ dp[j], dp[v+j], dp[2*v+j], dp[3*v+j], ... , dp[k*v+j] } 中的最大值，
可以通过维护一个单调队列来得到结果。这样的话，问题就变成了 j 个单调队列的问题
所以，我们可以得到
dp[j]    =     dp[j]
dp[j+v]  = max(dp[j] +  w,  dp[j+v])
dp[j+2v] = max(dp[j] + 2w,  dp[j+v] +  w, dp[j+2v])
dp[j+3v] = max(dp[j] + 3w,  dp[j+v] + 2w, dp[j+2v] + w, dp[j+3v])
...
但是，这个队列中前面的数，每次都会增加一个 w ，所以我们需要做一些转换
dp[j]    =     dp[j]
dp[j+v]  = max(dp[j], dp[j+v] - w) + w
dp[j+2v] = max(dp[j], dp[j+v] - w, dp[j+2v] - 2w) + 2w
dp[j+3v] = max(dp[j], dp[j+v] - w, dp[j+2v] - 2w, dp[j+3v] - 3w) + 3w
...
这样，每次入队的值是 dp[j+k*v] - k*w
单调队列问题，最重要的两点
1）维护队列元素的个数，如果不能继续入队，弹出队头元素
2）维护队列的单调性，即：尾值 >= dp[j + k*v] - k*w

本题中，队列中元素的个数应该为 s+1 个，即 0 -- s 个物品 i
C++ 代码
#include <iostream>
#include <cstring>

using namespace std;

const int N = 20010;

int dp[N], pre[N], q[N];
int n, m;

int main() {
    cin >> n >> m;
    for (int i = 0; i < n; ++i) {
        memcpy(pre, dp, sizeof(dp));
        int v, w, s;
        cin >> v >> w >> s;
        for (int j = 0; j < v; ++j) {
            int head = 0, tail = -1;
            for (int k = j; k <= m; k += v) {

                if (head <= tail && k - s*v > q[head])
                    ++head;

                while (head <= tail && pre[q[tail]] - (q[tail] - j)/v * w <= pre[k] - (k - j)/v * w)
                    --tail;

                if (head <= tail)
                    dp[k] = max(dp[k], pre[q[head]] + (k - q[head])/v * w);

                q[++tail] = k;
            }
        }
    }
    cout << dp[m] << endl;
    return 0;
}

作者：lys
链接：https://www.acwing.com/solution/content/6500/

```

{% file src="../../.gitbook/assets/AcWing 6. 多重背包问题 III（图解，无代码） - AcWing.pdf" %}

```
单调队列优化+滚动数组优化
根据yxc大佬的讲解
#include<bits/stdc++.h>
using namespace std;
const int maxn=110;
int n,m;
int v[maxn],w[maxn],s[maxn],f[maxn][maxn];
int main()
{
    cin>>n>>m;
    for(int i=1;i<=n;i++) cin>>v[i]>>w[i]>>s[i];
    for(int i=1;i<=n;i++)
    {
        for(int j=0;j<=m;j++)
        {
            for(int k=0;k<=s[i] && j-k*v[i]>=0;k++)
            {
                f[i][j]=max(f[i-1][j],f[i-1][j-k*v[i]]+k*w[i]);
            }
        }
    }
    cout<<f[m]<<endl;
}
首先优化思路来自最最原始的无优化的方程。
仔细观察，对于任意的j，都是从v[i]的倍数转移过来的。它们本来应该是连续的，可以用滑动窗口（不熟悉此问题的同学可以先行百度）解决。但在无优化的时候却每次把所有的倍数都遍历了一遍。
所以可以把m根据模v[i]的余数分为v[i]类。

for(int j=0;j<v;j++)
此时对于任意的j，只需要向v[i]的倍数去转移。所以我们在下一层循环的时候把k定义为j+k*v[i]

for(int k=j;k<=m;k+=v)
此时的k相当于原来的j，但是我们可以利用k和v之间存在的倍数关系去做滑动窗口。
由于滑动窗口记录的是下标，但每一个k所对应的下标都是在变化的。所以要根据当前的k判断窗口里存在的k对应的值包含了多少个v，以便于计算新的价值

v的个数=(下标-余数)/v   价值=(下标-余数)/v*w
       =(q[h]-j)/v          =(k-j)/v*w
然后每次只用了前i-1的值，所以可以滚动数组优化一下空间

C++ 代码
#include<bits/stdc++.h>
using namespace std;
int n,m;
int f[20002],q[20002],g[20002];
int main()
{
    cin>>n>>m;
    for(int i=1;i<=n;i++)
    {
        int v,w,s;
        cin>>v>>w>>s;
        memcpy(g,f,sizeof(f));
        //滚动数组优化空间，g[]即f[i-1][];
        for(int j=0;j<v;j++)
        {
            int hh=0,tt=-1;
            for(int k=j;k<=m;k+=v)
            {
                f[k]=g[k];
                if(hh<=tt && k-s*v>q[hh])  
                //如果当前窗口的内容超过了s个;
                {
                    hh++;
                }
                if(hh<=tt) 
                {
                    f[k]=max(f[k],g[q[hh]]+(k-q[hh])/v*w); 
                    //max(f[i-1][k],f[i-1][能转移里最大]+个数*v[i]);
                }
                while(hh<=tt && g[q[tt]]-(q[tt]-j)/v*w <= g[k]-(k-j)/v*w)
                {
                    tt--;
                }
                q[++tt]=k;
            }
        }
    }
    cout<<f[m]<<endl;
}

作者：这个显卡不太冷
链接：https://www.acwing.com/solution/content/921/

```





```
#include <bits/stdc++.h>
using namespace std;

int n, m;
int f[20002], q[20002], g[20002];
int main() {
    cin >> n >> m;
    for (int i = 0; i <= n; ++i) {
        int v, w, s;
        cin >> v >> w >> s;
        memcpy(g, f, sizeof(f));
        for (int j = 0; j < v; ++j) {
            /*
            hh为队首位置
            tt为队尾位置
            数值越大，表示位置越后面
            队首在队尾后面队列为空，即hh>tt时队列为空
            */
            int hh = 0, tt = -1;
            /* 
            q[]为单调队列
            存储前个s(物品数量)中的最大值
            数组从头(hh)到尾(tt)单调递减
            */
            for (int k = j; k <= m; k += v) {

                // f[k] = g[k]; //这一行我没理解有什么用，去掉后也能ac？我认为前面使用过了memcpy,这里应该一定是相等的

                //k代表假设当前背包容量为k
                //q[hh]为队首元素（最大值的下标）
                //g[]为dp[i-1][]
                //f[]为dp[i][]

                /*
                维护一个大小为k的区间
                使最大值为前k个元素中最大
                (k - q[hh]) / v 表示拿取物品的数量（相当于最原始的多重背包dp的k）
                */
                if (hh <= tt && (k - q[hh]) / v > s) {
                    hh++;
                }

                /*
                若队内有值，该值就是前k个元素的最大值
                (k - q[hh]) / v 表示拿取物品的数量（相当于最原始的多重背包dp的k）
                q[hh]为队首元素（g[]中前k个数中最大值的下标），g[]为dp[i-1][]
                所以 g[q[hh]]为只考虑前i-1个物品时，拿前q[hh]个物品的最大价值
                */
                if (hh <= tt) {
                    f[k] = max(f[k], g[q[hh]] + (k - q[hh]) / v * w);
                }

                /*
                若队尾元素小于当前元素，则队尾元素出队；
                若队内一个元素比当前元素小，则该元素一定不会被用到（单调队列思想）
                g[q[tt]] + (k - q[tt]) / v * w 
                与
                g[k] - (k - j) / v * w
                分别表示队尾元素的值和当前元素的值
                */
                while (hh <= tt && g[q[tt]] - (q[tt] - j) / v * w <= g[k] - (k - j) / v * w) {
                    tt--;
                }

                //去除了比当前小的元素，保证队列里的元素都比当前元素大，入队
                q[++tt] = k;
            }
        }
    }
    cout << f[m] << endl;
}


```

```
变量名换成了英文名，可能更容易理解
#include <bits/stdc++.h>
using namespace std;

int item_number, packge_volumn;
int volume, value, number;

int dp[20010];
int dp_prev[20010];
int monotone_queue[20010];

int main() {
    cin >> item_number >> packge_volumn;
    for (int i = 0; i < item_number; ++i) {
        memcpy(dp_prev, dp, sizeof(dp));
        cin >> volume >> value >> number;
        for (int j = 0; j < volume; ++j) {
            int head = 0, tail = -1;
            for (int k = j; k <= packge_volumn; k += volume) {
                if (head <= tail && (k - monotone_queue[head]) / volume > number)
                    head++;

                if (head <= tail)
                    dp[k] = max(dp[k], dp_prev[monotone_queue[head]] + (k - monotone_queue[head]) / volume * value);

                while (head <= tail && dp_prev[monotone_queue[tail]] - (monotone_queue[tail] - j) / volume * value<= dp_prev[k] - (k - j) / volume * value) {
                    --tail;
                }

                monotone_queue[++tail] = k;
            }
        }
    }
    cout << dp[packge_volumn] << endl;
}

作者：o_O
链接：https://www.acwing.com/solution/content/1537/

```

```
(单调队列优化dp) O(NV)O(NV)
n个物品， 背包容量为m

假设第i个物品体积为v， 价值为w， 个数为s

f[i, j ] = f[i-1, j-v]+w, f[i-1, j-2v]+2w, f[i-1, j-3v] +3w,  f[i-1, j-4v]+4w,  ... f[i-1,j-sv]+sw
f[i, j-v] =               f[i-1, j-2v]+w,  f[i-1，j-3v] +2w,  f[i-1, j-4v]+3w, ... f[i-1, j-(s+1)v] + sw
f[i, j-2v] =                               f[i-1, j-3v】+w ,  f[i-1, j-4v]+2w, ...f[i-1, j-(s+2)v] + sw

设m % v = d 变换下上面式子可以得出，

f[i, d]    = f[i-1][d]
f[i, d+v]  = max(f[i-1, d] +w,  f[i-1, d+v])                                    = max(f[i-1, d],  f[i-1, d+v]-w)                                    + w
f[i, d+2v] = max(f[i-1, d] +2w, f[i-1, d+v] +w,  f[i-1, d+2v])                  = max(f[i-1, d],  f[i-1, d+v]-w, f[i-1, d+2v]-2w)                   + 2w
f[i, d+3v] = max(f[i-1, d] +3w, f[i-1, d+v] +2w, f[i-1, d+2v]+w,  f[i-1, d+3v]) = max(f[i-1, d],  f[i-1, d+v]-w, f[i-1, d+2v]-2w,  f[i-1, d+3v]-3w) + 3w
f[i, d+4v] = max(f[i-1, d] +4w, f[i-1, d+v] +3w, f[i-1, d+2v]+2w, f[i-1, d+3v]+w, f[i, d+4v])

我们发现对于体积 j，j % v = d的话，j的状态仅由体积 % v 也等于d的状态转移而来， 比如d+3v, 仅由体积为d+v, d+2v, d这些状态转移， 这些体积 % v都等于d，我们将之前的状态
放入单调队列即可， 放入时，有个技巧，放入 f[i-1, d+ jv] - jw  而不是 f[i-1, d+ jv] ，看看上面的式子就知道为啥了，是为了利用之前的结果。 我们减去再加回来就能保证当前状态最后的答案正确了。

第0次 f[i, d] = f[i-1, d] 入队
第1次 f[i-1, d+v]-w 进队列与队列之前的数字比较
第2次 f[i-1, d+2v]-2w 进队列与队列之前的数字比较
第j次 f[i-1, d+jv]-jw 进队列与之前队列中的数字比较

所以我们枚举i个物品
枚举余数d
枚举j即可， 用于判断体积为d + jv的时候,背包能装满的最大值 由上面式子发现可以用单调队列处理重复项

有个小细节，由于物品个数有限制我们要加个数组存储 队列中状态对应的编号j


#include <iostream>

using namespace std;

const int C_N = 1005, C_V = 20005;

int f[C_N][C_V], queue[C_V], num[C_V];


int main() {
    int N, V;
    cin >> N >> V;

    for (int i = 1; i <= N; i++) {
        int v, w, s;
        cin >> v >> w >> s;
        for (int d = 0; d < v; d ++) {
            int head = 0, tail = -1;
            for (int j = 0; j <= (V-d)/v; j++) {
                int tmp = f[i-1][d + j * v] - j * w;
                while (head <= tail && j - num[head] > s ) head++;
                while (head <= tail && queue[tail] <= tmp) tail--;
                queue[++tail] = tmp;
                num[tail] = j;
                f[i][d + j*v] = queue[head]+ w * j;
            }
        }
    }
    cout << f[N][V];
    return 0;
}


这里我和yxc的代码有不同的地方，y总的第三重循环是直接枚举的体积

作者：福如东海
链接：https://www.acwing.com/solution/content/4237/

```



```
对单调队列优化的一些理解
变量定义
令背包体积为c,循环时体积为j,当前物品的体积为v,价值为w,数量为s. 需计算dp[0,c]的值
理解
根据状态转移方程 dp[i][j] = max(dp[i - 1][j - k * (v)] + k * w) 0<=k<=s (此处不考虑体积不足情况)
可知dp[j]是由 间隔为 v 的值转移过来的.
那么根据j%v的余数划分等价类,dp[j]就只能由同一等价类转移过来
(依靠划分等价类将不相邻的区间,变成逻辑相邻的区间,就可以套用单调队列进行优化)
由
dp[i][j   ] = max{                               dp[i-1][j]   , dp[i-1][j-v]+ w, dp[i-1][j-2v]+2w......dp[i-1][j-(k-2)v]+(k-2)w, dp[i-1][j-(k-1)v]+(k-1)w, dp[i-1][j-kv]+kw}; 
dp[i][j+ v] = max{               dp[i-1][j+v]  , dp[i-1][j]+ w, dp[i-1][j-v]+2w, dp[i-1][j-2v]+3w......dp[i-1][j-(k-2)v]+(k-1)w, dp[i-1][j-(k-1)v]+(k  )w                  };
dp[i][j+2v] = max{dp[i-1][j+2v], dp[i-1][j+v]+w, dp[i-1][j]+2w, dp[i-1][j-v]+3w, dp[i-1][j-2v]+4w......dp[i-1][j-(k-2)v]+(k  )w                                            };
可知:
    越靠前的项增加的w越多
    j每增加一个v时,原集合所有元素需增加一个w
    可以运用滑动窗口,单调队列解决

    如何解决每次增加w问题?
    我们只需知道哪项最大,单调队列可以只用维护相对大小.
将j离t（令t同一等价类首元素）越近增加的w越多。每多间隔一个v，增加会少一个w。
转化为，离f越近减去w越少，减少（（j - t）/ v）个 w。
那么同一集合元素其相对大小不变。例子如下:
令v = 3, s = 3可知
f[0 ] = f[0 ]
f[ v] = f[ v], f[0 ]+w
f[2v] = f[2v], f[ v]+w, f[0 ]+2w
f[3v] = f[3v], f[2v]+w, f[ v]+2w, f[0]+3w
f[4v] = f[4v], f[3v]+w, f[2v]+2w, f[v]+3w

令
f[0 ] = f[0 ]
f[ v] = f[ v] - 1w
f[2v] = f[2v] - 2w
f[3v] = f[3v] - 3w
f[4v] = f[4v] - 4w

则原式变化为
f[0 ] = f[0 ]
f[ v] = f[ v]- w, f[0 ]
f[2v] = f[2v]-2w, f[ v]- w, f[0 ]
f[3v] = f[3v]-3w, f[2v]-2w, f[ v]- w, f[0]
f[4v] = f[4v]-4w, f[3v]-3w, f[2v]-2w, f[v]-w

对比
f[3v] = f[3v]   , f[2v]+ w, f[v]+2w, f[0]+3w
f[3v] = f[3v]-3w, f[2v]-2w, f[v]- w, f[0]
可得:f[3v]对应状态集合中的相对大小并未改变
//先放上yxc大佬代码 1000ms左右
#include<bits/stdc++.h>
using namespace std;
const int N = 20010;
int n, m;
int dp[N], g[N], q[N];

int main(){
    cin>>n>>m;
    for(int i=1; i<=n; ++i){
        int v, w, s; cin>>v>>w>>s;
        memcpy(g, dp, sizeof(dp));

        //g为dp[i-1], dp为dp[i]
        for(int j=0; j<v; ++j){//枚举余数即等价类
            int hh = 0, tt = -1;//hh为队首 tt为队尾
            for(int k=j; k<=m; k+=v){//枚举同一等价类的背包体积
                dp[k] = g[k]; //此句意义不大，
                if(hh <= tt && k - s * v > q[hh]) ++hh;//弹出首部（该首部为不合法信息）
                /*
                    tt >= hh 是单调队列非空成立条件
                    k - s * v > q[hh]
                    该单调队列要维护一个不超过s的集合。
                    如果当前编号-队列最大容积（间隔v 容积s * v）＞首元素，则需将首元素出队
                */
                if(hh <= tt) dp[k] = max(dp[k], g[q[hh]]+(k - q[hh]) / v * w);//更新当前数
                /*
                    首元素是放置物品中的最大，需要和不放物品值进行比较
                    q队列实际放的是位置，位置差比上v就是放置物品的数量
                */
                while(hh <= tt && g[q[tt]] - (q[tt] - j) / v * w <= g[k] - (k - j) / v * w) --tt;//弹出小于元素
                /*
                   （编号-j）/ v * w是上面转化的条件
                */
                q[++tt] = k;//将当前数插入队列
                /*
                   插入队列的实际是位置而不是数值
                   因为我们还需要位置差信息
                */
            }
        }
    }
    cout<<dp[m]<<endl;
}
// 讨论区Belous大佬的代码 380ms
#include <stdio.h>
const int  N  = 20001;
struct node{
    int pos, val;
}q[N];
int dp[N];
int main(){
    int n, m; scanf("%d%d", &n, &m);
    while (n--){
        int v, w, s; scanf("%d%d%d", &v, &w, &s);
        for (int j = 0; j < v; ++j){
            int hh = 0, tt = 0, stop = (m - j) / v;
            //[hh, tt)
            for (int k = 0; k <= stop; ++k){
                int val = dp[k * v + j] - k * w;
                while (hh < tt && val >= q[tt-1].val) --tt;
                q[tt].pos = k, q[tt++].val = val;
                if (q[hh].pos < k - s) ++hh;
                dp[v * k + j] = q[hh].val + k * w;
            }
        }
    }
    printf("%d\n", dp[m]);
}

```

```




C++二维代码如下

for (int i = 1; i <= n; ++ i) {
        int v, w, s;
        cin >> v >> w >> s;
        for (int j = 0; j <= m; ++ j) 
            for (int k = 0; k <= s && k * v <= j; ++ k) 
                f[i][j] = max(f[i][j], f[i - 1][j - k * v] + k * w);
    }
C++一维代码如下

for (int i = 1; i <= n; ++ i) {
        int v, w, s;
        cin >> v >> w >> s;
        for (int j = m; j >= v; -- j) 
            for (int k = 0; k <= s && k * v <= j; ++ k) 
                f[j] = max(f[j], f[j - k * v] + k * w);
    }
通过观察朴素二维代码可发现

for (int j = 0; j <= m; ++ j) 
            for (int k = 0; k <= s && k * v <= j; ++ k) 
                f[i][j] = max(f[i][j], f[i - 1][j - k * v] + k * w);
在上述代码f[i][j] = max(f[i][j], f[i - 1][j - k * v] + k * w)中f[i - 1][j - k * v]多次重复出现，造成时间浪费；
（j : 0 ~ m, 而m / v大部分情况下存在m / v > 1，会存在不同的j和k进行j - k * v会出现相同值：j1 - k1 * v == j2 - k2 * v), 由此进行思考可考虑对其进行优化。
由于f[i][j] = max(f[i - 1][k * v] + k * w) 0 =< k <= s 对其进行展开如下图

可发现f[i][j]所求随着j的上升而发生滑动，可发现适用于滑动窗口进行操作，随着j上升，选取值始终在窗口内，避免了如同朴素版多次重复计算
另外我们可以发现，在滑动窗口内，进入窗口与滑出窗口的始终余数相等满足r, r + v, r + 2v, ..., j - v, j

C++二维滑动窗口 // 辅助理解，直接使用二维会超

for (int i = 1; i <= n; ++ i) {
        int v, w, s;
        cin >> v >> w >> s;
        for (int j = 0; j < v; ++ j) {
            int hh = 0, tt = -1;
            for (int k = j; k <= m; k += v) {
                if (hh <= tt && q[hh] < k - s * v) ++ hh;
                f[i][k] = f[i-1][k];
                if (hh <= tt) f[i][k] = max(f[i][k], f[i-1][q[hh]] + (k - q[hh]) / v * w);
                while (hh <= tt && f[i-1][q[tt]] + (k - q[tt]) / v * w <= f[i-1][k]) -- tt;
                q[++tt] = k;
            }
        }
    }
C++一维滑动窗口优化代码

#include <bits/stdc++.h>

using namespace std;

const int N = 20010;

int n, m;
int f[N], g[N];
int q[N];

int main() {
    ios::sync_with_stdio(false), cin.tie(0), cout.tie(0);
    cin >> n >> m;
    for (int i = 0; i < n; ++ i) {
        int v, w, s;
        cin >> v >> w >> s;
        memcpy(g, f, sizeof f);
        for (int j = 0; j < v; ++ j) {
            int hh = 0, tt = -1;
            for (int k = j; k <= m; k += v) {
                if (hh <= tt && q[hh] < k - s * v) ++ hh;
                if (hh <= tt) f[k] = max(f[k], g[q[hh]] + (k - q[hh]) / v * w);
                while (hh <= tt && g[q[tt]] - (q[tt] - j) / v * w <= g[k] - (k - j) / v * w) -- tt;
            //  while (hh <= tt && g[q[tt]] - q[tt] / v * w <= g[k] - k / v * w) -- tt;
            //  while (hh <= tt && g[q[tt]] + (k - q[tt]) / v * w <= g[k]) -- tt;
                q[++tt] = k;
            }
        }
    }

    cout << f[m] << "\n";

    return 0;
}
//以红色框为例
if (hh <= tt) f[k] = max(f[k], g[q[hh]] + (k - q[hh]) / v * w);
// 红色框下面三个中所找出最大值 g[q[hh]] + ?w 对其进行 "+w" 操作，与f[i-1][k]比较取最大值 
while (hh <= tt && g[q[tt]] + (k - q[tt]) / v * w <= g[k]) -- tt;
//更新红色框上面四个(s+1)个的最大值以便进行下一个j+v计算

作者：yuyangp
链接：https://www.acwing.com/solution/content/15988/

```



simple DP analysis:

![32.png](https://cdn.acwing.com/media/article/image/2020/07/10/37963\_87f64696c2-32.png)



![123.png](https://cdn.acwing.com/media/article/image/2020/07/10/37963\_93635b5cc2-123.png)
