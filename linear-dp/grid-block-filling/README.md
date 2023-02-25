---
description: '291'
---

# 1x2 block filling

i<<1<\<n! use long long for f\[n]\[m]

```cpp
 1. 所谓的状态压缩DP，就是用二进制数保存状态。为什么不直接用数组记录呢？因为用一个二进制数记录方便作位运算。前面做过的八皇后，八数码，也用到了状态压缩。

 2. 本题等价于找到所有横放 1 X 2 小方格的方案数，因为所有横放确定了，那么竖放方案是唯一的。

 3. 用f[i][j]记录第i列第j个状态。j状态位等于1表示上一列有横放格子，本列有格子捅出来。转移方程很简单，本列的每一个状态都由上列所有“合法”状态转移过来f[i][j] += f[i - 1][k]

 4. 两个转移条件： i 列和 i - 1列同一行不同时捅出来 ； 本列捅出来的状态j和上列捅出来的状态k求或，得到上列是否为奇数空行状态，奇数空行不转移。

 5. 初始化条件f[0][0] = 1，第0列只能是状态0，无任何格子捅出来。返回f[m][0]。第m + 1列不能有东西捅出来。

#include<bits/stdc++.h>
using namespace std;
const int N = 12, M = 1 << N;
int st[M];
long long f[N][M];


int main(){
    int n, m;
    while (cin >> n >> m && (n || m)){

        for (int i = 0; i < 1 << n; i ++){
            int cnt = 0;
            st[i] = true;
            for (int j = 0; j < n; j ++)
                if (i >> j & 1){
                    if (cnt & 1) st[i] = false; // cnt 为当前已经存在多少个连续的0
                    cnt = 0;
                }
                else cnt ++;
            if (cnt & 1) st[i] = false; // 扫完后要判断一下最后一段有多少个连续的0
        }

        memset(f, 0, sizeof f);
        f[0][0] = 1;
        for (int i = 1; i <= m; i ++)
            for (int j = 0; j < 1 << n; j ++)
                for (int k = 0; k < 1 << n; k ++)
                    if ((j & k) == 0 && (st[j | k])) 
                    // j & k == 0 表示 i 列和 i - 1列同一行不同时捅出来
                    // st[j | k] == 1 表示 在 i 列状态 j， i - 1 列状态 k 的情况下是合法的.
                        f[i][j] += f[i - 1][k];      
        cout << f[m][0] << endl;
    }
    return 0;
}

作者：sjytker
链接：https://www.acwing.com/solution/content/5121/

题目描述
本题可以用状态压缩DP解答
用一个N位二进制数，每一位用0/1来表示有N个物品的组合中第i个物品的状态
因此可以循环1~2^N-1中的所有数来枚举它的全部状态

下面是我加了十分详细的注释的y总代码

代码主要分为两个部分（横着摆放的方式确定后，竖着摆放的方式就已确定）：

枚举每一列合法的占位方式，记录在str数组内
（把连续空格数为奇数的状态设定为false，因为空格要摆放竖着的方块，单个空格无法摆放方块）

开始DP

#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 12, M = 1 << N;//M的每一位二进制位储存一种状态

int n, m;
long long f[N][M];
bool st[M];//储存每一列上合法的摆放状态

//f[i][j]表示摆放第i列，i列向后伸出来横着的方格状态为j的方案数，j为一个二进制数，用01表示是否戳出来
int main()
{
    while (cin >> n >> m, n || m)
    {
        //枚举每一列的占位状态里哪些是合法的
        for (int i = 0; i < 1 << n; i ++ )//一共n行，枚举n位不同的状态
        {
            int cnt = 0;//用来记录连续的0的个数
            st[i] = true;//记录这个状态被枚举过且可行
            for (int j = 0; j < n; j ++ )//从低位到高位枚举它的每一位
                if (i >> j & 1)//如果为1
                {
                    if (cnt & 1) st[i] = false;//如果之前连续0的个数是奇数，竖的方块插不进来，这种状态不行
                    cnt = 0;//清空计数器
                }
                else cnt ++ ;//如果不为1，计数器+1
            if (cnt & 1) st[i] = false;//到末尾再判断一下前面0的个数是否为奇数，同前
        }

        memset(f, 0, sizeof f);//一定要记得初始化成0，对于每个新的输入要重新计算f[N][M]
        f[0][0] = 1;
        for (int i = 1; i <= m; i ++ )//对于每一列
            for (int j = 0; j < 1 << n; j ++ )//枚举j的状态
                for (int k = 0; k < 1 << n; k ++ )//再枚举前一行的伸出状态k
                    if ((j & k) == 0 && st[j | k])//如果它们没有冲突，i这一列被占位的情况也是合法的话
                        f[i][j] += f[i - 1][k];//那么这种状态下它的方案数等于之前每种k状态数目的和

        cout << f[m][0] << endl;//求的是第m行排满，并且第m行不向外伸出块的情况
    }
    return 0;
}

作者：Alier
链接：https://www.acwing.com/solution/content/6103/

```



