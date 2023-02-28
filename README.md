---
description: set1map2 -std=c++11
---

# algorithm basic

[https://codeforces.com/blog/entry/15643](https://codeforces.com/blog/entry/15643) only works with -std=c++11

```
g++ t2.cc -std=c++11
```

otherwise no longer works many past shortcut syntaxes:

```cpp
res.push_back({st,ed}) ; throws error expected expression now in g++ C++11!
expected expression; swp.push_back(make_pair(a,b)); make_pair still works!
```

vector\<PII> swp;  works for push\_back() above but not for direct index accessing below:

then trying to assign values like below will give seg fault errors:

```
for(int i=0; i<kk; i++) {
    cin>>a>>b;
    //zsh: segmentation fault  ./a.out < aa.in
    swp[i].first=a;
    swp[i].second=b;
}
```

need to assign vector size first: vector\<PII> swp (N); much more similar to array now in C++ 11.

```
rsdata@rsdatas-MacBook-Pro ~ % g++ --version
Configured with: --prefix=/Library/Developer/CommandLineTools/usr --with-gxx-include-dir=/Library/Developer/CommandLineTools/SDKs/MacOSX.sdk/usr/include/c++/4.2.1
Apple clang version 11.0.3 (clang-1103.0.32.29)
Target: x86_64-apple-darwin19.6.0
Thread model: posix
InstalledDir: /Library/Developer/CommandLineTools/usr/bin

```











1. $$n\le30$$ (exponential) dfs with pruning, state compression dp
2. $$n\le100\Rightarrow O(n^3)$$floyd, dp, gaussian elimination
3. $$n\le1000\Rightarrow O(n^2), O(n^2logn)$$dp, 2 split, naive dijkstra, naive prim, bellman-ford
4. $$n\le10000\Rightarrow O(n*\sqrt n)$$ block shaped link list, decompose, Mo's algorithm
5. $$n\le100000 \Rightarrow O(n log n)$$varIous sort, segment tree, set/map, heap, topo sort, dijkstra+heap, prim+heap, spfa, convex hull, half-plane intersection, 2 split, CDQ divide-and-conquer, whole 2 spli
6. $$n\le1000000 \Rightarrow O(n)$$small c\*O(n log n), mono queue, hash, double pointer scan, union find, KMP, AC automata,  small c\* O(n log n): sort, tree shape array, heap, dijkstra, spfa
7. $$n\le10000000 \Rightarrow O(n)$$double pointer scan, KMP, AC automata, linear sieve prime
8. $$n \le 10^9 \Rightarrow O(\sqrt n)$$checking prime
9. $$n \le 10 ^ {18} \Rightarrow O(log n)$$Greatest Common Divisor, quick exponentiation
10. $$n \le 10^{1000} \Rightarrow O((log n)^2)$$arbitrary add/subtract/multiply/divide
11. $$n \le 10^{100000} \Rightarrow O(log k \times log k)$$where k is number of digits, arbitrary precision add/subtract, FFT/NTT



[https://oi-wiki.org/ds/decompose/](https://oi-wiki.org/ds/decompose/) block decomposition, [https://oi-wiki.org/misc/mo-algo/](https://oi-wiki.org/misc/mo-algo/) mo algorithm&#x20;

[https://cp-algorithms.com/data\_structures/sqrt\_decomposition.html](https://cp-algorithms.com/data\_structures/sqrt\_decomposition.html) sqrt decomposition





```
#include <iostream>
using namespace std;

typedef long long LL; 

const int N = 10010;
int n, m;
LL f[N];

int main() // 完全背包问题求解总方案数
{
    cin >> n >> m;
    f[0] = 1; 
    for (int i = 0; i < n; ++i) {
        int v;
        cin >> v;
        for (int j = v; j <= m; ++j) {
            f[j] += f[j - v];
        }
    }
    cout << f[m] << endl;
    return 0;
}

//p1021


题目描述
给你一个n种面值的货币系统，求组成面值为m的货币有多少种方案。

算法：完全背包
时间复杂度O(N∗MN∗M)
需要注意
(1)状态表示为体积恰好为j的方案的数量,需要对f[0]进行初始化
(2)答案可能会爆int,需要用long long 存储答案
C++代码
#include<iostream>

using namespace std;
typedef long long LL;
const int N=20,M=3010;
int w[N];
LL f[M];

int main()
{
    int n,m;
    cin>>n>>m;

    f[0]=1;
    for(int i=1;i<=n;i++){
        scanf("%d",&w[i]);
        for(int j=w[i];j<=m;j++){
            f[j]+=f[j-w[i]];
        }
    }
    cout<<f[m]<<endl;
    return 0;
}


```



```
//4. combo number via factoring into prime numbers;
//can get the exact result, no modulo used; no qmi!
//分解质因数法求组合数 —— 模板题 AcWing 888. 求组合数 IV
//当我们需要求出组合数的真实值，而非对某个数的余数时，分解质因数的方式比较好用：
//    1. 筛法求出范围内的所有质数
//    2. 通过 C(a, b) = a! / b! / (a - b)! 这个公式求出每个质因子的次数。 n! 中p的次数是 n / p + n / p^2 + n / p^3 + ...
//    3. 用高精度乘法将所有质因子相乘

int primes[N], cnt;     // 存储所有质数
int sum[N];     // 存储每个质数的次数
bool st[N];     // 存储每个数是否已被筛掉


void get_primes(int n)      // 线性筛法求素数
{
    for (int i = 2; i <= n; i ++ )
    {
        if (!st[i]) primes[cnt ++ ] = i;
        for (int j = 0; primes[j] <= n / i; j ++ )
        {
            st[primes[j] * i] = true;
            if (i % primes[j] == 0) break;
        }
    }
}


int get(int n, int p)       // 求n！中的次数
{
    int res = 0;
    while (n)
    {
        res += n / p;
        n /= p;
    }
    return res;
}


vector<int> mul(vector<int> a, int b)       // 高精度乘低精度模板
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

    return c;
}

get_primes(a);  // 预处理范围内的所有质数

for (int i = 0; i < cnt; i ++ )     // 求每个质因数的次数
{
    int p = primes[i];
    sum[i] = get(a, p) - get(b, p) - get(a - b, p);
}

vector<int> res;
res.push_back(1);

for (int i = 0; i < cnt; i ++ )     // 用高精度乘法将所有质因子相乘
    for (int j = 0; j < sum[i]; j ++ )
        res = mul(res, primes[i]);


2.				//union find with distance info

		int p[N], d[N];
    //p[] store ancestor node for each node
    //d[x] store distance from x to p[x] 

		// return ancestor node for x
		int find(int x)
		{
			if (p[x] != x)
			{
				int t = find(p[x]);
				d[x] += d[p[x]];
				p[x] = t;
			}
			return p[x];
		}

		// init, assuming node index 1 to n
		for (int i = 1; i <= n; i ++ )
		{
			p[i] = i;
			d[i] = 0;
		}

		// union of sets a and b 
		p[find(a)] = find(b);
		d[find(a)] = distance; // init find(a) offset according to the specifics of the problem

		
use pen and paper to figure out first, especially DP, need to write down
transition equation first

next_permutation() 
prev_permutation()
```

### binary exponentiation and multiplication

```cpp
#include <bits/stdc++.h>
using namespace std;
#define lowbit(S) ((S) & -(S))

//binary exponentiation
//turns exponentiation to multiplication
//same idea can turn multiplication to addition
//get m^k%p in time O(logk)。

//get x^y in log(y) time by converting power to multiplication

typedef long long ll;
ll qexp(ll x, ll y){
        ll ans=1;
        while(y){
                if(y&1) ans=ans*x;
                x*=x;
                y>>=1;
        }
        return ans;
}

//get x*y in log(y) time by converting multiplication to addition
ll qmulti(ll x, ll y){
        ll ans=0;
        while(y){
                if(y&1) ans=ans+x;
                x+=x;
                y>>=1;
        }
        return ans;
}

int qmi(int m, int k, int p)
{
    int res = 1%p, t = m;
    while (k)
    {
        if (k&1) res = res * t % p;
        t = t * t % p;
        k >>= 1;
    }
    return res;
}
```

### prefix sum and difference

```cpp
// 1-D prefix sum
let S[i] = a[1] + a[2] + ... a[i]

then sum of elements from l to r is
a[l] + ... + a[r] = S[r] - S[l - 1]

// 2-D prefix sum
let S[i, j] = sum of all elements to the left and above of grid [i,j]

then sum of sub-matrix with (x1,y1) as left-up corner,
(x2,y2) as the right-down corner would be

S[x2, y2] - S[x1 - 1, y2] - S[x2, y1 - 1] + S[x1 - 1, y1 - 1]

// 1-D difference
B[i] = a[i] - a[i - 1]
add c to every number in [l,r]:
B[l] += c, B[r + 1] -= c

// 2-D difference
add c to every element of submatrix with (x1,y1) as left-up corner and (x2,y2) as
right-down corner
S[x1, y1] += c, S[x2 + 1, y1] -= c, S[x1, y2 + 1] -= c, S[x2 + 1, y2 + 1] += c

```

### bitwise ops

```cpp
n >> k & 1 //k-th digit in binary format of n
lowbit(n) = n & -n = n & (~n + 1)
n&((1<<k)-1)
n^(1<<k)
n|(1<<k)
n&(~(1<<k))

```

### two pointer technique

[https://mp.weixin.qq.com/s/4-4-9GoVqQ8sjguMzcZkcg](https://mp.weixin.qq.com/s/4-4-9GoVqQ8sjguMzcZkcg) sums up 双指针---6题14图一次搞懂

[https://www.geeksforgeeks.org/find-first-node-of-loop-in-a-linked-list/](https://www.geeksforgeeks.org/find-first-node-of-loop-in-a-linked-list/) two pointer finding first node of loop:\
1\. If a loop is found, initialize a slow pointer to head, leave fast pointer be at its position. \
2\. Move both slow and fast pointers one node at a time. \
3\. The point at which they meet is the start of the loop.



```cpp

//allows optimization from n^2 in brutal force to O(n) of linear time;
//2 pointers only move a total of 2*n times for example

	for (int i = 0, j = 0; i < n; i ++ )
	{
		while (j < i && check(i, j)) j ++ ;

		// logic of the specific problem
	}
	
common use cases:
(1) for a sequence, use 2 pointers to mark a segment.
(2) for 2 sequences, maintain an order, for example,
		when merging 2 sorted arrays in merge sort.
```

### discretization/dedup

```cpp
	//unique+erase example in c++ primer book
	vector<int> alls; // store values to be discretized
	sort(alls.begin(), alls.end()); // sort them
	alls.erase(unique(alls.begin(), alls.end()), alls.end());	//remove duplicates
		// 2-split to find the discretized corresponding value of x
	// can use lower_bound()=binary search
	//lower_bound(alls.begin(), alls.end(),x)-alls.begin();
	int find(int x)
	{
		int l = 0, r = alls.size() - 1;
		while (l < r)
		{
			int mid = l + r >> 1;
			if (alls[mid] >= x) r = mid;
			else l = mid + 1;
		}
		return r + 1;
	}
//above from yxc class

// discretization second way from lyd blue book
void discrete() {
	sort(a + 1, a + n + 1);
	for (int i = 1; i <= n; i++) //can also use STL unique()
		if (i == 1 || a[i] != a[i - 1])
			b[++m] = a[i];
}
// after discretization, query which int of 1-m is mapping of x
void query(int x) {
	return lower_bound(b + 1, b + m + 1, x) - b;
}
```

### segment merge&#x20;

```cpp
// greedy method to merge all intersecting segments

typedef pair<int,int> PII;
typedef pair<int,int> pii;

void merge_segs(vector<PII> &segs)
	{
		vector<PII> res;

		sort(segs.begin(), segs.end());

		int st = -2e9, ed = -2e9; //current seg in use
		for (auto seg : segs)
			if (ed < seg.first) //no intersection
			{
				if (st != -2e9) res.push_back({st, ed});
				//can't be the initial seg
				st = seg.first, ed = seg.second;
			}
			else ed = max(ed, seg.second); //union
		
		//need to insert the last maintained segment
		
		if (st != -2e9) res.push_back({st, ed});

		segs = res;
	}
```

### singly linked list

```cpp

single linked list such as adjacency list in graph and tree; 
aka static list

1D array as pointer; 
1D array simulation; 
more efficient than struct + ptr
new() in C++ is very slow!

// head is the last element inserted; 
// the list is stored in reverse order of insertion
// e[] stores node value
// ne[] stores node's next pointer which is the
// previous element inserted before this one
// idx represents current node index being used
int head, e[N], ne[N], idx;

// init
void init()
{
    head = -1;
    idx = 0;
}

// insert number a to end of list 
void insert(int a)
{
    e[idx] = a, ne[idx] = head, head = idx ++ ;
}

//insert x after position k
void insertk(int k, int x){
    e[idx]=x, ne[idx]=ne[k],ne[k]=idx++;

}
// delete head node, need to make sure head exists
void remove()
{
    head = ne[head];
}

//remove node after k-th position
void rmk(int k){
    ne[k]=ne[ne[k]];
}
```

### doubly linked list

```cpp
// e[] is node value
// l[] is left pointer of node ie left neighbor
// r[] is right pointer of node ie right neighbor
// idx is index currently being used

int e[N], l[N], r[N], idx;

// init
void init()
{
    //0 is left end; 1 is right end
    r[0] = 1, l[1] = 0;
    idx = 2;
}

// insert int x to right of node a
void insert(int a, int x)
{
    e[idx] = x;
    l[idx] = a, r[idx] = r[a];
    l[r[a]] = idx, r[a] = idx ++ ;
}

// delete node a
void remove(int a)
{
    l[r[a]] = l[a];
    r[l[a]] = r[a];
}
```

### stack

```cpp
// tt is stack top
int stk[N], tt = 0;

// insert x to stack top
stk[ ++ tt] = x;

// pop from stack top
tt -- ;

// value of stack top
stk[tt];

// check if stack is empty
if (tt > 0) {
}

```

### queue

```cpp
// hh is head of queue，tt is tail of queue
int q[N], hh = 0, tt = -1;

// insert x to queue tail
q[ ++ tt] = x;

// pop from head 
hh ++ ;

// value of head
q[hh];

// check if queue is empty
if (hh <= tt) {
}
```

### mono stack

```cpp
//Given N numbers, for every number find its left closest smaller neighbor 

//need a monotonic increasing stack;

//cpp
//参考y总
#include <iostream>
using namespace std;
const int N = 100010;
int stk[N], tt;

int main()
{
    int n;
    cin >> n;
    while (n -- )
    {
        int x;
        scanf("%d", &x);
        while (tt && stk[tt] >= x) tt -- ;//如果栈顶元素大于当前待入栈元素，则出栈
        if (!tt) printf("-1 ");//如果栈空，则没有比该元素小的值。
        else printf("%d ", stk[tt]);//栈顶元素就是左侧第一个比它小的元素。
        stk[ ++ tt] = x;
    }
    return 0;
}

作者：Hasity
链接：https://www.acwing.com/solution/content/27437/
来源：AcWing
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

//old template below

	int tt = 0;
	for (int i = 1; i <= n; i ++ )
	{
		while (tt && check(q[tt], i)) tt -- ;
		stk[ ++ tt] = i;
	}
```

### mono queue

```cpp
//used to find max/min in sliding window

	int hh = 0, tt = -1;
	for (int i = 0; i < n; i ++ )
	{
		while (hh <= tt && check_out(q[hh])) hh ++ ;  
		// check if head is out of window 
		while (hh <= tt && check(q[tt], i)) tt -- ;
		// only add to tail if meeting condition
		q[ ++ tt] = i; //add index to tail 
	}
```

### KMP

```cpp
//find pattern p[m] in string s[n]

	//calculate ne[] first：
	for (int i = 2, j = 0; i <= m; i ++ )
	{
		while (j && p[i] != p[j + 1]) j = ne[j];
		if (p[i] == p[j + 1]) j ++ ;
		ne[i] = j;
	}

	// match
	for (int i = 1, j = 0; i <= n; i ++ )
	{
		while (j && s[i] != p[j + 1]) j = ne[j];
		if (s[i] == p[j + 1]) j ++ ;
		if (j == m)
		{
			j = ne[j];
			// steps to do after match 
		}
	}
```

### Trie = prefix tree

Trie is a tree structure where each node represents a letter or a character in a word, and root to any node path is a prefix of one or more words.

In a trie, each node represents a letter or a character in a word, and the path from the root node to a leaf node represents a complete word. Each node typically has a boolean flag that indicates whether the node corresponds to the end of a word.

trie store string set efficiently

```cpp
	int son[N][26], cnt[N], idx;
    // node 0 is both root and empty
    // son[][] store the child node of every node
    // cnt[] store count of words ending with the node

	// insert a string 
	void insert(char *str)
	{
		int p = 0;
		for (int i = 0; str[i]; i ++ )
		{
			int u = str[i] - 'a';
			if (!son[p][u]) son[p][u] = ++ idx;
			p = son[p][u];
		}
		cnt[p] ++ ;
	}

	// query number of appearances of a string 
	int query(char *str)
	{
		int p = 0;
		for (int i = 0; str[i]; i ++ )
		{
			int u = str[i] - 'a';
			if (!son[p][u]) return 0;
			p = son[p][u];
		}
		return cnt[p];
	}
```

### Union find disjoint set

data structure used for grouping elements into disjoint sets, each represented by a tree. Initially, each element is in its own set, and the tree has only one node. The union-find data structure supports two main operations union (Given two elements a and b, the union operation connects the sets containing a and b) and find (Given an element a, the find operation returns the root node of the tree representing the set to which a belongs.)&#x20;

Union-find data structure can be implemented using two arrays, parent and rank. The parent array stores the parent node of each element, and the rank array stores the height of each tree (i.e., the length of the longest path from the root to a leaf node). The union and find operations can be implemented efficiently using path compression and union-by-rank(seldom used) heuristics.

Path compression is a technique that flattens the tree by making every node on the path from a given element to its root directly point to the root. This reduces the height of the tree and improves the efficiency of future find operations. Usually path compression is used most of the time.

Union-by-rank (seldom used) is a technique that ensures that the smaller tree is always merged into the larger tree during a union operation. This helps to keep the height of the tree small and ensures that the find operation is efficient. The Union-by-rank is seldomly used.

root node: if(p\[x]==x)

find(x): while(p\[x]!=x)x=p\[x]; //also allow checking two elements belong to same set

union find disjoint set is used in Kruskal's minimum spanning tree algorithm and image processing algorithms.

```cpp

	(1) naive union find：

		int p[N]; //store the ancestor node of each node 

		// find ancestor node for x 
		int find(int x)
		{
			if (p[x] != x) p[x] = find(p[x]); 
			//path compression; pointing directly to parent root
			return p[x];
		}

		//above recurion version of find same as:
		
		int find(int x){ //iterative verson of find()
    // find which set x belongs
    //after the while-loop, t is the root node
    int t = x; 
    while(p[t] != t) t = p[t];
    //path compression: point all son nodes to root node t 
    while(p[x] != x){
        p[x] = t;
        x = p[x];
    }
    return t;
		} //end of iterative find()

		// init, assuming node index 1 to n
		for (int i = 1; i <= n; i ++ ) p[i] = i;

		// union of sets a and b 
		p[find(a)] = find(b);


	(2) union find with size information 

		int p[N], size[N];
        // p[] stores ancestor node of each node
        // size[] only applies to ancestor node, meaning count of nodes in the set of the ancestor node

		// return ancestor node of x
		int find(int x)
		{
			if (p[x] != x) p[x] = find(p[x]); //path compression; pointing directly to parent root
			return p[x];
		}

		// init, assuming node index 1 to n
		for (int i = 1; i <= n; i ++ )
		{
			p[i] = i;
			size[i] = 1;
		}

		// union of sets a and b：b becomes parent of a
		//if (find(a) == find(b)) continue; // no action if a and b already belong to same set

        if (find(a) != find(b)) size[find(b)] += size[find(a)]; // add set a size to set b size

        p[find(a)] = find(b); //set parent root of a to be parent root of b
		//size[b] += size[a];


	(3) union find with distance information 

		int p[N], d[N];
        //p[] store ancestor node for each node
        //d[x] store distance from x to p[x] 

		// return ancestor node for x
		int find(int x)
		{
			if (p[x] != x)
			{
				int u = find(p[x]);
				d[x] += d[p[x]];
				p[x] = u;
			}
			return p[x];
		}

		// init, assuming node index 1 to n
		for (int i = 1; i <= n; i ++ )
		{
			p[i] = i;
			d[i] = 0;
		}

		// union of sets a and b 
		p[find(a)] = find(b);
		d[find(a)] = distance; // init find(a) offset according to the specifics of the problem

```

### Heap - using 1D array

A heap is a special binary tree that has heap order (parent is not less than children by default).

.There are two main types of heaps: max heaps and min heaps. In a max heap, the value of each node is greater than or equal to the values of its children, and the maximum value is stored at the root of the tree. In a min heap, the value of each node is less than or equal to the values of its children, and the minimum value is stored at the root of the tree.

Heaps are typically implemented as binary trees, where each node has at most two children.&#x20;

Heap is used to efficiently maintain a partially ordered set. Heaps are often used to implement priority queues, which are data structures that allow efficient access to the element with the highest priority

```cpp

//Self made min-heap and NOT STL heap!
supports actions 
a) insert a number; 
b) find minimum of set; 
c) delete minimum; 
d) delete k-th inserted element; 
e) modify k-th inserted element
* d) and e) can't be done by STL heap.

heap is a complete/full binary tree except the last level; 
last level is from left right;
we will build a min heap here = root is less than both left and right son node; 
so root of tree is minimum in min-heap

    STL heap is just priority queue; here heap is implemented using 1D array

    // heap index starts at 1 not 0!
    // h[N] store values of the heap, h[1] is heap top
    // h[x] left son is h[2x], right son is h[2x+1]
    // ph[k] stores the heap position of the the k-th inserted node
    // hp[k] stores the insert number of h[k], the value of its rank of insertion
// hp[k] is for the k-index point and keeps the order of when it's inserted
	//存储堆中下标是k的点是第几个插入的
	int h[N], ph[N], hp[N], size;

	// swap 2 nodes as well as their associated mappings 
	void heap_swap(int a, int b)
	{
		swap(ph[hp[a]],ph[hp[b]]);
		swap(hp[a], hp[b]);
		swap(h[a], h[b]);
	}

	void down(int u)
	{
	//check if u is the smallest of the 3 nodes; if not, down it
		int t = u; 
		if (u * 2 <= size && h[u * 2] < h[t]) t = u * 2; //left child
		if (u * 2 + 1 <= size && h[u * 2 + 1] < h[t]) t = u * 2 + 1; //right child
		if (u != t)
		{
			heap_swap(u, t);
			down(t);
		}
	}

	void up(int u)
	{
		while (u / 2 && h[u] < h[u / 2]) //compare u with parent
		{
			heap_swap(u, u / 2);
			u >>= 1;
		}
	}

	// O(n) heap building
	for (int i = n / 2; i; i -- ) down(i);

heap actions:- dijkstra will use

a) insert a number x: heap[++size]=x; up(size);
b) find minimum: heap[1];
c) delete minimum: heap[1]=heap[size];size--;down(1);
d) delete k-th inserted element: heap[k]=heap[size]; size--;down(k);up(k);
e) modify k-th inserted element:heap[k]=x;down(k);up(k);

```

### Hash using 1D array&#x20;

```cpp
	General hash
		(1) Open hashing 拉链法 chaining
			int h[N], e[N], ne[N], idx;

			// insert x into hash table
			void insert(int x)
			{
				int k = (x % N + N) % N;
				e[idx] = x;
				ne[idx] = h[k];
				h[k] = idx ++ ;
			}

			// find if x exist in hash 
			bool find(int x)
			{
				int k = (x % N + N) % N;
				for (int i = h[k]; i != -1; i = ne[i])
					if (e[i] == x)
						return true;

				return false;
			}

		(2) Open addressing/closed hashing 开放寻址法
			int h[N];

			// if x is in hash, return its subscript;
            // if x is not in the hash, return where x should be inserted
			int find(int x)
			{
				int t = (x % N + N) % N;
				while (h[t] != null && h[t] != x)
				{
					t ++ ;
					if (t == N) t = 0;
				}
				return t;
			}

	string hash

        key idea: treat the string as a P-base number; P usually 131 or 13331
        both are prime numbers which gives low collision rates.
        small trick: use 2^64 modulo and unsigned long long to store values,
        this way the overflow result is automatically the modulo op result.
    
		typedef unsigned long long ULL;
		ULL h[N], p[N]; 
        //h[k] stores hash value of first k characters of the string
        //p[k] store P^k mode 2^64

		// init
		p[0] = 1;
		for (int i = 1; i <= n; i ++ )
		{
			h[i] = h[i - 1] * P + str[i];
			p[i] = p[i - 1] * P;
		}

		// calculate substring[1 ~ r] hash
		ULL get(int l, int r)
		{
			return h[r] - h[l - 1] * p[r - l + 1];
		}
```

### unordered map = key-value pair, associative array

set (only keys) and map (key-value pairs) are both sorted in increasing order and values are unique. multiset and multimap allow duplicates (thus no direct indexing).

set is used to store only keys while map is used to store key value pairs.

bitset=bit sequences. A bitset is essentially an array of bits, where each element can be either 0 or 1. The `bitset` class provides various functions to manipulate and query individual bits, as well as to perform logical operations on the entire bitset.

### STL

```
	vector, variable length array, double-up allocation
		size()  count of elements
		empty()  check if empty
		clear()  clear up 
		front()/back() //return value of first and last element
		// front() == *begin(). 
		push_back()/pop_back()
		// back() == *(end()-1). 
		begin()/end() //return iterator of first and one-past-last element
		[]
		supports comparison in alphabetical order, binary-search of
        sorted vector lower_bound(v.begin(), v.end(), x) and 
        upper_bound(v.begin(), v.end(), x)

tuple<int,int,int>

	pair<int, int>
		first, 
		second,
		make_pair(a,b) or after C++11 p=(a,b);
		comparison based on first, then second alphabetically
		

	string，
		size()/length()  
		empty()
		clear()
		substr(start，size)  return substring
		c_str()  returns a const char* that points to a null-terminated string
		//ie c-style string, usually the address of 0-index first element of the string
		

	queue, FIFO
		size()
		empty()
		push()  //add element to queue end
		front()  //return queue front element
		back()  //return queue end element
		pop()  //remove queue front element
		no clear(), just declare a new queue to get an empty queue.

	priority_queue, default to max-heap
		push()  //insert one element
		top()  //return heap top element
		pop()  //remove heap top element
		min-heap add two more parameters：
		priority_queue<int, vector<int>, greater<int>> q;

	stack, LIFO
		size()
		empty()
		push()  //add one element to stack top
		top()  //return stack top element 
		pop()  //remove stack top element

	deque, double-end queue very slow
		size()
		empty()
		clear()
		front()/back()
		push_back()/pop_back()
		push_front()/pop_front()
		begin()/end()
		[]

	set, map, multiset, multimap, self-balancing binary search tree/red-black tree
	//dynamic maintainance ordered sequence
	
		size()
		empty()
		clear()
		begin()/end()
		++, -- //return predecessor and successor time complexity O(logn)
		

		set/multiset
			insert()  //insert one element 
			find()  //look up one element O(log n)
			count()  //return count of one element 
			erase()
				(1) input is number x, delete all x   O(k + logn)
				(2) input is iterator, delete the iterator
			lower_bound()/upper_bound()
				lower_bound(x) 
				//return smallest element >=x iterator successor,include x
				upper_bound(x)  
				//return smallest element >x iterator, not include x
				
				//page 25 of blue book
		map/multimap
			insert()  //insert a key-value pair
			erase()  //delete a key-value pair or iterator
			find()
			[]   //support direct element access such as map["CPU"]=25; 
			//O(log n)
			lower_bound()/upper_bound()

	unordered_set, unordered_map, unordered_multiset, unordered_multimap, //hash-table
		similar to set/map，but add/remove/change/find time is O(1)
		No support of lower_bound()/upper_bound()， 
		iterator ++，-- won't return predecessor or successor

	bitset, //bit packing
		bitset<10000> s;
		~, &, |, ^
		>>, <<
		==, !=
		[]

		count()  count of 1's in bitset

		any()  check if there's at least one 1
		none()  check is all bits is not 1(ie 0)

		set()  set all bits to 1
		set(k, v)  set k-index to v
		reset()  set all bits to 0
		flip()  flip all bits, same as ~
		flip(k) flip the k-index bit(k+1 th element from right)
```

### Tree and graph using 1D array

```cpp
//Tree is a special type of graph, same in terms of storage.
//for undirected graph, edge ab means 2 directed edge a->b and b->a
//so only directed graph needs to be considered.

	(1) adjacency matrix：g[a][b] representing edge a->b

	(2) adjacency list：

// for any node k, open a singly linked list to store all nodes reachable from k
//h[k] is the head and end element of the list
//list is stored in reverse order of being inserted

    int h[N], e[N], ne[N], idx;

		// add an edge a->b 
		void add(int a, int b)
		{
			e[idx] = b, ne[idx] = h[a], h[a] = idx ++ ;
		}
```

### tree/graph traversal dfs

```cpp

	// 深度优先遍历 dfs
		int dfs(int u)
		{
			st[u] = true; // st[u] 表示点u已经被遍历过

			for (int i = h[u]; i != -1; i = ne[i])
			{
				int j = e[i];
				if (!st[j]) dfs(j);
			}
		}
```

### tree/graph traversal bfs

```cpp
// 宽度优先遍历 bfs

		queue<int> q;
		st[1] = true; // 表示1号点已经被遍历过
		q.push(1);

		while (q.size())
		{
			int t = q.front();
			q.pop();

			for (int i = h[t]; i != -1; i = ne[i])
			{
				int j = e[i];
				if (!st[j]) //wrong, s[j] should be st[j]! yxc mistake
				{
					st[j] = true; // 表示点j已经被遍历过
					q.push(j);
				}
			}
		}
```

### Topological sort

```
	bool topsort()
	{
		int hh = 0, tt = -1;

		// in degree - d[i] 存储点i的入度
		for (int i = 1; i <= n; i ++ )
			if (!d[i])
				q[ ++ tt] = i;

		while (hh <= tt)
		{
			int t = q[hh ++ ];

			for (int i = h[t]; i != -1; i = ne[i])
			{
				int j = e[i];
				if (-- d[j] == 0)
					q[ ++ tt] = j;
			}
		}

		// 如果所有点都入队了，说明存在拓扑序列；否则不存在拓扑序列。
        // if all nodes are enqueued, there exists a topological sequence
        // otherwise no
		return tt == n - 1;
	}
```

### Dijkstra for shortest path

```cpp
1. Naive dijkstra 朴素dijkstra算法

	int g[N][N];  // edges 存储每条边
	int dist[N];  // 存储1号点到每个点的最短距离
	bool st[N];   // 存储每个点的最短路是否已经确定

	// 求1号点到n号点的最短路，如果不存在则返回-1
	int dijkstra()
	{
		memset(dist, 0x3f, sizeof dist);
		dist[1] = 0;

		for (int i = 0; i < n - 1; i ++ )
		{
			int t = -1;		// 在还未确定最短路的点中，寻找距离最小的点
			for (int j = 1; j <= n; j ++ )
				if (!st[j] && (t == -1 || dist[t] > dist[j]))
					t = j;

			// 用t更新其他点的距离
			for (int j = 1; j <= n; j ++ )
				dist[j] = min(dist[j], dist[t] + g[t][j]);

			st[t] = true;
		}

		if (dist[n] == 0x3f3f3f3f) return -1;
		return dist[n];
	}


2. 堆优化版dijkstra
	typedef pair<int, int> PII;

	int n;		// 点的数量
	int h[N], w[N], e[N], ne[N], idx;		// 邻接表存储所有边
	int dist[N];		// 存储所有点到1号点的距离
	bool st[N];		// 存储每个点的最短距离是否已确定

	// 求1号点到n号点的最短距离，如果不存在，则返回-1
	int dijkstra()
	{
		memset(dist, 0x3f, sizeof dist);
		dist[1] = 0;
		priority_queue<PII, vector<PII>, greater<PII>> heap;
		heap.push({0, 1});		// first存储距离，second存储节点编号

		while (heap.size())
		{
			auto t = heap.top();
			heap.pop();

			int ver = t.second, distance = t.first;

			if (st[ver]) continue;
			st[ver] = true;

			for (int i = h[ver]; i != -1; i = ne[i])
			{
				int j = e[i];
				if (dist[j] > distance + w[i])
				{
					dist[j] = distance + w[i];
					heap.push({dist[j], j});
				}
			}
		}

		if (dist[n] == 0x3f3f3f3f) return -1;
		return dist[n];
	}

```

### Bellman-Ford for shortest path

```cpp
	int n, m;		// n表示点数，m表示边数
	int dist[N];		// dist[x]存储1到x的最短路距离

	struct Edge		// 边，a表示出点，b表示入点，w表示边的权重
	{
		int a, b, w;
	}edges[M];

	// 求1到n的最短路距离，如果无法从1走到n，则返回-1。
	int bellman_ford()
	{
		memset(dist, 0x3f, sizeof dist);
		dist[1] = 0;

		// 如果第n次迭代仍然会松弛三角不等式，就说明存在一条长度是n+1的最短路径，由抽屉原理，
		// 路径中至少存在两个相同的点，说明图中存在负权回路。
		for (int i = 0; i < n; i ++ )
		{
			for (int j = 0; j < m; j ++ )
			{
				int a = edges[j].a, b = edges[j].b, w = edges[j].w;
				if (dist[b] > dist[a] + w)
					dist[b] = dist[a] + w;
			}
		}

		if (dist[n] == 0x3f3f3f3f) return -1;
		return dist[n];
	}
```

### SPFA - queue optimized Bellman-Ford 时间复杂度 平均情况下 O(m)，最坏情况下 O(nm), n 表示点数，m 表示边数

```cpp
	int n;		// 总点数
	int h[N], w[N], e[N], ne[N], idx;		// 邻接表存储所有边
	int dist[N];		// 存储每个点到1号点的最短距离
	bool st[N];		// 存储每个点是否在队列中

	// 求1号点到n号点的最短路距离，如果从1号点无法走到n号点则返回-1
	int spfa()
	{
		memset(dist, 0x3f, sizeof dist);
		dist[1] = 0;

		queue<int> q;
		q.push(1);
		st[1] = true;

		while (q.size())
		{
			auto t = q.front();
			q.pop();

			st[t] = false;

			for (int i = h[t]; i != -1; i = ne[i])
			{
				int j = e[i];
				if (dist[j] > dist[t] + w[i])
				{
					dist[j] = dist[t] + w[i];
					if (!st[j])		// 如果队列中已存在j，则不需要将j重复插入
					{
						q.push(j);
						st[j] = true;
					}
				}
			}
		}

		if (dist[n] == 0x3f3f3f3f) return -1;
		return dist[n];
	}

```

### spfa checking graph for negative loop

```cpp
//spfa判断图中是否存在负环 时间复杂度是 O(nm), n 表示点数，m 表示边数
	int n;		// 总点数
	int h[N], w[N], e[N], ne[N], idx;		// 邻接表存储所有边
	int dist[N], cnt[N];		// dist[x]存储1号点到x的最短距离，cnt[x]存储1到x的最短路中经过的点数
	bool st[N];		// 存储每个点是否在队列中

	// 如果存在负环，则返回true，否则返回false。
	bool spfa()
	{
		// 不需要初始化dist数组
		// 原理：如果某条最短路径上有n个点（除了自己），那么加上自己之后一共有n+1个点，
		// 由抽屉原理一定有两个点相同，所以存在环。

		queue<int> q;
		for (int i = 1; i <= n; i ++ )
		{
			q.push(i);
			st[i] = true;
		}

		while (q.size())
		{
			auto t = q.front();
			q.pop();

			st[t] = false;

			for (int i = h[t]; i != -1; i = ne[i])
			{
				int j = e[i];
				if (dist[j] > dist[t] + w[i])
				{
					dist[j] = dist[t] + w[i];
					cnt[j] = cnt[t] + 1;
					if (cnt[j] >= n) return true;		
					// 如果从1号点到x的最短路中包含至少n个点（不包括自己），则说明存在环
					if (!st[j])
					{
						q.push(j);
						st[j] = true;
					}
				}
			}
		}

		return false;
	}
```

### Floyd - using adjacency matrix

```
// need adjacency matrix 时间复杂度是 O(n^3), n 表示点数
// init 初始化：
		for (int i = 1; i <= n; i ++ )
			for (int j = 1; j <= n; j ++ )
				if (i == j) d[i][j] = 0;
				else d[i][j] = INF;

	// 算法结束后，d[a][b]表示a到b的最短距离
	void floyd()
	{
		for (int k = 1; k <= n; k ++ )
			for (int i = 1; i <= n; i ++ )
				for (int j = 1; j <= n; j ++ )
					d[i][j] = min(d[i][j], d[i][k] + d[k][j]);
	}
```

### prim for edge sum of MST using adjacency matrix&#x20;

```cpp
//need adjacency matrix
//时间复杂度是 O(n2+m)O(n2+m), nn 表示点数，mm 表示边数
int n;      // n表示点数
int g[N][N];        // 邻接矩阵，存储所有边
int dist[N];        // 存储其他点到当前最小生成树的距离
bool st[N];     // 存储每个点是否已经在生成树中


// 如果图不连通，则返回INF(值是0x3f3f3f3f), 否则返回最小生成树的树边权重之和
int prim()
{
    memset(dist, 0x3f, sizeof dist);

    int res = 0;
    for (int i = 0; i < n; i ++ )
    {
        int t = -1;
        for (int j = 1; j <= n; j ++ )
            if (!st[j] && (t == -1 || dist[t] > dist[j]))
                t = j;

        if (i && dist[t] == INF) return INF;

        if (i) res += dist[t];
        st[t] = true;

        for (int j = 1; j <= n; j ++ ) dist[j] = min(dist[j], g[t][j]);
    }

    return res;
}

```

### kruskal MST using 1D array

```
//union find needed

//时间复杂度是 O(mlogm)O(mlogm), nn 表示点数，mm 表示边数
int n, m;       // n是点数，m是边数
int p[N];       // 并查集的父节点数组

struct Edge     // 存储边
{
    int a, b, w;

    bool operator< (const Edge &W)const
    {
        return w < W.w;
    }
}edges[M];

int find(int x)     // 并查集核心操作
{
    if (p[x] != x) p[x] = find(p[x]);
    return p[x];
}

int kruskal()
{
    sort(edges, edges + m);

    for (int i = 1; i <= n; i ++ ) p[i] = i;    // 初始化并查集

    int res = 0, cnt = 0;
    for (int i = 0; i < m; i ++ )
    {
        int a = edges[i].a, b = edges[i].b, w = edges[i].w;

        a = find(a), b = find(b);
        if (a != b)     // 如果两个连通块不连通，则将这两个连通块合并
        {
            p[a] = b;
            res += w;
            cnt ++ ;
        }
    }

    if (cnt < n - 1) return INF;
    return res;
}


```

### 2 colorable test for bipartite graph

```
//2-colorable method for bipartite graph 染色法判别二分图
//时间复杂度是 O(n+m)O(n+m), nn 表示点数，mm 表示边数
int n;      // n表示点数
int h[N], e[M], ne[M], idx;     // 邻接表存储图
int color[N];       // 表示每个点的颜色，-1表示未染色，0表示白色，1表示黑色

// 参数：u表示当前节点，c表示当前点的颜色
bool dfs(int u, int c)
{
    color[u] = c;
    for (int i = h[u]; i != -1; i = ne[i])
    {
        int j = e[i];
        if (color[j] == -1)
        {
            if (!dfs(j, !c)) return false;
        }
        else if (color[j] == c) return false;
    }

    return true;
}

bool check()
{
    memset(color, -1, sizeof color);
    bool flag = true;
    for (int i = 1; i <= n; i ++ )
        if (color[i] == -1)
            if (!dfs(i, 0))
            {
                flag = false;
                break;
            }
    return flag;
}
```

### hungarian max matching for bipartite&#x20;

```
// Hungarian algorithm 匈牙利算法
//return max matching number

//	时间复杂度是 O(nm), n 表示点数，m 表示边数
int n1, n2;     // n1表示第一个集合中的点数，n2表示第二个集合中的点数
int h[N], e[M], ne[M], idx;     // 邻接表存储所有边，匈牙利算法中只会用到从第一个集合指向第二个集合的边，所以这里只用存一个方向的边
int match[N];       // 存储第二个集合中的每个点当前匹配的第一个集合中的点是哪个
bool st[N];     // 表示第二个集合中的每个点是否已经被遍历过

bool find(int x)
{
    for (int i = h[x]; i != -1; i = ne[i])
    {
        int j = e[i];
        if (!st[j])
        {
            st[j] = true;
            if (match[j] == 0 || find(match[j]))
            {
                match[j] = x;
                return true;
            }
        }
    }

    return false;
}

// 求最大匹配数，依次枚举第一个集合中的每个点能否匹配第二个集合中的点
int res = 0;
for (int i = 1; i <= n1; i ++ )
{
    memset(st, false, sizeof st);
    if (find(i)) res ++ ;
}

```

### math

<pre class="language-cpp"><code class="lang-cpp">
//trial division method for primes 试除法判定质数 —— 模板题 AcWing 866. 试除法判定质数
bool isprime(int x)
{
    if (x &#x3C; 2) return false;
    for (int i = 2; i &#x3C;= x / i; i ++ )
        if (x % i == 0)
            return false;
    return true;
}
//trial division method for factoring 试除法分解质因数 —— 模板题 AcWing 867. 分解质因数
void factori(int x)
{
    for (int i = 2; i &#x3C;= x / i; i ++ )
        if (x % i == 0)
        {
            int s = 0;
            while (x % i == 0) x /= i, s ++ ;
            cout &#x3C;&#x3C; i &#x3C;&#x3C; ' ' &#x3C;&#x3C; s &#x3C;&#x3C; endl;
        }
    if (x > 1) cout &#x3C;&#x3C; x &#x3C;&#x3C; ' ' &#x3C;&#x3C; 1 &#x3C;&#x3C; endl;
    cout &#x3C;&#x3C; endl;
}
//native sieve method for primes 朴素筛法求素数 O(nlogn) —— 模板题 AcWing 868. 筛质数
int primes[N], cnt;     // primes[]存储所有素数
bool st[N];         // st[x]存储x是否被筛掉

void get_primes(int n)
{
    for (int i = 2; i &#x3C;= n; i ++ )
    {
        if (st[i]) continue;
        primes[cnt ++ ] = i;
        for (int j = i + i; j &#x3C;= n; j += i)
            st[j] = true;
    }
}
//linear sieve method for primes 线性筛法求素数 —— 模板题 AcWing 868. 筛质数
int primes[N], cnt;     // primes[]存储所有素数
bool st[N];         // st[x]存储x是否被筛掉
//use the smallest prime factor to sieve only once;
//any composite number will be sived out by its smallest prime factor, only once
//thus this is linear 
void linearprimes(int n)
{
    for (int i = 2; i &#x3C;= n; i ++ )
    {
        if (!st[i]) primes[cnt ++ ] = i;
        for (int j = 0; primes[j] &#x3C;= n / i; j ++ )
        {
            st[primes[j] * i] = true;
            if (i % primes[j] == 0) break;
        }
    }
}
//trial division method for all factors 试除法求所有约数 —— 模板题 AcWing 869. 试除法求约数
vector&#x3C;int> get_divisors(int x)
{
    vector&#x3C;int> res;
    for (int i = 1; i &#x3C;= x / i; i ++ )
        if (x % i == 0)
        {
            res.push_back(i);
            if (i != x / i) res.push_back(x / i);
        }
    sort(res.begin(), res.end());
    return res;
}

//count of factors and sum of factors 约数个数和约数之和 —— 模板题 AcWing 870. 约数个数, 
//AcWing 871. 约数之和
//如果 N = p1^c1 * p2^c2 * ... *pk^ck
//约数个数： (c1 + 1) * (c2 + 1) * ... * (ck + 1)
//约数之和： (p1^0 + p1^1 + ... + p1^c1) * ... * (pk^0 + pk^1 + ... + pk^ck)

#include &#x3C;bits/stdc++.h>
using namespace std;
typedef long long LL; 
const int mod = 1e9 + 7;
int main(){
    int n,x;
    LL ans = 1;
    unordered_map&#x3C;int,int> hashi;
    cin >> n;
    while(n--){
        cin >> x;
        for(int i = 2;i &#x3C;= x/i; ++i){
            while(x % i == 0){
                x /= i;
                hashi[i] ++;
            }
        }
        if(x > 1) hashi[x] ++;
    }
    for(auto i : hashi) ans = ans*(i.second + 1) % mod;
    cout &#x3C;&#x3C; ans;
    return 0;
} //count of factors

///sum of factors
#include &#x3C;iostream>
#include &#x3C;algorithm>
#include &#x3C;unordered_map>
#include &#x3C;vector>

using namespace std;

typedef long long LL;

const int N = 110, mod = 1e9 + 7;

int main()
{
    int n;
    cin >> n;

    unordered_map&#x3C;int, int> primes;

    while (n -- )
    {
        int x;
        cin >> x;

        for (int i = 2; i &#x3C;= x / i; i ++ )
            while (x % i == 0)
            {
                x /= i;
                primes[i] ++ ;
            }

        if (x > 1) primes[x] ++ ;
    }

    LL res = 1;
    for (auto p : primes)
    {
        LL a = p.first, b = p.second;
        LL t = 1;
        while (b -- ) t = (t * a + 1) % mod;
        res = res * t % mod;
    }

    cout &#x3C;&#x3C; res &#x3C;&#x3C; endl;

    return 0;
} //sum of factors



//Euclidean method 欧几里得算法 —— 模板题 AcWing 872. 最大公约数
int gcd(int a, int b) //O(logn)
{
    return b ? gcd(b, a % b) : a;
}
//python euclidean:

>>> print inspect.getsource(gcd)
def gcd(a, b):
    """Calculate the Greatest Common Divisor of a and b.

    Unless b==0, the result will have the same sign as b (so that when
    b is divided by it, the result comes out positive).
    """
    while b:
        a, b = b, a%b
    return a
    
def gcd(x, y):
    while y != 0:
        (x, y) = (y, x % y)
    return x

def GCD(x , y):
    """This is used to calculate the GCD of the given two numbers.
    You remember the farm land problem where we need to find the 
    largest , equal size , square plots of a given plot?"""
    if y == 0:
        return x
    r = int(x % y)
    return GCD(y , r)

//Note: if your numbers are REALLY REALLY large then try to 
//increase the recursion limit by: (this could fill up RAM)

import sys
sys.seterecursionlimit("your new limit")



//Euler phi function 求欧拉函数 —— 模板题 AcWing 873. 欧拉函数
int phi(int x)
{
    int res = x;
    for (int i = 2; i &#x3C;= x / i; i ++ )
        if (x % i == 0)
        {
            res = res / i * (i - 1);
            while (x % i == 0) x /= i;
        }
    if (x > 1) res = res / x * (x - 1);

    return res;
}
//sieve method for Euler phi 
// based on linear prime sieving method
//筛法求欧拉函数 —— 模板题 AcWing 874. 筛法求欧拉函数
int primes[N], cnt;     // primes[]存储所有素数
int euler[N];           // 存储每个数的欧拉函数
bool st[N];         // st[x]存储x是否被筛掉


<strong>void get_eulers(int n)
</strong>{
    euler[1] = 1;
    for (int i = 2; i &#x3C;= n; i ++ )
    {
        if (!st[i])
        {
            primes[cnt ++ ] = i;
            euler[i] = i - 1;
        }
        for (int j = 0; primes[j] &#x3C;= n / i; j ++ )
        {
            int t = primes[j] * i;
            st[t] = true;
            if (i % primes[j] == 0)
            {
                euler[t] = euler[i] * primes[j];
                break;
            }
            euler[t] = euler[i] * (primes[j] - 1);
        }
    }
}
//binary exponentiation 快速幂 —— 模板题 AcWing 875. 快速幂
//求 m^k mod p，时间复杂度 O(logk)。

int bmi(int m, int k, int p)
{
    int res = 1 % p, t = m;
    while (k)
    {
        if (k&#x26;1) res = res * t % p;
        t = t * t % p; //square the base; one digit higher
        k >>= 1;
    }
    return res;
}
//extended Euclidean method 扩展欧几里得算法 —— 模板题 AcWing 877. 扩展欧几里得算法
// 求x, y，使得ax + by = gcd(a, b)
int exgcd(int a, int b, int &#x26;x, int &#x26;y)
{
    if (!b)
    {
        x = 1; y = 0;
        return a;
    }
    int d = exgcd(b, a % b, y, x);
    y -= (a/b) * x;
    return d;
}
//Gauss elimination 高斯消元 —— 模板题 AcWing 883. 高斯消元解线性方程组
// a[N][N]是增广矩阵
int gauss()
{
    int c, r;
    for (c = 0, r = 0; c &#x3C; n; c ++ )
    {
        int t = r;
        for (int i = r; i &#x3C; n; i ++ )   // 找到绝对值最大的行
            if (fabs(a[i][c]) > fabs(a[t][c]))
                t = i;

        if (fabs(a[t][c]) &#x3C; eps) continue;

        for (int i = c; i &#x3C;= n; i ++ ) swap(a[t][i], a[r][i]);      // 将绝对值最大的行换到最顶端
        for (int i = n; i >= c; i -- ) a[r][i] /= a[r][c];      // 将当前上的首位变成1
        for (int i = r + 1; i &#x3C; n; i ++ )       // 用当前行将下面所有的列消成0
            if (fabs(a[i][c]) > eps)
                for (int j = n; j >= c; j -- )
                    a[i][j] -= a[r][j] * a[i][c];

        r ++ ;
    }

    if (r &#x3C; n)
    {
        for (int i = r; i &#x3C; n; i ++ )
            if (fabs(a[i][n]) > eps)
                return 2; // 无解
        return 1; // 有无穷多组解
    }

    for (int i = n - 1; i >= 0; i -- )
        for (int j = i + 1; j &#x3C; n; j ++ )
            a[i][n] -= a[i][j] * a[j][n];   //move back up to solve h

    return 0; // 有唯一解
}

//1. combination number via recursion 递归法求组合数 —— 模板题 AcWing 885. 求组合数 I
// c[a][b]="a choose b"
for (int i = 0; i &#x3C; N; i ++ )
    for (int j = 0; j &#x3C;= i; j ++ )
        if (!j) c[i][j] = 1;
        else c[i][j] = (c[i - 1][j] + c[i - 1][j - 1]) % mod;

//2. modulo inverse and Fermat's little law for combo number
//通过预处理逆元的方式求组合数 —— 模板题 AcWing 886. 求组合数 II
//首先预处理出所有阶乘取模的余数fact[N]，以及所有阶乘取模的逆元infact[N]
//如果取模的数是质数，可以用费马小定理求逆元
int qmi(int a, int k, int p)    // 快速幂模板
{
    int res = 1;
    while (k)
    {
        if (k &#x26; 1) res = (LL)res * a % p;
        a = (LL)a * a % p;
        k >>= 1;
    }
    return res;
}

// 预处理阶乘的余数和阶乘逆元的余数
fact[0] = infact[0] = 1;
for (int i = 1; i &#x3C; N; i ++ )
{
    fact[i] = (LL)fact[i - 1] * i % mod;
    infact[i] = (LL)infact[i - 1] * qmi(i, mod - 2, mod) % mod;
}

//3. Lucas定理 —— 模板题 AcWing 887. 求组合数 III
//若p是质数，则对于任意整数 1 &#x3C;= m &#x3C;= n，有：
//    C(n, m) = C(n % p, m % p) * C(n / p, m / p) (mod p)

int qmi(int a, int k)       // 快速幂模板
{
    int res = 1;
    while (k)
    {
        if (k &#x26; 1) res = (LL)res * a % p;
        a = (LL)a * a % p;
        k >>= 1;
    }
    return res;
}


int C(int a, int b)     // 通过定理求组合数C(a, b)
{
    int res = 1;
    for (int i = 1, j = a; i &#x3C;= b; i ++, j -- )
    {
        res = (LL)res * j % p;
        res = (LL)res * qmi(i, p - 2) % p;
    }
    return res;
}


//lucas method does NOT use Catalan at all!
int lucas(LL a, LL b)
{
    if (a &#x3C; p &#x26;&#x26; b &#x3C; p) return C(a, b);
    return (LL)C(a % p, b % p) * lucas(a / p, b / p) % p;
}

//4. combo number via factoring into prime numbers;
//can get the exact result, no modulo used; no qmi!
//分解质因数法求组合数 —— 模板题 AcWing 888. 求组合数 IV
//当我们需要求出组合数的真实值，而非对某个数的余数时，分解质因数的方式比较好用：
//    1. 筛法求出范围内的所有质数
//    2. 通过 C(a, b) = a! / b! / (a - b)! 这个公式求出每个质因子的次数。 
// n! 中p的次数是 n / p + n / p^2 + n / p^3 + ...
//    3. 用高精度乘法将所有质因子相乘

int primes[N], cnt;     // 存储所有质数
int sum[N];     // 存储每个质数的次数
bool st[N];     // 存储每个数是否已被筛掉


void get_primes(int n)      // 线性筛法求素数
{
    for (int i = 2; i &#x3C;= n; i ++ )
    {
        if (!st[i]) primes[cnt ++ ] = i;
        for (int j = 0; primes[j] &#x3C;= n / i; j ++ )
        {
            st[primes[j] * i] = true;
            if (i % primes[j] == 0) break;
        }
    }
}


int get(int n, int p)       // 求n！中的次数
{
    int res = 0;
    while (n)
    {
        res += n / p;
        n /= p;
    }
    return res;
}


vector&#x3C;int> mul(vector&#x3C;int> a, int b)       // 高精度乘低精度模板
{
    vector&#x3C;int> c;
    int t = 0;
    for (int i = 0; i &#x3C; a.size(); i ++ )
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

    return c;
}

get_primes(a);  // 预处理范围内的所有质数

for (int i = 0; i &#x3C; cnt; i ++ )     // 求每个质因数的次数
{
    int p = primes[i];
    sum[i] = get(a, p) - get(b, p) - get(a - b, p);
}

vector&#x3C;int> res;
res.push_back(1);

for (int i = 0; i &#x3C; cnt; i ++ )     // 用高精度乘法将所有质因子相乘
    for (int j = 0; j &#x3C; sum[i]; j ++ )
        res = mul(res, primes[i]);

//Catalan numbers
卡特兰数 —— 模板题 AcWing 889. 满足条件的01序列
给定n个0和n个1，它们按照某种顺序排成长度为2n的序列，
满足任意前缀中0的个数都不少于1的个数的序列的数量为： 
Cat(n) = C(2n, n) / (n + 1)

#include &#x3C;iostream>
using namespace std
typedef long long LL;
const int mod = 1e9 + 7;

int qmi(int a, int k, int p) {
    int res = 1 % p;
    while (k) {
        if (k &#x26; 1) res = (LL)res * a % p;
        a = (LL)a * a % p;
        k >>= 1;
    }
    return res;
}

int main() {

    int n;
    cin >> n;

    int a = 2 * n, b = n;
    int res = 1;

    for (int i = a; i > a - b; i--) res = (LL)res * i % mod;//C top
    for (int i = 1; i &#x3C;= b; i++) res = (LL)res * qmi(i, mod - 2, mod) % mod;
    //C bottom, divide change to inverse element

    res = (LL)res * qmi(n + 1, mod - 2, mod) % mod; //divide (n+1)

    cout &#x3C;&#x3C; res &#x3C;&#x3C; endl;

    return 0;
}


//NIM game - basic game theory
NIM游戏 —— 模板题 AcWing 891. Nim游戏
给定N堆物品，第i堆物品有Ai个。两名玩家轮流行动，每次可以任选一堆，取走任意多个物品，可把一堆取光，但不能不取。取走最后一件物品者获胜。两人都采取最优策略，问先手是否必胜。

我们把这种游戏称为NIM博弈。把游戏过程中面临的状态称为局面。整局游戏第一个行动的称为先手，第二个行动的称为后手。若在某一局面下无论采取何种行动，都会输掉游戏，则称该局面必败。
所谓采取最优策略是指，若在某一局面下存在某种行动，使得行动后对面面临必败局面，则优先采取该行动。同时，这样的局面被称为必胜。我们讨论的博弈问题一般都只考虑理想情况，即两人均无失误，都采取最优策略行动时游戏的结果。
NIM博弈不存在平局，只有先手必胜和先手必败两种情况。

定理： NIM博弈先手必胜，当且仅当 A1 ^ A2 ^ … ^ An != 0

公平组合游戏ICG impartial combinatorial games
若一个游戏满足：

由两名玩家交替行动；
在游戏进程的任意时刻，可以执行的合法行动与轮到哪名玩家无关；
不能行动的玩家判负；
则称该游戏为一个公平组合游戏。
NIM博弈属于公平组合游戏，但城建的棋类游戏，比如围棋，就不是公平组合游戏。因为围棋交战双方分别只能落黑子和白子，胜负判定也比较复杂，不满足条件2和条件3。

有向图游戏
给定一个有向无环图，图中有一个唯一的起点，在起点上放有一枚棋子。两名玩家交替地把这枚棋子沿有向边进行移动，每次可以移动一步，无法移动者判负。该游戏被称为有向图游戏。
任何一个公平组合游戏都可以转化为有向图游戏。具体方法是，把每个局面看成图中的一个节点，并且从每个局面向沿着合法行动能够到达的下一个局面连有向边。

Mex运算
设S表示一个非负整数集合。定义mex(S)为求出不属于集合S的最小非负整数的运算，即：
mex(S) = min{x}, x属于自然数，且x不属于S

SG函数
在有向图游戏中，对于每个节点x，设从x出发共有k条有向边，分别到达节点y1, y2, …, yk，定义SG(x)为x的后继节点y1, y2, …, yk 的SG函数值构成的集合再执行mex(S)运算的结果，即：
SG(x) = mex({SG(y1), SG(y2), …, SG(yk)})
特别地，整个有向图游戏G的SG函数值被定义为有向图游戏起点s的SG函数值，即SG(G) = SG(s)。

有向图游戏的和 —— 模板题 AcWing 893. 集合-Nim游戏
设G1, G2, …, Gm 是m个有向图游戏。定义有向图游戏G，它的行动规则是任选某个有向图游戏Gi，并在Gi上行动一步。G被称为有向图游戏G1, G2, …, Gm的和。
有向图游戏的和的SG函数值等于它包含的各个子游戏SG函数值的异或和，即：
SG(G) = SG(G1) ^ SG(G2) ^ … ^ SG(Gm)

定理
有向图游戏的某个局面必胜，当且仅当该局面对应节点的SG函数值大于0。
有向图游戏的某个局面必败，当且仅当该局面对应节点的SG函数值等于0


</code></pre>

### probably not needed - arbitrary precision, 2 split, merge sort, quick sort&#x20;

```
// arbitrary precision addition 高精度加法
// C = A + B, A >= 0, B >= 0
vector<int> add(vector<int> &A, vector<int> &B)
{
    if (A.size() < B.size()) return add(B, A);

    vector<int> C;
    int t = 0;
    for (int i = 0; i < A.size(); i ++ )
    {
        t += A[i];
        if (i < B.size()) t += B[i];
        C.push_back(t % 10);
        t /= 10;
    }

    if (t) C.push_back(t);
    return C;
}

// 高精度减法
// C = A - B, 满足A >= B, A >= 0, B >= 0
vector<int> sub(vector<int> &A, vector<int> &B)
{
    vector<int> C;
    for (int i = 0, t = 0; i < A.size(); i ++ )
    {
        t = A[i] - t;
        if (i < B.size()) t -= B[i];
        C.push_back((t + 10) % 10);
        if (t < 0) t = 1;
        else t = 0;
    }

    while (C.size() > 1 && C.back() == 0) C.pop_back();
    return C;
}

// 高精度乘低精度
// C = A * b, A >= 0, b > 0
vector<int> mul(vector<int> &A, int b)
{
    vector<int> C;
    int t = 0;
    for (int i = 0; i < A.size() || t; i ++ )
    {
        if (i < A.size()) t += A[i] * b;
        C.push_back(t % 10);
        t /= 10;
    }

    return C;
}

// 高精度除以低精度
// A / b = C ... r, A >= 0, b > 0
vector<int> div(vector<int> &A, int b, int &r)
{
    vector<int> C;
    r = 0;
    for (int i = A.size() - 1; i >= 0; i -- )

	{
        r = r * 10 + A[i];
        C.push_back(r / b);
        r %= b;
    }
    reverse(C.begin(), C.end());
    while (C.size() > 1 && C.back() == 0) C.pop_back();
    return C;
}


// quicksort template with double pointer starting on each end
// pivot using one number x=q[left]

void quicksort(int q[], int l, int r)
{
    if (l >= r) return;
    // sect into two parts
    int i = l - 1, j = r + 1, x = q[l];
    while (i < j)
    {
        do i ++ ; while (q[i] < x);
        do j -- ; while (q[j] > x);
        if (i < j) swap(q[i], q[j]); // or int t=q[i];q[i]=q[j];q[j]=t;
        else break;
    }
    quicksort(q, l, j), quicksort(q, j + 1, r); // use j not i-1, i where x=q[r]
}

// merge_sort() template
// get/use mid-point index first; sort left and right; then merge
// also has double pointer

void mergesort(int q[], int l, int r)
{
    if (l >= r) return;

    int mid = l + r >> 1;
    mergesort(q, l, mid);
    mergesort(q, mid + 1, r);

    // below is merge two sorted parts into one
    // two way merge 

	int k = 0, i = l, j = mid + 1;
    while (i <= mid && j <= r)
        if (q[i] < q[j]) tmp[k ++ ] = q[i ++ ];
        else tmp[k ++ ] = q[j ++ ];

    while (i <= mid) tmp[k ++ ] = q[i ++ ];
    while (j <= r) tmp[k ++ ] = q[j ++ ];

    for (i = l, j = 0; i <= r; i ++, j ++ ) q[i] = tmp[j];
}



bool check(int x) {/* ... */} 
// check if x satisfy a condition 

// for use when dividing segment [l,r] to 
// [l,mid] + [mid+1,r]
int bsearch_1(int l, int r)
{
    while (l < r)
    {
        int mid = l + r >> 1;
        if (check(mid)) r = mid;    
        // check() to see if mid satisfy condition
        else l = mid + 1;			// TLE for leetcode LIS
    }
    return l;
}
//for use when dividing [l,r] to [l,mid-1]+[mid,r]
int bsearch_2(int l, int r)
{
    while (l < r)
    {
        int mid = l + r + 1 >> 1;
        if (check(mid)) l = mid;
        else r = mid - 1;
    }
    return l;
}

// floating point 2-split template

bool check(double x) {/* ... */} 
// check is x meet a condition

double bsearch_3(double l, double r)
{
    const double eps = 1e-6;   
    //eps is precision, depends on requirements of the problem
    
    
    while (r - l > eps)
    {
        double mid = (l + r) / 2;
        if (check(mid)) r = mid;
        else l = mid;
    }
    return l;
}

```

basic knowledge points are building foundations of programming, must understand first

no repeating no missing

don't forget brackets

don't confuse i and j - very hard to debug or notice!



```
#include <bits/stdc++.h>
using namespace std;
#define ll long long
#define fir(i,a,b) for(ll i=a;i<=b;i++)
const int N=le5+10;

ll n,m,t,a[N],b[N],s[N],x,y;
ll calc(ll a[],ll n){
}
int main(){   
}
ll calc(ll a[], ll n){
fir(i,1,n){
    a[i]-=(a[0]/n);
    s[i]=s[i-1]+a[i];
}
sort(s+1,s+n+1);
ll mid=(n+1)>>1,ans=0;
fir(i,1,n) ans+=abs(s[mid]-s[i]);
return ans;
}

int main(){
    cin>>n>>m>>t;
    fir(i,1,t){scanf("%d%d",&x,&y); a[x]++;b[y]++;}
    fir(i,1,n) a[0]+=a[i];
    fir(i,1,m) b[0]+=b[i];

    ll as=a[0]%n,bs=b[0]%m;
    if(!as && !bs) cout<<"both "<<calc(a,n)+calc(b,m);
    else if(!as) cout<<"row "<<calc(a,n);
    else if(!bs)cout<<"column "<<calc(b,m);
    else cout<<"impossible";

    return 0;
}
```

```
#include <bits/stdc++.h>
using namespace std;
 
#define ll long long
#define ld long double
#define ar array
 
#include <ext/pb_ds/assoc_container.hpp>
#include <ext/pb_ds/tree_policy.hpp>
using namespace __gnu_pbds;
 
template <typename T> using oset = tree<T, null_type, less<T>, rb_tree_tag, tree_order_statistics_node_update>;
 
#define vt vector
#define pb push_back
#define all(c) (c).begin(), (c).end()
#define sz(x) (int)(x).size()
 
#define F_OR(i, a, b, s) for (int i=(a); (s)>0?i<(b):i>(b); i+=(s))
#define F_OR1(e) F_OR(i, 0, e, 1)
#define F_OR2(i, e) F_OR(i, 0, e, 1)
#define F_OR3(i, b, e) F_OR(i, b, e, 1)
#define F_OR4(i, b, e, s) F_OR(i, b, e, s)
#define GET5(a, b, c, d, e, ...) e
#define F_ORC(...) GET5(__VA_ARGS__, F_OR4, F_OR3, F_OR2, F_OR1)
#define FOR(...) F_ORC(__VA_ARGS__)(__VA_ARGS__)
#define EACH(x, a) for (auto& x: a)
 
template<class T> bool umin(T& a, const T& b) {
        return b<a?a=b, 1:0;
}
template<class T> bool umax(T& a, const T& b) { 
        return a<b?a=b, 1:0;
} 
 
ll FIRSTTRUE(function<bool(ll)> f, ll lb, ll rb) {
        while(lb<rb) {
                ll mb=(lb+rb)/2;
                f(mb)?rb=mb:lb=mb+1; 
        } 
        return lb;
}
ll LASTTRUE(function<bool(ll)> f, ll lb, ll rb) {
        while(lb<rb) {
                ll mb=(lb+rb+1)/2;
                f(mb)?lb=mb:rb=mb-1; 
        } 
        return lb;
}
 
template<class A> void read(vt<A>& v);
template<class A, size_t S> void read(ar<A, S>& a);
template<class T> void read(T& x) {
        cin >> x;
}
void read(double& d) {
        string t;
        read(t);
        d=stod(t);
}
void read(long double& d) {
        string t;
        read(t);
        d=stold(t);
}
template<class H, class... T> void read(H& h, T&... t) {
        read(h);
        read(t...);
}
template<class A> void read(vt<A>& x) {
        EACH(a, x)
                read(a);
}
template<class A, size_t S> void read(array<A, S>& x) {
        EACH(a, x)
                read(a);
}
 
string to_string(char c) {
        return string(1, c);
}
string to_string(bool b) {
        return b?"true":"false";
}
string to_string(const char* s) {
        return string(s);
}
string to_string(string s) {
        return s;
}
string to_string(vt<bool> v) {
        string res;
        FOR(sz(v))
                res+=char('0'+v[i]);
        return res;
}
 
template<size_t S> string to_string(bitset<S> b) {
        string res;
        FOR(S)
                res+=char('0'+b[i]);
        return res;
}
template<class T> string to_string(T v) {
    bool f=1;
    string res;
    EACH(x, v) {
                if(!f)
                        res+=' ';
                f=0;
                res+=to_string(x);
        }
    return res;
}
 
template<class A> void write(A x) {
        cout << to_string(x);
}
template<class H, class... T> void write(const H& h, const T&... t) { 
        write(h);
        write(t...);
}
void print() {
        write("\n");
}
template<class H, class... T> void print(const H& h, const T&... t) { 
        write(h);
        if(sizeof...(t))
                write(' ');
        print(t...);
}
 
void DBG() {
        cerr << "]" << endl;
}
template<class H, class... T> void DBG(H h, T... t) {
        cerr << to_string(h);
        if(sizeof...(t))
                cerr << ", ";
        DBG(t...);
}
#ifdef _DEBUG
#define dbg(...) cerr << "LINE(" << __LINE__ << ") -> [" << #__VA_ARGS__ << "]: [", DBG(__VA_ARGS__)
#else
#define dbg(...) 0
#endif
 
template<class T> void offset(ll o, T& x) {
        x+=o;
}
template<class T> void offset(ll o, vt<T>& x) {
        EACH(a, x)
                offset(o, a);
}
template<class T, size_t S> void offset(ll o, ar<T, S>& x) {
        EACH(a, x)
                offset(o, a);
}
 
mt19937 mt_rng(chrono::steady_clock::now().time_since_epoch().count());
ll randint(ll a, ll b) {
        return uniform_int_distribution<ll>(a, b)(mt_rng);
}
 
template<class T, class U> void vti(vt<T> &v, U x, size_t n) {
        v=vt<T>(n, x);
}
template<class T, class U> void vti(vt<T> &v, U x, size_t n, size_t m...) {
        v=vt<T>(n);
        EACH(a, v)
                vti(a, x, m);
}
 
const int d4i[4]={-1, 0, 1, 0}, d4j[4]={0, 1, 0, -1};
const int d8i[8]={-1, -1, 0, 1, 1, 1, 0, -1}, d8j[8]={0, 1, 1, 1, 0, -1, -1, -1};
 
void solve() {
        int n, k;
        read(n, k);
        vt<int> x(n), y(n);
        read(x, y);
        sort(all(x));
        vt<int> d1(n+1), d2(n+1);
        for(int i=0, j=0; i<n; ++i) {
                while(x[j]<x[i]-k)
                        ++j;
                d1[i+1]=i-j+1;
        }
        for(int i=n-1, j=n-1; ~i; --i) {
                while(x[j]>x[i]+k)
                        --j;
                d2[i]=j-i+1;
        }
        int ans=0;
        FOR(n)
                umax(d1[i+1], d1[i]);
        FOR(n+1)
                umax(ans, d1[i]+d2[i]);
        print(ans);
}
 
int main() {
        ios::sync_with_stdio(0);
        cin.tie(0);
 
        int t=1;
        read(t);
        FOR(t) {
                //write("Case #", i+1, ": ");
                solve();
        }
}
```
