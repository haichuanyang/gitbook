---
description: 1. idea 2 coding familiarity - logic convert to code; write and debug code
---

# day9 multi-key sorting; struct overloading operator

[day 9](https://www.bilibili.com/video/BV1kp4y1x72j) - top 5 scholarship, given n students, each with 3 subject scores,  math, language arts, science. find top 5 students to assign scholarships, with total score first, if same score, the higher of math score; then the lesser of ID number if everything else is same. [https://www.acwing.com/problem/content/431/](https://www.acwing.com/problem/content/431/)



multi-key sorting, 2 methods 1) overloading the < operator; 2) self define comparator

```
const int N=310;
int n;
struct person{
    int id, sum, a,b,c;
    bool operator< (const person t) const
    {
        if(sum!=t.sum) return sum>t.sum;
        if(a!=t.a) return a>t.a;
        return id<t.id;

    }
} q[N];

int main()
{
    cin>>n;
    for (int i=1;i<=n;i++){
        int a,b,c;
        cin>>a>>b>>c;
        q[i]={i,a+b+c,a,b,c}
   }
   sort (q+1,q+n+1); //O(lg n)
   for (int i=1;i<=5;i++) cout<<q[i].id<<' ' <<q[i].sum<<endl;
   
   return 0;
}
```



self define comparator

```
struct Person{
    int id, sum, a,b,c;
    } q[N];
    
bool cmp(Person a, Person b){

    if(a.sum!=b.sum) return a.sum>b.sum;
    if(a.a !=b.a) return a.a>b.a;
    return a.id<b.id;
}

sort (q+1, q+n+1,cmp);

```



lambda expression which C++17 supports

```
sort(q+1, q+n+1, []((Person a, Person b){

    if(a.sum!=b.sum) return a.sum>b.sum;
    if(a.a !=b.a) return a.a>b.a;
    return a.id<b.id;
});

```

O(lg n)

C++ primer book; great wall is built brick by brick
