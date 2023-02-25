---
description: list out method; 打表法 Kim larsen calculation formula
---

# day12 leap year/weekdays count

```
时间复杂度O(n)

C++写法一
#include<iostream>
using namespace std;
int res[7];
int main()
{
    int n,day=-1;
    int leap_year[12]={31,29,31,30,31,30,31,31,30,31,30,31};
    int common_year[12]={31,28,31,30,31,30,31,31,30,31,30,31};
    scanf("%d",&n);
    for(int i=1900;i<=1900+n-1;i++)
    {
        bool is_leap_year=(i%4==0 && i%100) || i%400==0;
        for(int j=1;j<=12;j++)
        {
            if(is_leap_year) 
            {
                for(int k=1;k<=leap_year[j-1];k++)
                {
                    day=(day+1)%7;
                    if(k==13) res[day]++;
                }
            }
            else
            {
                for(int k=1;k<=common_year[j-1];k++)
                {
                    day=(day+1)%7;
                    if(k==13) res[day]++;
                }
            }
        }
    }
    printf("%d %d ",res[5],res[6]);
    for(int i=0;i<=4;i++) printf("%d ",res[i]);
}
C++写法2 (修改了输出)
#include<iostream>
using namespace std;
int res[7];
int main()
{
    int n,day=-1;
    int leap_year[12]={31,29,31,30,31,30,31,31,30,31,30,31};
    int common_year[12]={31,28,31,30,31,30,31,31,30,31,30,31};
    scanf("%d",&n);
    for(int i=1900;i<=1900+n-1;i++)
    {
        bool is_leap_year=(i%4==0 && i%100) || i%400==0;
        for(int j=1;j<=12;j++)
        {
            if(is_leap_year) 
            {
                for(int k=1;k<=leap_year[j-1];k++)
                {
                    day=(day+1)%7;
                    if(k==13) res[day]++;
                }
            }
            else
            {
                for(int k=1;k<=common_year[j-1];k++)
                {
                    day=(day+1)%7;
                    if(k==13) res[day]++;
                }
            }
        }
    }
    for(int i=0;i<7;i++) printf("%d ",res[(i+5)%7]);
}
Java
import java.util.Scanner;
public class Main
{
    public static void main(String[] args)
    {
        int[] res= new int[7];
        int n,day=-1;
        int[] leap_year={31,29,31,30,31,30,31,31,30,31,30,31};
        int[] common_year={31,28,31,30,31,30,31,31,30,31,30,31};
        Scanner input=new Scanner(System.in);
        n=input.nextInt();
        for(int i=1900;i<=1900+n-1;i++)
        {
            boolean is_leap_year= (i%4==0 && i%100!=0) || i%400==0;
            for(int j=1;j<=12;j++)
            {
                if(is_leap_year) 
                {
                    for(int k=1;k<=leap_year[j-1];k++)
                    {
                        day=(day+1)%7;
                        if(k==13) res[day]++;
                    }
                }
                else
                {
                    for(int k=1;k<=common_year[j-1];k++)
                    {
                        day=(day+1)%7;
                        if(k==13) res[day]++;
                    }
                }
            }
        }
        for(int i=0;i<7;i++) System.out.printf("%d ",res[(i+5)%7]);
    }
}
Python3

if __name__ == "__main__":
        res= [0,0,0,0,0,0,0]
        day=-1
        leap_year=[31,29,31,30,31,30,31,31,30,31,30,31]
        common_year=[31,28,31,30,31,30,31,31,30,31,30,31]
        n=int(input().split()[0])
        for i in range(1900,1900+n):
            is_leap_year= (i%4==0 and i%100!=0) or i%400==0
            for j in range(1,13):
                if is_leap_year:
                    for k in range(1,leap_year[j-1]+1):
                        day=(day+1)%7
                        if k==13:
                            res[day]+=1
                else:
                    for k in range(1,common_year[j-1]+1):
                        day=(day+1)%7
                        if k==13:
                            res[day]+=1

        for i in range(7):
            print(f"""{res[(i+5)%7]} """,end="")


作者：Struggle
链接：https://www.acwing.com/solution/content/30707/

```