![蒙德里安的梦想 .png](https://cdn.acwing.com/media/article/image/2020/07/22/652\_83a603dacb-%E8%92%99%E5%BE%B7%E9%87%8C%E5%AE%89%E7%9A%84%E6%A2%A6%E6%83%B3-.png)



![蒙德里安的梦想 -2.png](https://cdn.acwing.com/media/article/image/2020/07/22/652\_796582accb-%E8%92%99%E5%BE%B7%E9%87%8C%E5%AE%89%E7%9A%84%E6%A2%A6%E6%83%B3--2.png)

![蒙德里安的梦想3.png](https://cdn.acwing.com/media/article/image/2020/09/01/652\_72d9d7f0ec-%E8%92%99%E5%BE%B7%E9%87%8C%E5%AE%89%E7%9A%84%E6%A2%A6%E6%83%B33.png)



![蒙德里安的梦想4.png](https://cdn.acwing.com/media/article/image/2020/09/01/652\_fd0fe18aec-%E8%92%99%E5%BE%B7%E9%87%8C%E5%AE%89%E7%9A%84%E6%A2%A6%E6%83%B34.png)



![蒙德里安的梦想6.png](https://cdn.acwing.com/media/article/image/2020/09/01/652\_25176c5cec-%E8%92%99%E5%BE%B7%E9%87%8C%E5%AE%89%E7%9A%84%E6%A2%A6%E6%83%B36.png)



```cpp
核心：
摆放的小方格方案数 等价于 横着摆放的小方格方案数

如何判断当前方案是否合法？
遍历每一列，i列的方案数只和i-1列有关系

j&k==0， i-2列伸到i-1的小方格 和i-1列放置的小方格 不重复。
每一列，所有连续着空着的小方格必须是偶数个
dp分析：
状态表示 f[i][j]: 前i-1列已经确定，且从第i-1列伸出的小方格在第i列的状态为j 的方案数。属性：个数。

i=2, j=11001 表示下列图的状态


但是i-1, i列已经固定，所以集合划分是依据i-2 列伸到 i-1 列的不同状态 k 来划分

i-2 列伸到 i-1 列的状态 k=00100





状态计算：（限制条件：i-1列非空白位置可以不能放置小方格），在i列不同的放置方法就是不同的集合划分。

问题：第 i-2 列伸到 i-1 列的状态为 k ， 是否能成功转移到 第 i-1 列伸到 i 列的状态为 j ?
需要满足如下条件:

j&k==0， i-2列伸到i-1的小方格 和i-1列放置的小方格 不重复。
每一列，所有连续着空着的小方格必须是偶数个
f[m][0]:
列数从0开始计数，m列不放小方格，前m-1列已经完全摆放好并且不伸出来的状态

#include<iostream>
#include<cstring>

using namespace std;

//数据范围1~11
const int N = 12;
//每一列的每一个空格有两种选择，放和不放，所以是2^n
const int M = 1 << N;
//方案数比较大，所以要使用long long 类型
//f[i][j]表示 i-1列的方案数已经确定，从i-1列伸出，并且第i列的状态是j的所有方案数
long long f[N][M];
//第 i-2 列伸到 i-1 列的状态为 k ， 是否能成功转移到 第 i-1 列伸到 i 列的状态为 j
//st[j|k]=true 表示能成功转移
bool st[M];
//n行m列
int n, m;

int main() {
//    预处理st数组
    while (cin >> n >> m, n || m) {
        for (int i = 0; i < 1 << n; i++) {
//            第 i-2 列伸到 i-1 列的状态为 k ， 
//            能成功转移到 
//            第 i-1 列伸到 i 列的状态为 j
            st[i] = true;
//            记录一列中0的个数
            int cnt = 0;
            for (int j = 0; j < n; j++) {
//                通过位操作，i状态下j行是否放置方格，
//                0就是不放， 1就是放
                if (i >> j & 1) {
//                    如果放置小方块使得连续的空白格子数成为奇数，
//                    这样的状态就是不行的，
                    if (cnt & 1) {
                        st[i] = false;
                        break;
                    }
                }else cnt++;
//                不放置小方格
            }

            if (cnt & 1) st[i] = false;
        }

//        初始化状态数组f
        memset(f, 0, sizeof f);

//        棋盘是从第0列开始，没有-1列，所以第0列第0行，不会有延伸出来的小方块
//        没有横着摆放的小方块，所有小方块都是竖着摆放的，这种状态记录为一种方案
        f[0][0] = 1;
//        遍历每一列
        for (int i = 1; i <= m; i++) {
//            枚举i列每一种状态
            for (int j = 0; j < 1 << n; j++) {
//                枚举i-1列每一种状态
                for (int k = 0; k < 1 << n; k++) {
//                    f[i-1][k] 成功转到 f[i][j]
                    if ((j & k) == 0 && st[j | k]) {
                        f[i][j] += f[i - 1][k]; //那么这种状态下它的方案数等于之前每种k状态数目的和
                    }
                }
            }
        }
//        棋盘一共有0~m-1列
//        f[i][j]表示 前i-1列的方案数已经确定，从i-1列伸出，并且第i列的状态是j的所有方案数
//        f[m][0]表示 前m-1列的方案数已经确定，从m-1列伸出，并且第m列的状态是0的所有方案数
//        也就是m列不放小方格，前m-1列已经完全摆放好并且不伸出来的状态
        cout << f[m][0] << endl;
    }
    return 0;
}

作者：松鼠爱葡萄
链接：https://www.acwing.com/solution/content/15616/

```
