# merge segments



```cpp
思路提示
可以先按左端点排序，再维护一个区间，与后面一个个区间进行三种情况的比较，存储到数组里去。

C++ 代码
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std ;
typedef pair<int,int> pii ;
vector<pii>nums,res ;
int main()
{
    int st=-2e9,ed=-2e9 ;                           //ed代表区间结尾，st代表区间开头
    int n ;
    scanf("%d",&n) ; 
    while(n--)
    {
        int l,r ; 
        scanf("%d%d",&l,&r) ;
        nums.push_back({l,r}) ;
    }
    sort(nums.begin(),nums.end()) ;                 //按左端点排序
    for(auto num:nums)                   
    {
        if(ed<num.first)                            //情况1：两个区间无法合并
        {
            if(ed!=-2e9) res.push_back({st,ed}) ;   //区间1放进res数组
            st=num.first,ed=num.second ;            //维护区间2
        }
        //情况2：两个区间可以合并，且区间1不包含区间2，区间2不包含区间1
        else if(ed<num.second)  
            ed=num.second ;                         //区间合并
    }  
    //(实际上也有情况3：区间1包含区间2，此时不需要任何操作，可以省略)

    //注：排过序之后，不可能有区间2包含区间1

    if(st!=-2e9&&ed!=-2e9) res.push_back({st,ed});

    //剩下还有一个序列，但循环中没有放进res数组，因为它是序列中的最后一个序列

    /*
    for(auto r:res)
        printf("%d %d\n",r.first,r.second) ;
    puts("") ;
    */

    //(把上面的注释去掉，可以在调试时用)

    printf("%d",res.size()) ;           //输出答案
    return 0 ;
}

作者：李乾
链接：https://www.acwing.com/solution/content/2615/

```
