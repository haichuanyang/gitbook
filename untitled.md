---
description: leetcode 1504
---

# number of submatrices of 1's

[https://leetcode.com/problems/count-submatrices-with-all-ones/discuss/?currentPage=1\&orderBy=most\_votes\&query=](https://leetcode.com/problems/count-submatrices-with-all-ones/discuss/?currentPage=1\&orderBy=most\_votes\&query=) sort by vote!

[https://www.acwing.com/file\_system/file/content/whole/index/content/1075030/](https://www.acwing.com/file\_system/file/content/whole/index/content/1075030/) acwing presum solution

[https://leetcode.com/problems/count-submatrices-with-all-ones/discuss/?currentPage=1\&orderBy=hot\&query=](https://leetcode.com/problems/count-submatrices-with-all-ones/discuss/?currentPage=1\&orderBy=hot\&query=) leetcode 1504 discuss has many solutions



[https://www.acwing.com/solution/content/35261/](https://www.acwing.com/solution/content/35261/) 3054 AcWing 3054. 与或和



{% embed url="https://leetcode.com/problems/count-submatrices-with-all-ones/" %}



{% embed url="https://www.geeksforgeeks.org/number-of-submatrices-with-all-1s/" %}



```
LeetCode 1504. Count Submatrices With All Ones    原题链接    中等
作者：    zyq2652192993 ,  2020-07-05 14:46:45 ,  阅读 1144

1


题目描述
Given a rows * columns matrix mat of ones and zeros, return how many submatrices have all ones.

样例
Example 1:

Input: mat = [[1,0,1],
              [1,1,0],
              [1,1,0]]
Output: 13
Explanation:
There are 6 rectangles of side 1x1.
There are 2 rectangles of side 1x2.
There are 3 rectangles of side 2x1.
There is 1 rectangle of side 2x2. 
There is 1 rectangle of side 3x1.
Total number of rectangles = 6 + 2 + 3 + 1 + 1 = 13.
Example 2:

Input: mat = [[0,1,1,0],
              [0,1,1,1],
              [1,1,1,0]]
Output: 24
Explanation:
There are 8 rectangles of side 1x1.
There are 5 rectangles of side 1x2.
There are 2 rectangles of side 1x3. 
There are 4 rectangles of side 2x1.
There are 2 rectangles of side 2x2. 
There are 2 rectangles of side 3x1. 
There is 1 rectangle of side 3x2. 
Total number of rectangles = 8 + 5 + 2 + 4 + 2 + 2 + 1 = 24.
Example 3:

Input: mat = [[1,1,1,1,1,1]]
Output: 21
Example 4:

Input: mat = [[1,0,1],[0,1,0],[1,0,1]]
Output: 5
Constraints:

1 <= rows <= 150
1 <= columns <= 150
0 <= mat[i][j] <= 1
算法
(二维前缀和) O(m2n)O(m2n)
这个题很容易联想到LeetCode 1277.Count Square Submatrices with All Ones，不妨在这里总结一下以子矩阵（子矩形/正方形）相关的问题方法：

求子矩阵和：304.Range Sum Query 2D - Immutable（矩阵不修改，二维前缀和）/308. Range Sum Query 2D - Mutable（矩阵可修改，线段树）
求最大子矩形/求最大正方形：85. Maximal Rectangle（单调栈）/221. Maximal Square（动态规划）
统计子矩形/正方形的个数：1504.Count Submatrices With All Ones（本题，二维前缀和）/1277.Count Square Submatrices with All Ones（动态规划）
当矩阵不是01矩阵的子矩形问题：求最大子矩形和（HDU 1081 To The Max，二维压缩成一维的最大连续子数组和）/矩形区域不超过 K 的最大数值和（363.Max Sum of Rectangle No Larger Than K，相当于在HDU 1081的基础上增加二分的方法）
本题其实只需要掌握二维前缀和即可。首先处理出二维前缀和存储在preSum里，通过函数getSum可以在O(1)O(1)的时间内得到以x1, y1为左上角，以x2, y2为右下角的矩阵的和。

如果一个子矩阵里全是1，那么它的和一定是这个子矩阵的长度乘上宽度。从第一行开始枚举，子矩阵的起始行设为row，然后枚举子矩阵的竖向长度len，那么子矩阵的结尾行是row + len - 1，子矩阵的列设为col，然后去寻找竖向长度为len的列最远可以扩展的位置，那么这个位置减去起始列col的结果就是子矩阵的横向长度（设为length），那么竖向长度是len的子矩阵就有(1 + length) * length / 2个，这样就保证了不重不漏。

时间复杂度O(m2n)O(m2n)。

C++ 代码
class Solution {
public:
    int numSubmat(vector<vector<int>>& matrix) {
        std::ios_base::sync_with_stdio(false);
        cin.tie(NULL);
        cout.tie(NULL);

        int m = matrix.size(), n = matrix[0].size();
        vector<vector<int>> preSum(m + 1, vector<int>(n + 1, 0));

        for (int i = 1; i <= m; ++i) {
            for (int j = 1; j <= n; ++j) {
                preSum[i][j] = preSum[i - 1][j] + preSum[i][j- 1] - preSum[i - 1][j - 1] + matrix[i - 1][j - 1];
            }
        }

        int res = 0;
        for (int row = 1; row <= m; ++row) {
            for (int len = 1; row + len - 1 <= m; ++len) {
                for (int col = 1; col <= n; ++col) {
                    if (getSum(preSum, row, col, row + len - 1, col) == len) {
                        int tmp = col;
                        while (tmp <= n && getSum(preSum, row, tmp, row + len - 1, tmp) == len) ++tmp;
                        int length = tmp - col;
                        res += (1 + length) * length / 2;
                        col = tmp - 1;
                    }
                }
            }
        }

        return res;
    }

    inline int getSum(vector<vector<int>> & preSum, int x1, int y1, int x2, int y2)
    {
        return preSum[x2][y2] - preSum[x1 - 1][y2] - preSum[x2][y1 - 1] + preSum[x1 - 1][y1 - 1];
    }

作者：zyq2652192993
链接：https://www.acwing.com/file_system/file/content/whole/index/content/1075030/
来源：AcWing
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```
