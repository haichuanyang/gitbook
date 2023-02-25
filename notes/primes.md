# primes

```
Cba(分解质因数)求分子分解每个p的次数cntmi(a!)为什么只+1,因为a中pk低阶项的同理可类推到pk它在计同理可求分母的p的次数,=a×(a−1)×…×(a−b+1)b×(b−1)×…×1=a!b!(a−b)!=pα11pα22…pαkk=[ap]下取整+[ap2]下取整+[ap3]下取整+…=p∈[1,a]的倍数的个数+p2∈[1,a]的倍数的个数+…次数已经被算过了比如p3既是p的倍数也是p2的倍数算p1∼pk−1次数中都已经算过1次了分子次数和−分母次数和
 

具体:

筛素数(1~5000)
求每个质数的次数
用高精度乘把所有质因子乘上
#include<iostream>
#include<algorithm>
#include<vector>

using namespace std;

const int N=5010;

int primes[N],cnt;
int sum[N];
bool st[N];

void get_primes(int n)
{
    for(int i=2;i<=n;i++)
    {
        if(!st[i])primes[cnt++]=i;
        for(int j=0;primes[j]*i<=n;j++)
        {
            st[primes[j]*i]=true;
            if(i%primes[j]==0)break;//==0每次漏
        }
    }
}
// 对p的各个<=a的次数算整除下取整倍数
int get(int n,int p)
{
    int res =0;
    while(n)
    {
        res+=n/p;
        n/=p;
    }
    return res;
}
//高精度乘
vector<int> mul(vector<int> a, int b)
{
    vector<int> c;
    int t = 0;
    for (int i = 0; i < a.size(); i ++ )
    {
        t += a[i] * b;
        c.push_back(t % 10);
        t /= 10;
    }
    while (t)
    {
        c.push_back(t % 10);
        t /= 10;
    }
    // while(C.size()>1 && C.back()==0) C.pop_back();//考虑b==0时才有pop多余的0 b!=0不需要这行
    return c;
}

int main()
{
    int a,b;
    cin >> a >> b;
    get_primes(a);

    for(int i=0;i<cnt;i++)
    {
        int p = primes[i];
        sum[i] = get(a,p)-get(a-b,p)-get(b,p);//是a-b不是b-a
    }

    vector<int> res;
    res.push_back(1);

    for (int i = 0; i < cnt; i ++ )
        for (int j = 0; j < sum[i]; j ++ )//primes[i]的次数
            res = mul(res, primes[i]);

    for (int i = res.size() - 1; i >= 0; i -- ) printf("%d", res[i]);
    puts("");

    return 0;
}
```
