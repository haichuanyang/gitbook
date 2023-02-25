# segment DP

{% embed url="https://www.acwing.com/file_system/file/content/whole/index/content/1483494/" %}



```
区间DP顾名思义就是在区间上进行DP。
一般的模板为

for(int len=1;len<=n;len++)
{
    for(int l=1;l+len-1<=n;l++)
    {
        int r=l+len-1;
        for(int k=l;k<r;k++)
        {
            .....
        }
    }
}
首先区间DP问题需要给问题进行划分区间，确定问题可以由小区间来推理出来大区间。然后进行枚举即可。

首先枚举一下区间长度，因为是从小区间推理出来大区间，所以len一般是从1~n,有时不能为1~n，因为有些具体的问题区间的范围是有最小值的，比如下面有一道例题，叫做zuma（祖玛），来源好像是CF。

那么确定了咱们的区间长度怎么枚举，那么咱们就可以接着来看怎么枚举区间了，首先枚举一下左端点，然后因为咱们的最外层循环为区间长度，所以咱们区间的右端点也就确定了。这样咱们就确定了区间。

然后要进行的就是区间的划分得到更小的区间。

就需要咱们的第三重循环k变量。

状态转移方程一般为f[l][r]=max/min(f[l][r],f[l][k],f[k+1][r]+合并区间的花费);

作者：TARDIS
链接：https://www.acwing.com/file_system/file/content/whole/index/content/1483494/
来源：AcWing
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```
