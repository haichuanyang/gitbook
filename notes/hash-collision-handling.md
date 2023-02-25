# hash collision handling

C++与数据结构 BIT Univ&#x20;

hash collision handling - 4 ways:

1.  open addressing H\[i] = (H(key) + d\[i]) mod m, where i = 1, 2, ...,k (k<=m-1), H(key) is hashing function; m is hash table length; d\[i] is increment sequence; d\[i] has 3 ways to implement:

    1. d\[i]=1,2,3,...m-1; linear detection discretization;
    2. 2\. d\[i]=1^2, -1^2,2^2,-2^2,3^2,-3^2....+/-k^2 (k<=m/2); quadratic detection discretization;
    3. d\[i]=pseudo random sequence; pseudo random detection discretization


2. &#x20;rehash method: H\[i]=RH\[i]\(key), i=1,2,3,...k; R and H are different hash functions; no clustering, but added computing time.
3. Chaining hash
4. setting up a common overflow area, and sending every collision value to the overflow area







```
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
```
