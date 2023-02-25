# day14 8 queen on chess board

p1432 8 queen problem - dfs recursion, with state restoring;

sudoku problems are also same type, bigger space, so must dfs add pruning; p166 p169 p

dancing links p1067





```
AcWing 1432. 棋盘挑战【含详细推导过程】   


问题概述
有一个n * n 的盘，要在每行放置一个皇后，并且满足：该放置位置的每行、每列、对角线上，均不能出现其他皇后，问如何放置，有多少种放置方法。

解题思路
我们上来肯定能想到最简单的方法，就是暴力枚举法，枚举所有情况，并且判断情况的合法性即可。

我们用(1,x1)、(2,x2)......、(n,xn)(1,x1)、(2,x2)......、(n,xn) 表示放置位置，其中第一个坐标表示行，第二个坐标表示列。

判断条件：

xi≠xjxi≠xj (即列数不能相同)

i−xi≠j−xji−xi≠j−xj (即主对角线不能相同，因为在某条主对角线上，行坐标减列坐标的值是一个常量)

i+xi≠j+xji+xi≠j+xj (即副对角线不能相同，因为在某条副对角线上，行坐标加列坐标的值是一个常量)

这里的对角线判断可以统一成abs(xi−xj)≠i−jabs(xi−xj)≠i−j，因为， 将上述两个式子移位变形可以得到:

①i−j==xi−xj①i−j==xi−xj 、 ②i−j==xj−xi②i−j==xj−xi
即判断：xi−xjxi−xj的绝对值和i−ji−j 的关系即可。

下面以8皇后问题来求解。

#include <bits/stdc++.h>
using namespace std;
typedef long long ll;
const int maxn = 100;
int n;
int arr[maxn + 5];
bool check(int arr[], int n)
{
    for(int i = 1; i <= n - 1; ++i) {
        if(abs(arr[i] - arr[n]) == abs(i - n) || arr[i] == arr[n]) return false;
    }
    return true;
}
void queen(int a[])
{
    for(a[1] = 1; a[1] <= 8; ++a[1]) 
        for(a[2] = 1; a[2] <= 8; ++a[2]) {
            if(!check(arr, 2)) continue;
            for(a[3] = 1; a[3] <= 8; ++a[3]) {
                if(!check(arr, 3)) continue;
                for(a[3] = 1; a[3] <= 8; ++a[3]) {
                if(!check(arr, 3)) continue;
                    for(a[4] = 1; a[4] <= 8; ++a[4]) {
                        if(!check(arr, 4)) continue;
                        for(a[5] = 1; a[5] <= 8; ++a[5]) {
                            if(!check(arr, 5)) continue;
                            for(a[6] = 1; a[6] <= 8; ++a[6]) {
                                if(!check(arr, 6)) continue;
                                for(a[7] = 1; a[7] <= 8; ++a[7]) {
                                    if(!check(arr, 7)) continue;
                                    for(a[8] = 1; a[8] <= 8; ++a[8]) {
                                        if(!check(arr, 8)) continue;
                                        else {
                                            for(int i = 1; i<= 8; ++i) {
                                               cout <<"第" << i << "行放置位置为(" << i << "," << arr[i] << ")" << endl;
                                            }
                                            puts("-------------------------");
                                      }
                                 }
                             }
                        }
                     }
                }

            }
        }
     }
}


int main()
{
    queen(arr);
    return 0;
}
上述的check执行时间是O(n2)，因为第二行时，check函数要执行2次，第三行时，check函数要执行3次，依此类推，求和可得，check函数执行的总时间是O(n2)O(n2)。

回溯法
我们观察到，在很多情况下是没有必要再往下试探了的，因为在排完之前，就已经不满足上述的判断条件了，这个时候，我们可以停止向下搜索，并进行下一次的搜索，这种思路被称为回溯思想，这样可以大大的减少了可能的情况。

但是如果推广到n皇后，就无法再使用循环的这种方法了，因为不能确定循环的个数，这个时候，
我们就想到了使用递归的方法来解决，下述代码使用了3个bool数组来对条件进行判断，a数组判断列，
b数组判断主对角线，c数组判断副对角线，这样可以使判断语句的时间复杂度降低到O(1)O(1)。

实现代码

#include <bits/stdc++.h>
using namespace std;
typedef long long ll;
const int maxn = 100;
int n;
int arr[maxn + 5]; //存放结果，下标代表行坐标，元素值代表列坐标
bool a[maxn + 5], b[maxn + 5], c[maxn + 5];
void backtrack(int k) //k代表行数，i代表列数
{
    if(k > n) {
        for(int i = 1; i <= n; ++i) {
            cout <<"第" << i << "行放置位置为(" << i << "," << arr[i] << ")" << endl;
        }
        puts("----------------");
    } 
    else {
        for(int i = 1; i <= n; ++i) {
            if(!a[i] && !b[i - k + n] && !c[i + k]) { //i - k + n 是为防止出现负数，运用了移码的思想
                arr[k] = i;
                a[i] = b[i - k + n] = c[i + k] = true;
                backtrack(k + 1);
                a[i] = b[i - k + n] = c[i + k] = false;
            }
        }
    }
}

int main()
{
    cin >> n;
    backtrack(1);
    return 0;
}
还可以把上述三个布尔数组合成一个二维数组，改进写法。

#include <bits/stdc++.h>
using namespace std;
typedef long long ll;
const int maxn = 100;
int n;
int arr[maxn + 5]; //存放结果，下标代表行坐标，元素值代表列坐标
bool vis[3][maxn + 5];  //0行代表列，1行代表主对角线，2行代表副对角线
void backtrack(int k) //k代表行数，i代表列数
{
    if(k > n) {
        for(int i = 1; i <= n; ++i) {
            cout <<"第" << i << "行放置位置为(" << i << "," << arr[i] << ")" << endl;
        }
        puts("----------------");
    } 
    else {
        for(int i = 1; i <= n; ++i) {
            if(!vis[0][i] && !vis[1][i - k + n] && !vis[2][i + k]) { //i - k + n 是为防止出现负数，运用了移码的思想
                arr[k] = i;
                vis[0][i] = vis[1][i - k + n] = vis[2][i + k] = true;
                backtrack(k + 1);
                vis[0][i] = vis[1][i - k + n] = vis[2][i + k] = false;
            }
        }
    }
}

int main()
{
    cin >> n;
    backtrack(1);
    return 0;
}
实战训练
hduoj2553:N皇后问题

该题问一共有多少种情况，那么我们可以直接套用上述模板即可解决。

#include <bits/stdc++.h>
using namespace std;
typedef long long ll;
const int maxn = 100;
int n, t, sum;
bool vis[3][maxn + 5];  //0行代表列，1行代表主对角线，2行代表副对角线
int ans[maxn + 5];
void queen(int k)
{
    if(k > n) {
        sum++;
        return;
    }
    else {
        for(int i = 1; i <= n; ++i) {
            if(!vis[0][i] && !vis[1][i - k + n] && !vis[2][i + k]) {
                vis[0][i] = vis[1][i - k + n] = vis[2][i + k] = true;
                queen(k + 1);
                vis[0][i] = vis[1][i - k + n] = vis[2][i + k] = false;
            }
        }
    }
}

int main()
{
    for(n = 1; n <= 10; ++n) {
        memset(vis, false, sizeof(vis));
        sum = 0;
        queen(1);
        ans[n] = sum;
    }
    while(cin >> t, t) cout << ans[t] << endl;
    return 0;
}
总结
回溯法是在问题的解空间树中，按深度优先策略，从根节点除法搜索解空间树。
算法搜索至解空间树的任意一点时，先判断该结点是否包含问题的解。如果肯定不包含，
则跳过对以该结点为根子树的搜索，逐层向其祖先结点 回溯，否则，进入该子树，继续按深度优先策略搜索。

求问题所有解：要回溯到根，且根节点的所有子树均已被搜索遍才结束。

求问题某一解：只要搜索到一个解即可结束搜索。

由上述的问题，我们可以总结出回溯递归法的一般模板，模板如下：

void try(int i) {
    if(i > n) 输出结果;
    else {
        //枚举i所以可能情况
        for(j = 下界; j <= 上界; j++) {
            if(check(j)) { //满足限界函数和约束条件
                //设置标识
                try(i + 1);
                //清除标识
            }
        }
    }
}
题解
按照上面的模板，我们从第一行到最后一行，字典序肯定是递增的，所以我们加一个判断，如果当前方案小于等于3，则输出，否则忽略，path是用来记录方案的数组。

#include <bits/stdc++.h>
using namespace std;
typedef long long ll;
const int maxn = 100;
int n, t, sum;
bool vis[3][maxn + 5];  //0行代表列，1行代表主对角线，2行代表副对角线

vector<int> path;
void queen (int k) {
    if(k > n) {
        if(++sum <= 3) {
            for(auto &x: path) cout << x << " ";
            cout << endl;
        }
        return;
    }
    else {
        for(int i = 1; i <= n; ++i) {
            if(!vis[0][i] && !vis[1][i - k + n] && !vis[2][i + k]) {
                vis[0][i] = vis[1][i - k + n] = vis[2][i + k] = true;
                path.push_back(i);
                queen(k + 1);
                vis[0][i] = vis[1][i - k + n] = vis[2][i + k] = false;
                path.pop_back();
            }
        }
    }
}

int main() {
    cin >> n;
    queen(1);
    cout << sum << endl;
    return 0;
}


作者：血小板自动机
链接：https://www.acwing.com/solution/content/31098/

```
