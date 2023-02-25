# day5 base conversion for numbers

p1346 usaco training palindrome numbers p124 base a to base b conversion

short division - base conversion

```
题目描述
编写一个程序，可以实现将一个数字由一个进制转换为另一个进制。

这里有62个不同数位{0-9,A-Z,a-z}。

输入格式
第一行输入一个整数，代表接下来的行数。

接下来每一行都包含三个数字，首先是输入进制（十进制表示），然后是输出进制（十进制表示），最后是用输入进制表示的输入数字，数字之间用空格隔开。

输入进制和输出进制都在2到62的范围之内。

（在十进制下）A = 10，B = 11，…，Z = 35，a = 36，b = 37，…，z = 61 (0-9仍然表示0-9)。

输出格式
对于每一组进制转换，程序的输出都由三行构成。

第一行包含两个数字，首先是输入进制（十进制表示），然后是用输入进制表示的输入数字。

第二行包含两个数字，首先是输出进制（十进制表示），然后是用输出进制表示的输入数字。

第三行为空白行。

同一行内数字用空格隔开。

样例
输入样例：
8
62 2 abcdefghiz
10 16 1234567890123456789012345678901234567890
16 35 3A0C92075C0DBF3B8ACBC5F96CE3F0AD2
35 23 333YMHOUE8JPLT7OX6K9FYCQ8A
23 49 946B9AA02MI37E3D3MMJ4G7BL2F05
49 61 1VbDkSIMJL3JjRgAdlUfcaWj
61 5 dl9MDSWqwHjDnToKcsWE1S
5 10 42104444441001414401221302402201233340311104212022133030
输出样例：
62 abcdefghiz
2 11011100000100010111110010010110011111001001100011010010001

10 1234567890123456789012345678901234567890
16 3A0C92075C0DBF3B8ACBC5F96CE3F0AD2

16 3A0C92075C0DBF3B8ACBC5F96CE3F0AD2
35 333YMHOUE8JPLT7OX6K9FYCQ8A

35 333YMHOUE8JPLT7OX6K9FYCQ8A
23 946B9AA02MI37E3D3MMJ4G7BL2F05

23 946B9AA02MI37E3D3MMJ4G7BL2F05
49 1VbDkSIMJL3JjRgAdlUfcaWj

49 1VbDkSIMJL3JjRgAdlUfcaWj
61 dl9MDSWqwHjDnToKcsWE1S

61 dl9MDSWqwHjDnToKcsWE1S
5 42104444441001414401221302402201233340311104212022133030

5 42104444441001414401221302402201233340311104212022133030
10 1234567890123456789012345678901234567890
高精度+进制转换
相信一部分的大佬们,都会认为这道题目要先转移到十进制,然后再转移到目标进制.不过这里提出一个比较有趣的精简代码,我们发现其实这个初始进制到十进制是可以省略到的,我们可以直接从初始进制运算到到目标进制,十进制只是一个跳板.
高精度,就是高精度乘上低精度的乘法,因为进制转换需要用到
C++ 代码
#include <bits/stdc++.h>
using namespace std;
const int maxn = 1100;
#define fir(a,b,c) for (int a=b;a<=c;a++)
int t[maxn],A[maxn],n,m,i,len,k;
char str1[maxn], str2[maxn];
void solve()
{
    k=0;
    len = strlen(str1);
    for(i=len; i>=0; i--) 
        t[len-i-1] = str1[i] -(str1[i]<58 ? 48: str1[i]<97 ? 55: 61);//开始转换成为十进制
    while(len)
    {
        for(i=len; i>=1; i--) 
        {
            t[i-1] +=t[i]%m*n;//开始计算,首先*n,为n进制,然后转移到m进制
            t[i] /= m;//当前数已经处理完了
        }
        A[k++] = t[0] % m;//直接加到答案数组中
        t[0] /=m;//当前数已经处理完了
        while(len>0&&!t[len-1])  //处理0的情况
            len--;
    }
    str2[k] =NULL;//第k位,我们不需要,直接抛去,利用NULL这个特殊的字符串结束符,具体可以自行百度.
    fir(i,0,k-1)
        str2[k-1-i] = A[i]+(A[i]<10 ? 48: A[i]<36 ? 55:61);//三目运算符,具体可以百度,这段代码的意思是,找这个数字对应的字母
}

int main()
{
    int t;
    scanf("%d\n",&t);
    while(t--) 
    {
        scanf("%d %d %s\n",&n,&m,str1);
        solve();
        printf("%d %s\n%d %s\n\n",n,str1,m,str2);
    }
    return 0;
}

作者：秦淮岸灯火阑珊
链接：https://www.acwing.com/solution/content/859/

```

```
首先想到一种朴素做法就是将读入进来的数转换成十进制，再将其转换成所需进制。

但是C++并不自带高精度，所以我们需要手写高精度。

观察将数转换成所需进制的过程， 发现确定每一位数时只需要得到这个数除以所需进制数的余数即可。

一个数不论以何种进制表示，其除以一个数的余数都是一样的。

所以可以直接将某进制的数除以另一进制的进制数，以此得到余数，构建所需进制的数。

细节不再多说，核心代码在此：

for(int i=len;i>=1;--i)//nxt是所需进制数， pre是原进制数
{
    int b=(x*pre+a[i])/nxt;
    x=(x*pre+a[i])%nxt;
    a[i]=b;
}
C++ 代码
#include<bits/stdc++.h>
using namespace std;
int pre,nxt;
char s[1010011];
int  a[1010011];

void work()
{
    // string -> number
    int len=strlen(s);
    for(int i=0;i<len;++i)
    {
        if(s[i]>='0'&&s[i]<='9') a[len-i]=s[i]-'0';
        if(s[i]>='A'&&s[i]<='Z') a[len-i]=s[i]-'A'+10;
        if(s[i]>='a'&&s[i]<='z') a[len-i]=s[i]-'a'+36;
    }
    stack<char>sta;
    while(len)
    {
        int x=0;
        for(int i=len;i>=1;--i)//核心代码，高精除低精（除nxt）
        {
            int b=(x*pre+a[i])/nxt;
            x=(x*pre+a[i])%nxt;
            a[i]=b;
        }
        while(!a[len]&&len>=1) --len;
        if(x>=0&&x<=9) sta.push((char)(x+'0'));
        if(x>=10&&x<=35) sta.push((char)(x-10+'A'));
        if(x>=36&&x<=61) sta.push((char)(x-36+'a'));
    }
    while(!sta.empty()) {
        cout<<sta.top();
        sta.pop();
    }
    return;
}

int main()
{
    int T; cin >> T;
    while(T--)
    {
        scanf("%d%d%s",&pre,&nxt,s);
        printf("%d %s\n",pre,s);
        printf("%d ",nxt);
        work();
        cout<<"\n\n";
    }
    return 0;
 } 

作者：xwmwr
链接：https://www.acwing.com/solution/content/5922/

```