```
AcWing 436. 立体图    原题链接    简单

算法
(字符串处理,模拟,坐标变换) O(42nmh)O(42nmh)
首先将一个小正方体的投影画出来：

char box[6][8] = {
    "..+---+",
    "./   /|",
    "+---+ |",
    "|   | +",
    "|   |/.",
    "+---+.."
};
然后为了正确处理遮挡关系，按照从后到前、从左到右、从下到上的顺序画每个小正方体即可。

两个坐标系如下所示，我们将最靠左、前、下的小立方体，和二维坐标系中左下角的点(499,0)(499,0)对齐。



然后观察可以发现二者的映射关系：设小立方体在 (x,y,z)(x,y,z) ，即第 xx 行第 yy 列第 zz 层， 则对于投影后的坐标而言：

横坐标：zz 每变1，横坐标变3，xx 每变1，横坐标变2；
纵坐标：xx 每变1，总坐标变2，yy 每变1，纵坐标变4
因此投影之后的左下角坐标是：

横坐标是 499−3z−2(n−1−x)499−3z−2(n−1−x)
纵坐标是 2(n−1−x)+4y2(n−1−x)+4y
接下来还需要确定投影之后的二维平面的范围。

最后将二维投影平面的范围内的部分输出即可。

时间复杂度
每个小立方体均需要画一次，每次需要画 6 * 7 = 42个字符，因此总时间复杂度是 O(42nmh)O(42nmh)，其中 n,m,hn,m,h 分别表示长宽高。

C++ 代码
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 500;

int n, m;
char box[6][8] = {
    "..+---+",
    "./   /|",
    "+---+ |",
    "|   | +",
    "|   |/.",
    "+---+.."
};
char g[N][N];
int h[N][N];

int main()
{
    scanf("%d%d", &n, &m);
    for (int i = 0; i < n; i++)
        for (int j = 0; j < m; j++)
            scanf("%d", &h[i][j]);

    for (int i = 0; i < N; i++)
        for (int j = 0; j < N; j++)
            g[i][j] = '.';

    int up = N, right = 0;
    for (int x = 0; x < n; x ++ )
        for (int y = 0; y < m; y ++ )
            for (int z = 0; z < h[x][y]; z++)
            {
                int X = 499 - 2 * (n - 1 - x) - 3 * z;
                int Y = 2 * (n - 1 - x) + 4 * y;
                up = min(up, X - 5);
                right = max(right, Y + 6);
                for (int a = 0; a < 6; a++)
                    for (int b = 0; b < 7; b++)
                        if (box[a][b] != '.')
                            g[X - 5 + a][Y + b] = box[a][b];
            }

    for (int i = up; i < N; i++)
    {
        for (int j = 0; j <= right; j++) printf("%c", g[i][j]);
        puts("");
    }

    return 0;
}

leetcode 273
https://zxi.mytechroad.com/blog/string/leetcode-273-integer-to-english-words/

```

```

// Author: Huahua
constexpr char * kUnder20[] = {"One", "Two", "Three", "Four", "Five", 
                               "Six", "Seven", "Eight", "Nine","Ten",
                               "Eleven", "Twelve", "Thirteen", "Fourteen",
                               "Fifteen", "Sixteen", "Seventeen", "Eighteen", 
                               "Nineteen"};
constexpr char * kUnder100[] = {"Twenty", "Thirty", "Forty", "Fifty", 
                                "Sixty", "Seventy", "Eighty", "Ninety"};
constexpr char * kHTMB[] = {"Hundred", "Thousand", "Million", "Billion"};
constexpr long kP[] = {100, 1000, 1000*1000, 1000*1000*1000};
 
class Solution {
public:
  string numberToWords(int n) {
    if (n == 0) return "Zero";
    return convert(n).substr(1);
  }
private:    
  string convert(int n) {
    if (n == 0) return "";
    if (n < 20) return string(" ") + kUnder20[n - 1]; // 1 - 19
    if (n < 100) return string(" ") + kUnder100[n / 10 - 2] + convert(n % 10); // 20 ~ 99
    for (int i = 3; i >= 0; --i)
      if (n >= kP[i]) return convert(n / kP[i]) + " " + kHTMB[i] + convert(n % kP[i]);
    return "";
  }
};
```

```
AcWing 1341. 十三号星期五：模拟法 + 公式法

方法一：
模拟法：
从1900年开始，按天递推到最后一天。
中间根据年份，月份算出每个月的天数。
遍历一边，如果日期是 13 号，对应的星期数 加 1。

//cpp
#include <iostream>
using namespace std;

int main()
{
    int n;
    cin >> n;
    int a[7] = {0};//记录星期出现次数
    int week = 1;
    for (int y = 1900; y < 1900 + n ; y++)
    {
        for (int m = 1; m <= 12; m++)
        {
            int d;
            if (m == 1 || m == 3 || m == 5 || m == 7 || m == 8 || m == 10 || m == 12)//根据年与月算该月有多少天
            {
                d = 31;
            }
            else if (m == 2)
            {
                if ((y % 100 != 0 && y % 4 == 0) || (y % 400 == 0))
                    d = 29;
                else d = 28;
            }
            else
                d = 30;
            for (int i = 1; i <= d; i++)
            {
                week++;
                if (i == 13)//如果是 13 号，对应的星期出现次数加 1 
                    a[week % 7] ++;

            }
        }
    }
    for (int i = 0; i < 7; i++) cout << a[i] << " ";
    cin >> n;
}
时间复杂度 O(365n)

方法二：
基母拉尔森公式： https://baike.so.com/doc/2738265-2890169.html

W= (d+2m+3(m+1)/5+y+y/4-y/100+y/400+1)%7 //C++计算公式

在公式中d表示日期中的日数，m表示月份数，y表示年数。

注意:在公式中有个与其他公式不同的地方:

把一月和二月看成是上一年的十三月和十四月，例:如果是2004-1-10则换算成:2003-13-10来代入公式计算。
输入年月日到公式，返回当天的星期数。

//cpp
#include <iostream>
using namespace std;
int week(int y, int m, int d)
{
    if (m == 1 || m == 2)
        m = m + 12, y--;
    return (d + 2 * m + 3 * (m + 1) / 5 + y + y / 4 - y / 100 + y / 400 + 1) % 7;//公式
}
int main()
{
    int n = 0;
    cin >> n;
    int weekday[7] = {0};//存储星期的天数
    for (int y = 1900; y < 1900 + n; y++)
        for (int m = 1; m <= 12; m++)
        {
            int w = week(y, m, 13);//公式计算每一年，每一月的13号是星期几
            weekday[w]++;//对应星期天数加 1
        }

    for(int i = 6; i < 6 + 7; i++)
        cout << weekday[i % 7] << " ";
    return 0;
}
时间复杂度 O(12n)

作者：猿六
链接：https://www.acwing.com/solution/content/30656/

```
