# USACO 2020 January Contest, SilverProblem 2. Loan Repayment

[http://www.usaco.org/current/data/sol\_loan\_silver\_jan20.html](http://www.usaco.org/current/data/sol\_loan\_silver\_jan20.html) solution official





```
(Analysis by Benjamin Qi)
Binary search on X. For the first subtask, we can check whether the number of gallons of milk that FJ gives is at least N in O(K) time. However, this does not suffice for full points.

How can we do better than Θ(K)? As the numbers in the statement are up to 1012, not 1018, this suggests that some sort of N‾‾√ factor is involved.

Suppose that we fix X. Then Y decreases over time. It turns out that if we process all transitions that leave Y unchanged in O(1) time, then our solution runs quickly enough! If there are more than 2N‾‾‾√ distinct values of Y then FJ definitely gives Bessie enough mlik because

1+2+…+⌈2N‾‾‾√⌉≥N.
Thus, we can check whether X works in O(N‾‾√) time.
It follows that our solution runs in O(N‾‾√logN) time.

Nick Wu's code:

#include <stdio.h>
 
int valid(long long n, long long k, long long m, long long x) {
  long long g = 0;
  while(k > 0 && g < n) {
    long long y = (n - g) / x;
    if(y < m) {
      long long leftover = (n-g + m-1) / m;
      return leftover <= k;
    }
    long long maxmatch = n - x*y;
    long long numdays = (maxmatch - g) / y + 1;
    if(numdays > k) numdays = k;
    g += y * numdays;
    k -= numdays;
  }
  return g >= n;
}
 
int main() {
  freopen("loan.in", "r", stdin);
  freopen("loan.out", "w", stdout);
  long long n, k, m;
  scanf("%lld %lld %lld", &n, &k, &m);
  long long lhs = 1;
  long long rhs = 1e12;
  while(lhs < rhs) {
    long long mid = (lhs + rhs + 1) / 2;
    if(valid(n, k, m, mid)) {
      lhs = mid;
    }
    else {
      rhs = mid - 1;
    }
  }
  printf("%lld\n", lhs);
}
```

```
解题方法：
这道题目的方法是二分答案+数学。

对于30 3030%的数据
我们只要二分答案，然后直接模拟k kk天就行了。
时间复杂度为O ( k log ⁡ n 2 ) O(k\log_n^2)O(klog 
n
2
​	
 )。

对于100 100100%的数据
我们要二分答案x xx，对上面的办法进行优化。
假设当天要给的牛奶数是y yy，一共有连续a aa天要给一样的数量，然而还有g gg个牛奶没给，还剩下t tt天要给牛奶。
那么，我们可以知道第a aa天要给y yy个  ( 1 ) \:(1)(1)，第a + 1 a+1a+1天给的数量一定比y yy要小  ( 2 ) \:(2)(2)。
则
⌊ g − ( a − 1 ) y x ⌋ = y   ( 1 ) \lfloor \frac{g-(a-1)y}{x} \rfloor=y\:(1)⌊ 
x
g−(a−1)y
​	
 ⌋=y(1)
⌊ g − a y x ⌋ < y   ( 2 ) \lfloor \frac{g-ay}{x} \rfloor<y\:(2)⌊ 
x
g−ay
​	
 ⌋<y(2)

简化1 11式：
把向下取整去掉，得
g − ( a − 1 ) y x ≥ y \frac{g-(a-1)y}{x}\geq y 
x
g−(a−1)y
​	
 ≥y
两边同时乘以x xx，得
g − ( a − 1 ) y ≥ x y {g-(a-1)y}\geq xyg−(a−1)y≥xy
两边同时除以y yy，得
g y − ( a − 1 ) ≥ x \frac{g}{y}-(a-1)\geq x 
y
g
​	
 −(a−1)≥x
拆括号，得
g y − a + 1 ≥ x \frac{g}{y}-a+1\geq x 
y
g
​	
 −a+1≥x
整理出关于a aa的式子，得
g y − x + 1 ≥ a \frac{g}{y}-x+1\geq a 
y
g
​	
 −x+1≥a

简化2 22式：
把向下取整去掉，得
g − a y x < y \frac{g-ay}{x}<y 
x
g−ay
​	
 <y
两边同时乘以x xx，得
g − a y < x y g-ay<xyg−ay<xy
两边同时除以y yy，得
g y − a < x \frac{g}{y}-a<x 
y
g
​	
 −a<x
整理出关于a aa的式子，得
g y − x < a \frac{g}{y}-x<a 
y
g
​	
 −x<a

整理1 11和2 22式：
将两个式子合并，得
g y − x < a ≤ g y − x + 1 \frac{g}{y}-x<a\leq \frac{g}{y}-x+1 
y
g
​	
 −x<a≤ 
y
g
​	
 −x+1
因为a aa是整数，所以
a = ⌊ g y − x + 1 ⌋ a=\lfloor \frac{g}{y}-x+1 \rfloora=⌊ 
y
g
​	
 −x+1⌋

所以我们只要进行分块计算，也就是一次计算a aa次，这样就会提高效率。
但是有时候a aa会大于你剩下的天数t tt，就会越界，所以
a = min ⁡ ( ⌊ g y − x + 1 ⌋ , t ) a=\min(\lfloor \frac{g}{y}-x+1 \rfloor,t)a=min(⌊ 
y
g
​	
 −x+1⌋,t)
我们只需要这样做：将g gg减去a aa乘以y yy，得到剩下要给的牛奶数，然后再将t tt减去a aa，得到剩下的天数。
注：上面的运算仅限于y > m y>my>m。
如果y < = m y<=my<=m，我们应该直接把剩下的所有都拿走，也就是将g gg减去y yy乘以t tt，然后t = 0 t=0t=0。
这是判断x xx是否合法的程序：

int ck(long long x)
{
	long long g=n,t=k,y,a;
	while(g>0&&t>0)
	{
		y=g/x;
		if(y<=m) g=g-t*m,s=0;
		else a=min(g/y-x+1,t),g=g-a*y,t-=a;
	}
	if(g<=0) return 1;
	return 0;
}
1
2
3
4
5
6
7
8
9
10
11
12
这道题就这样被解决了，时间复杂度大概为O ( n log ⁡ n 2 ) O(\sqrt{n}\log_{n}^{2})O( 
n
​	
 log 
n
2
​	
 )，可以过n ≤ 1 0 12 n\leq10^{12}n≤10 
12
 的数据。
 
 
 //o为x
while(s>0){
	long long y=(n-g)/o;
	if(y<=m){
		g+=s*m;
		s=0;
		break;
	}
	else{
		long long a=(n-g)/y-o+1;
		if(a<=s){
			g+=a*y;
			s-=a;
		}
		else{
			g+=s*y;
			s=0;
		}
	}
}
return g;


```

[https://blog.csdn.net/zhy\_Learn/article/details/105477164](https://blog.csdn.net/zhy\_Learn/article/details/105477164) 1solution

[https://blog.csdn.net/linweitong2020/article/details/105459864](https://blog.csdn.net/linweitong2020/article/details/105459864) 2 solution
