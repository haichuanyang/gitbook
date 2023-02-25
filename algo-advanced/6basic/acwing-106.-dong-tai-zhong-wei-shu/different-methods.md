# different methods

```
快速选择   47   47行   105ms   105ms
#include <stdio.h>

typedef long long ll;

const int N=10005;

int test,n;
int a[N];
ll ans[N>>1],size;

template<typename T>
T quick_search(T *begin,T *end,int k)
{
    if(begin==end-1)return *begin;
    T pivot=*(begin+(end-begin>>1));
    T *i=begin-1,*j=end;
    while(i!=j)
    {
        while(i!=j&&*++i<pivot);
        while(i!=j&&*--j>pivot);
        if(i!=j)*i^=*j,*j^=*i,*i^=*j;
    }
    if(k<=j-begin)return quick_search(begin,j,k);
    else    return quick_search(j,end,k-(j-begin));
}

int main()
{
    int T;
    for(scanf("%d",&T);T--;putchar('\n'))
    {
        size=0;
        scanf("%d%d",&test,&n);
        printf("%d %d\n",test,n+1>>1);
        for(int i=1;i<=n;i++)
        {
            scanf("%d",a+i);
            if(i&1)ans[size++]=quick_search(a+1,a+i+1,i+1>>1);
        }
        for(int i=0;i<size;i++)
        {
            if(i&&i%10==0)putchar('\n');
            printf("%d ",ans[i]);
        }
    }
    return 0;
}
对顶堆   148   148行   5ms   5ms
#include <stdio.h>

typedef long long ll;

const int N=10005;

int test,n;
ll heap1[N],size_heap1; // 大根堆
ll heap2[N],size_heap2; // 小根堆
ll ans[N],size_ans;

void down_heap1(int u)
{
    int t=u;
    if(u<<1<=size_heap1&&heap1[t]<heap1[u<<1])t=u<<1;
    if((u<<1|1)<=size_heap1&&heap1[t]<heap1[u<<1|1])t=u<<1|1;
    if(u!=t)
    {
        heap1[u]^=heap1[t];
        heap1[t]^=heap1[u];
        heap1[u]^=heap1[t];
        down_heap1(t);
    }
}

void up_heap1(int u)
{
    while(u>>1&&heap1[u]>heap1[u>>1])
    {
        heap1[u]^=heap1[u>>1];
        heap1[u>>1]^=heap1[u];
        heap1[u]^=heap1[u>>1];
        u>>=1;
    }
}

void insert_heap1(int x)
{
    heap1[++size_heap1]=x;
    up_heap1(size_heap1);
}

void erase_heap1(int u)
{
    if(u!=size_heap1)
    {
        heap1[u]^=heap1[size_heap1];
        heap1[size_heap1]^=heap1[u];
        heap1[u]^=heap1[size_heap1];
    }
    size_heap1--;
    down_heap1(u);
    up_heap1(u);
}

void down_heap2(int u)
{
    int t=u;
    if(u<<1<=size_heap2&&heap2[u<<1]<heap2[t])t=u<<1;
    if((u<<1|1)<=size_heap2&&heap2[u<<1|1]<heap2[t])t=u<<1|1;
    if(t!=u)
    {
        heap2[u]^=heap2[t];
        heap2[t]^=heap2[u];
        heap2[u]^=heap2[t];
        down_heap2(t);
    }
}

void up_heap2(int u)
{
    while(u>>1&&heap2[u>>1]>heap2[u])
    {
        heap2[u]^=heap2[u>>1];
        heap2[u>>1]^=heap2[u];
        heap2[u]^=heap2[u>>1];
        u>>=1;
    }
}

void insert_heap2(int x)
{
    heap2[++size_heap2]=x;
    up_heap2(size_heap2);
}

void erase_heap2(int u)
{
    if(u!=size_heap2)
    {
        heap2[u]^=heap2[size_heap2];
        heap2[size_heap2]^=heap2[u];
        heap2[u]^=heap2[size_heap2];
    }
    size_heap2--;
    down_heap2(u);
    up_heap2(u);
}

int main()
{
    int T;
    for(scanf("%d",&T);T--;putchar('\n'))
    {
        size_ans=0;
        size_heap1=0;
        size_heap2=0;
        scanf("%d %d",&test,&n);
        for(int i=1;i<=n;i++)
        {
            int x;
            scanf("%d",&x);
            if(i&1)
                if(x<=heap1[1])
                {
                    insert_heap1(x);
                    ans[size_ans++]=heap1[1];
                }
                else
                {
                    insert_heap2(x);
                    ans[size_ans++]=heap2[1];
                }
            else
            {
                if(size_heap1>size_heap2)insert_heap1(x);
                else    insert_heap2(x);
                if(size_heap1>size_heap2)
                {
                    insert_heap2(heap1[1]);
                    erase_heap1(1);
                }
                else if(size_heap1<size_heap2)
                {
                    insert_heap1(heap2[1]);
                    erase_heap2(1);
                }
            }
        }
        printf("%d %d\n",test,size_ans);
        for(int i=0;i<size_ans;i++)
        {
            if(i&&i%10==0)putchar('\n');
            printf("%d ",ans[i]);
        }
    }
    return 0;
}
线段树   135   135行   17ms   17ms
#include <stdio.h>

typedef long long ll;

const int N=10005;
const int M=40005;

int test,n;
struct point
{
    int l,r;
    ll sum;
}tr[M];
ll alls[N],size_alls;
ll ans[N],size_ans;
ll a[N];

template<typename T>
void sort(T *begin,T *end)
{
    if(begin==end-1)return;
    T pivot=*(begin+(end-begin>>1));
    T *i=begin-1,*j=end;
    while(i!=j)
    {
        while(i!=j&&*++i<pivot);
        while(i!=j&&*--j>pivot);
        if(i!=j)*i^=*j,*j^=*i,*i^=*j;
    }
    sort(begin,j),sort(j,end);
}

template<typename T>
int unique(T *begin,T *end)
{
    T *pivot1,*pivot2;
    pivot1=pivot2=begin;
    while(pivot1!=end)
    {
        while(pivot1!=end&&*pivot1==*pivot2)pivot1++;
        if(pivot1!=end)*++pivot2=*pivot1;
    }
    return pivot2-begin+1;
}

int find(int x)
{
    int l=1,r=size_alls;
    while(l<r)
    {
        int mid=l+r>>1;
        if(alls[mid]>=x)r=mid;
        else    l=mid+1;
    }
    return r;
}

void pushup(int u)
{
    tr[u].sum=tr[u<<1].sum+tr[u<<1|1].sum;
}

void build(int u,int l,int r)
{
    tr[u].l=l,tr[u].r=r;
    tr[u].sum=0;
    if(l==r)return;
    int mid=l+r>>1;
    build(u<<1,l,mid);
    build(u<<1|1,mid+1,r);
}

void modify(int u,int x,int c)
{
    if(tr[u].l==tr[u].r)tr[u].sum+=c;
    else
    {
        int mid=tr[u].l+tr[u].r>>1;
        if(x<=mid)modify(u<<1,x,c);
        else    modify(u<<1|1,x,c);
        pushup(u);
    }
}

ll query(int u,int l,int r)
{
    if(tr[u].l>=l&&tr[u].r<=r)return tr[u].sum;
    ll sum=0,mid=tr[u].l+tr[u].r>>1;
    if(l<=mid)sum+=query(u<<1,l,r);
    if(mid<r)sum+=query(u<<1|1,l,r);
    return sum;
}

int get_kth(int k)
{
    int l=1,r=size_alls;
    while(l<r)
    {
        int mid=l+r>>1;
        if(query(1,1,mid)>=k)r=mid;
        else    l=mid+1;
    }
    return alls[r];
}

int main()
{
    int T;
    for(scanf("%d",&T);T--;putchar('\n'))
    {
        size_alls=0;
        size_ans=0;
        scanf("%d%d",&test,&n);
        for(int i=1;i<=n;i++)
        {
            scanf("%lld",a+i);
            alls[++size_alls]=a[i];
        }
        sort(alls+1,alls+size_alls+1);
        size_alls=unique(alls+1,alls+size_alls+1);
        build(1,1,n);
        for(int i=1;i<=n;i++)
        {
            modify(1,find(a[i]),1);
            if(i&1)ans[size_ans++]=get_kth(i+1>>1);
        }
        printf("%d %d\n",test,size_ans);
        for(int i=0;i<size_ans;i++)
        {
            if(i&&i%10==0)putchar('\n');
            printf("%d ",ans[i]);
        }
    }
    return 0;
}
线段树(lazytag)   153(lazytag)   153行   27ms   27ms
#include <stdio.h>

typedef long long ll;

const int N=10005;
const int M=40005;

int test,n;
struct point
{
    int l,r;
    ll sum;
    ll lazytag;
}tr[M];
ll alls[N],size_alls;
ll ans[N],size_ans;
ll a[N];

template<typename T>
void sort(T *begin,T *end)
{
    if(begin==end-1)return;
    T pivot=*(begin+(end-begin>>1));
    T *i=begin-1,*j=end;
    while(i!=j)
    {
        while(i!=j&&*++i<pivot);
        while(i!=j&&*--j>pivot);
        if(i!=j)*i^=*j,*j^=*i,*i^=*j;
    }
    sort(begin,j),sort(j,end);
}

template<typename T>
int unique(T *begin,T *end)
{
    T *pivot1,*pivot2;
    pivot1=pivot2=begin;
    while(pivot1!=end)
    {
        while(pivot1!=end&&*pivot1==*pivot2)pivot1++;
        if(pivot1!=end)*++pivot2=*pivot1;
    }
    return pivot2-begin+1;
}

int find(int x)
{
    int l=1,r=size_alls;
    while(l<r)
    {
        int mid=l+r>>1;
        if(alls[mid]>=x)r=mid;
        else    l=mid+1;
    }
    return r;
}

void pushup(int u)
{
    tr[u].sum=tr[u<<1].sum+tr[u<<1|1].sum;
}

void pushdown(int u)
{
    if(tr[u].lazytag)
    {
        tr[u<<1].lazytag+=tr[u].lazytag;
        tr[u<<1|1].lazytag+=tr[u].lazytag;
        tr[u<<1].sum+=tr[u].lazytag*(tr[u<<1].r-tr[u<<1].l+1);
        tr[u<<1|1].sum+=tr[u].lazytag*(tr[u<<1|1].r-tr[u<<1|1].l+1);
        tr[u].lazytag=0;
    }
}

void build(int u,int l,int r)
{
    tr[u].l=l,tr[u].r=r;
    tr[u].sum=0;
    tr[u].lazytag=0;
    if(l==r)return;
    int mid=l+r>>1;
    build(u<<1,l,mid);
    build(u<<1|1,mid+1,r);
}

void modify(int u,int l,int r,ll c)
{
    if(tr[u].l>=l&&tr[u].r<=r)
    {
        tr[u].sum+=c*(tr[u].r-tr[u].l+1);
        tr[u].lazytag+=c;
        return;
    }
    pushdown(u);
    int mid=tr[u].l+tr[u].r>>1;
    if(l<=mid)modify(u<<1,l,r,c);
    if(mid<r)modify(u<<1|1,l,r,c);
    pushup(u);
}

ll query(int u,int l,int r)
{
    if(tr[u].l>=l&&tr[u].r<=r)return tr[u].sum;
    pushdown(u);
    ll sum=0,mid=tr[u].l+tr[u].r>>1;
    if(l<=mid)sum+=query(u<<1,l,r);
    if(mid<r)sum+=query(u<<1|1,l,r);
    return sum;
}

int get_kth(int k)
{
    int l=1,r=size_alls;
    while(l<r)
    {
        int mid=l+r>>1;
        if(query(1,mid,mid)>=k)r=mid;
        else    l=mid+1;
    }
    return alls[r];
}

int main()
{
    int T;
    for(scanf("%d",&T);T--;putchar('\n'))
    {
        size_alls=0;
        size_ans=0;
        scanf("%d%d",&test,&n);
        for(int i=1;i<=n;i++)
        {
            scanf("%lld",a+i);
            alls[++size_alls]=a[i];
        }
        sort(alls+1,alls+size_alls+1);
        size_alls=unique(alls+1,alls+size_alls+1);
        build(1,1,n);
        for(int i=1;i<=n;i++)
        {
            modify(1,find(a[i]),n,1);
            if(i&1)ans[size_ans++]=get_kth(i+1>>1);
        }
        printf("%d %d\n",test,size_ans);
        for(int i=0;i<size_ans;i++)
        {
            if(i&&i%10==0)putchar('\n');
            printf("%d ",ans[i]);
        }
    }
    return 0;
}
树状数组   114   114行   9ms   9ms
#include <stdio.h>
#include <string.h>

typedef long long ll;

const int N=10005;

int test,n;
ll a[N];
ll tr[N];
ll alls[N],size_alls;
ll ans[N],size_ans;

template<typename T>
void sort(T *begin,T *end)
{
    if(begin==end-1)return;
    T pivot=*(begin+(end-begin>>1));
    T *i=begin-1,*j=end;
    while(i!=j)
    {
        while(i!=j&&*++i<pivot);
        while(i!=j&&*--j>pivot);
        if(i!=j)*i^=*j,*j^=*i,*i^=*j;
    }
    sort(begin,j),sort(j,end);
}

template<typename T>
int unique(T *begin,T *end)
{
    T *pivot1,*pivot2;
    pivot1=pivot2=begin;
    while(pivot1!=end)
    {
        while(pivot1!=end&&*pivot1==*pivot2)pivot1++;
        if(pivot1!=end)*++pivot2=*pivot1;
    }
    return pivot2-begin+1;
}

template<typename T>
int find(T x)
{
    int l=1,r=size_alls;
    while(l<r)
    {
        int mid=l+r>>1;
        if(alls[mid]>=x)r=mid;
        else    l=mid+1;
    }
    return r;
}

ll lowbit(ll x)
{
    return x&-x;
}

void modify(int x,ll c)
{
    for(int i=x;i<=n;i+=lowbit(i))
        tr[i]+=c;
}

ll query(int x)
{
    ll res=0;
    for(int i=x;i;i-=lowbit(i))
        res+=tr[i];
    return res;
}

int get_kth(ll k)
{
    int l=1,r=size_alls;
    while(l<r)
    {
        int mid=l+r>>1;
        if(query(mid)>=k)r=mid;
        else    l=mid+1;
    }
    return alls[r];
}

int main()
{
    int T;
    for(scanf("%d",&T);T--;putchar('\n'))
    {
        size_alls=size_ans=0;
        memset(tr,0,sizeof tr);
        scanf("%d%d",&test,&n);
        for(int i=1;i<=n;i++)
        {
            scanf("%lld",a+i);
            alls[++size_alls]=a[i];
        }
        sort(alls+1,alls+size_alls+1);
        size_alls=unique(alls+1,alls+size_alls+1);
        for(int i=1;i<=n;i++)
        {
            modify(find(a[i]),1);
            if(i&1)ans[size_ans++]=get_kth(i+1>>1);
        }
        printf("%d %d\n",test,size_ans);
        for(int i=0;i<size_ans;i++)
        {
            if(i&&i%10==0)putchar('\n');
            printf("%d ",ans[i]);
        }
    }
    return 0;
}

作者：垫底抽风
链接：https://www.acwing.com/activity/content/code/content/362841/

```
