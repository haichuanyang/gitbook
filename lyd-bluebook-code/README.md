# C++ bluebook code



```cpp
// 快速幂，求a^b mod p
int power(int a, int b, int p) {
	int ans = 1;
	for (; b; b >>= 1) {
		if (b & 1) ans = (long long)ans * a % p;
		a = (long long)a * a % p;
	}
	return ans;
}

// 64位整数乘法的O(log b)算法
long long mul(long long a, long long b, long long p) {
	long long ans = 0;
	for (; b; b >>= 1) {
		if (b & 1) ans = (ans + a) % p;
		a = a * 2 % p;
	}
	return ans;
}

// 64位整数乘法的long double算法
typedef unsigned long long ull;
ull mul(ull a, ull b, ull p) {
	a %= p, b %= p;  // 当a,b一定在0~p之间时，此行不必要
	ull c = (long double)a * b / p;
	ull x = a * b, y = c * p;
	long long ans = (long long)(x % p) - (long long)(y % p);
	if (ans < 0) ans += p;
	return ans;
}

// hamilton路径
int f[1 << 20][20];
int hamilton(int n, int weight[20][20]) {
	memset(f, 0x3f, sizeof(f));
	f[1][0] = 0;
	for (int i = 1; i < 1 << n; i++)
		for (int j = 0; j < n; j++)
			if (i >> j & 1)
				for (int k = 0; k < n; k++)
					if (i >> k & 1)
						f[i][j] = min(f[i][j], f[i ^ 1 << j][k] + weight[k][j]);
	return f[(1 << n) - 1][n - 1];
}

// lowbit运算，找到二进制下所有是1的位
int H[37];
// 预处理
for (int i = 0; i < 36; i++) H[(1ll << i) % 37] = i;
// 对多次询问进行求解
while (cin >> n) {
	while (n > 0) {
		cout << H[(n & -n) % 37] << ' ';
		n -= n & -n;
	}
	cout << endl;
}
// 递归实现指数型枚举
vector<int> chosen;
void calc(int x) {
	if (x == n + 1) {
		for (int i = 0; i < chosen.size(); i++)
			printf("%d ", chosen[i]);
		puts("");
		return;
	}
	calc(x + 1);
	chosen.push_back(x);
	calc(x + 1);
	chosen.pop_back();
}

// 递归实现组合型枚举
vector<int> chosen; 
void calc(int x) {
	if (chosen.size() > m || chosen.size() + (n - x + 1) < m) return;
	if (x == n + 1) {
		for (int i = 0; i < chosen.size(); i++)
			printf("%d ", chosen[i]);
		puts("");
		return;
	}
	calc(x + 1);
	chosen.push_back(x);
	calc(x + 1);
	chosen.pop_back();
}

// 递归实现排列型枚举
int order[20];
bool chosen[20];
void calc(int k) {
	if (k == n + 1) {
		for (int i = 1; i <= n; i++)
			printf("%d ", order[i]);
		puts("");
		return;
	}
	for (int i = 1; i <= n; i++) {
		if (chosen[i]) continue;
		order[k] = i;
		chosen[i] = 1;
		calc(k + 1);
		chosen[i] = 0;
		order[k] = 0;
	}
}


// 模拟机器实现，把组合型枚举改为非递归
vector<int> chosen;
int stack[100010], top = 0, address = 0;

void call(int x, int ret_addr) { // 模拟计算机汇编指令call
	int old_top = top;
	stack[++top] = x; // 参数x
	stack[++top] = ret_addr; // 返回地址标号
	stack[++top] = old_top; // 在栈顶记录以前的top值
}

int ret() { // 模拟计算机汇编指令ret
	int ret_addr = stack[top - 1];
	top = stack[top]; // 恢复以前的top值
	return ret_addr;
}

int main() {
	int n, m;
	cin >> n >> m;
	call(1, 0); // calc(1)
	while (top) {
		int x = stack[top - 2]; // 获取参数
		switch (address) {
		case 0:
			if (chosen.size() > m || chosen.size() + (n - x + 1) < m) {
				address = ret(); // return
				continue;
			}
			if (x == n + 1) {
				for (int i = 0; i < chosen.size(); i++)
					printf("%d ", chosen[i]);
				puts("");
				address = ret(); // return
				continue;
			}
			call(x + 1, 1); // 相当于calc(x + 1)，返回后会从case 1继续执行
			address = 0;
			continue; // 回到while循环开头，相当于开始新的递归
		case 1:
			chosen.push_back(x);
			call(x + 1, 2); // 相当于calc(x + 1)，返回后会从case 2继续执行
			address = 0;
			continue; // 回到while循环开头，相当于开始新的递归
		case 2:
			chosen.pop_back();
			address = ret(); // 相当于原calc函数结尾，执行return
		}
	}
}
// 在单调递增序列a中查找>=x的数中最小的一个（即x或x的后继）
while (l < r) {
	int mid = (l + r) / 2;
	if (a[mid] >= x) r = mid; else l = mid + 1;
}

// 在单调递增序列a中查找<=x的数中最大的一个（即x或x的前驱）
while (l < r) {
	int mid = (l + r + 1) / 2;
	if (a[mid] <= x) l = mid; else r = mid - 1;
}

// 实数域二分，设置eps法
while (l + eps < r) {
	double mid = (l + r) / 2;
	if (calc(mid)) r = mid; else l = mid; 
}

// 实数域二分，规定循环次数法
for (int i = 0; i < 100; i++) {
	double mid = (l + r) / 2;
	if (calc(mid)) r = mid; else l = mid;	
}

// 把n本书分成m组，每组厚度之和<=size，是否可行
bool valid(int size) {
	int group = 1, rest = size;
	for (int i = 1; i <= n; i++) {
		if (rest >= a[i]) rest -= a[i];
		else group++, rest = size - a[i];
	}
	return group <= m;
}

// 二分答案，判定“每组厚度之和不超过二分的值”时能否在m组内把书分完
int l = 0, r = sum_of_Ai;
while (l < r) {
	int mid = (l + r) / 2;
	if (valid(mid)) r = mid; else l = mid + 1;
}
cout << l << endl;
// 离散化
void discrete() {
	sort(a + 1, a + n + 1);
	for (int i = 1; i <= n; i++) // 也可用STL中的unique函数
		if (i == 1 || a[i] != a[i - 1])
			b[++m] = a[i];
}

// 离散化后，查询x映射为哪个1~m之间的整数
void query(int x) {
	return lower_bound(b + 1, b + m + 1, x) - b;
}


// 归并排序求逆序对
void merge(int l, int mid, int r) {
	// 合并a[l~mid]与a[mid+1~r]
	// a是待排序数组, b是临时数组, cnt是逆序对个数
	int i = l, j = mid + 1;
	for (int k = l; k <= r; k++)
		if (j > r || i <= mid && a[i] < a[j]) b[k] = a[i++];
		else b[k] = a[j++], cnt += mid - i + 1;
	for (int k = l; k <= r; k++) a[k] = b[k];
}
// 区间最值问题的ST算法
void ST_prework() {
	for (int i = 1; i <= n; i++) f[i][0] = a[i];
	int t = log(n) / log(2) + 1;
	for (int j = 1; j < t; j++)
		for (int i = 1; i <= n - (1<<j) + 1; i++)
			f[i][j] = max(f[i][j-1], f[i + (1<<(j-1))][j-1]);
}

int ST_query(int l, int r) {
	int k = log(r - l + 1) / log(2);
	return max(f[l][k], f[r - (1<<k) + 1][k]);
}
// 递归法求中缀表达式的值，O(n^2)
int calc(int l, int r) {
	// 寻找未被任何括号包含的最后一个加减号
	for (int i = r, j = 0; i >= l; i--) {
		if (s[i] == '(') j++;
		if (s[i] == ')') j--;
		if (j == 0 && s[i] == '+') return calc(l, i - 1) + calc(i + 1, r);
		if (j == 0 && s[i] == '-') return calc(l, i - 1) - calc(i + 1, r);
	}
	// 寻找未被任何括号包含的最后一个乘除号
	for (int i = r, j = 0; i >= l; i--) {
		if (s[i] == '(') j++;
		if (s[i] == ')') j--;
		if (j == 0 && s[i] == '*') return calc(l, i - 1) * calc(i + 1, r);
		if (j == 0 && s[i] == '/') return calc(l, i - 1) / calc(i + 1, r);
	}
	// 首尾是括号
	if (s[l] == '('&&s[r] == ')') return calc(l + 1, r - 1);
	// 是一个数
	int ans = 0;
	for (int i = l; i <= r; i++) ans = ans * 10 + s[i] - '0';
	return ans;
}

// ----------------------------------------------------
// 后缀表达式转中缀表达式，同时求值，O(n)

// 数值栈 
vector<int> nums; 
// 运算符栈 
vector<char> ops;

// 优先级 
int grade(char op) {
	switch (op) {
	case '(':
		return 1;
	case '+':
	case '-':
		return 2;
	case '*':
	case '/':
		return 3;
	}
	return 0;
}

// 处理后缀表达式中的一个运算符 
void calc(char op) {
	// 从栈顶取出两个数 
	int y = *nums.rbegin();
	nums.pop_back();
	int x = *nums.rbegin();
	nums.pop_back();
	int z;
	switch (op) {
	case '+':
		z = x + y;
		break;
	case '-':
		z = x - y;
		break;
	case '*':
		z = x * y;
		break;
	case '/':
		z = x / y;
		break;
	}
	// 把运算结果放回栈中 
	nums.push_back(z);	
}

// 中缀表达式转后缀表达式，同时对后缀表达式求值 
int solve(string s) {
	nums.clear();
	ops.clear();
	int top = 0, val = 0;
	for (int i = 0; i < s.size(); i++) {
		// 中缀表达式的一个数字 
		if (s[i] >= '0' && s[i] <= '9') {
			val = val * 10 + s[i] - '0';
			if (s[i+1] >= '0' && s[i+1] <= '9') continue;
			// 后缀表达式的一个数，直接入栈 
			nums.push_back(val);
			val = 0;
		}
		// 中缀表达式的左括号 
		else if (s[i] == '(') ops.push_back(s[i]);
		// 中缀表达式的右括号 
		else if (s[i] == ')') {
			while (*ops.rbegin() != '(') {
				// 处理后缀表达式的一个运算符 
				calc(*ops.rbegin());
				ops.pop_back();
			}
			ops.pop_back();
		}
		// 中缀表达式的加减乘除号 
		else {
			while (ops.size() && grade(*ops.rbegin()) >= grade(s[i])) {
				calc(*ops.rbegin());
				ops.pop_back();
			}
			ops.push_back(s[i]);
		} 
	}
	while (ops.size()) {
		calc(*ops.rbegin());
		ops.pop_back();
	}
	// 后缀表达式栈中最后剩下的数就是答案 
	return *nums.begin();
}

// ----------------------------------------------------
// 单调栈
a[n + 1] = p = 0;
for (int i = 1; i <= n + 1; i++) {
	if (a[i] > s[p]) {
		s[++p] = a[i], w[p] = 1;
	} else {
		int width=0;
		while (s[p] > a[i]) {
			width += w[p];
			ans = max(ans, (long long)width * s[p]);
			p--;
		}
		s[++p] = a[i], w[p] = width + 1;
	}
}
// 单调队列
int l = 1, r = 1;
q[1] = 0; // save choice j=0
for(int i = 1; i <= n; i++)
{
	while (l <= r && q[l] < i - m) l++;
	ans = max(ans, sum[i] - sum[q[l]]);
	while (l <= r && sum[q[r]] >= sum[i]) r--;
	q[++r] = i;
}
// 双向链表
struct Node {
	int value; // data
	Node *prev, *next; // pointers
};
Node *head, *tail;

void initialize() { // create an empty list
	head = new Node();
	tail = new Node();
	head->next = tail;
	tail->prev = head;
}

void insert(Node *p, int value) { // insert data after p
	q = new Node();
	q->value = value;
	p->next->prev = q; q->next = p->next;
	p->next = q; q->prev = p;
}

void remove(Node *p) { // remove p
	p->prev->next = p->next;
	p->next->prev = p->prev;
	delete p;
}

void recycle() { // release memory
	while (head != tail) {
		head = head->next;
		delete head->prev;
	}
	delete tail;
}

// 数组模拟链表
struct Node {
	int value;
	int prev, next;
} node[SIZE];
int head, tail, tot;

int initialize() {
	tot = 2;
	head = 1, tail = 2;
	node[head].next = tail;
	node[tail].prev = head;
}

int insert(int p, int value) {
	q = ++tot;
	node[q].value = value;
	node[node[p].next].prev = q;
	node[q].next = node[p].next;
	node[p].next = q; node[q].prev = p;
}

void remove(int p) {
	node[node[p].prev].next = node[p].next;
	node[node[p].next].prev = node[p].prev;
}


// 邻接表：加入有向边(x, y)，权值为z
void add(int x, int y, int z) {
	ver[++tot] = y, edge[tot] = z; // 真实数据
	next[tot] = head[x], head[x] = tot; // 在表头x处插入
}

// 邻接表：访问从x出发的所有边
for (int i = head[x]; i; i = next[i]) {
	int y = ver[i], z = edge[i];
	// 一条有向边(x, y)，权值为z
}﻿// Hash 例题：兔子与兔子
char s[1000010];
unsigned long long f[1000010], p[1000010];
int n, q;
int main() {
	scanf("%s", s + 1);
	n = strlen(s + 1);
	cin >> q;
	p[0] = 1; // 131^0
	for (int i = 1; i <= n; i++) {
		f[i] = f[i-1] * 131 + (s[i]-'a'+1); // hash of 1~i
		p[i] = p[i-1] * 131; // 131^i
	}
	for (int i = 1; i <= q; i++) {
		int l1, r1, l2, r2;
		scanf("%d%d%d%d", &l1, &r1, &l2, &r2);
		if (f[r1] - f[l1-1] * p[r1-l1+1] == // hash of l1~r1
			f[r2] - f[l2-1] * p[r2-l2+1]) { // hash of l2~r2
			puts("Yes");
		} else {
			puts("No");
		}
	}
}
﻿// KMP
next[1] = 0;
for (int i = 2, j = 0; i <= n; i++) {
	while (j > 0 && a[i] != a[j+1]) j = next[j];
	if (a[i] == a[j+1]) j++;
	next[i] = j;
}

for (int i = 1, j = 0; i <= m; i++) {
	while (j > 0 && (j == n || b[i] != a[j+1])) j = next[j];
	if (b[i] == a[j+1]) j++;
	f[i] = j;
	// if (f[i] == n)，此时就是A在B中的某一次出现
}


// 最小表示法
int n = strlen(s + 1);
for (int i = 1; i <= n; i++) s[n+i] = s[i];
int i = 1, j = 2, k;
while (i <= n && j <= n) {
	for (k = 0; k < n && s[i+k] == s[j+k]; k++);
	if (k == n) break; // s likes "aaaaa"
	if (s[i+k] > s[j+k]) {
		i = i + k + 1;
		if (i == j) i++;
	} else {
		j = j + k + 1;
		if (i == j) j++;
	}
}
ans = min(i, j);
// 假设字符串由小写字母构成
int trie[SIZE][26], tot = 1;

// Trie的插入
void insert(char* str) {
	int len = strlen(str), p = 1;
	for (int k = 0; k < len; k++) {
		int ch = str[k]-'a';
		if (trie[p][ch] == 0) trie[p][ch] = ++tot;
		p = trie[p][ch];
	}
	end[p] = true;
}

// Trie的检索
bool search(char* str) {
	int len = strlen(str), p = 1;
	for (int k = 0; k < len; k++) {
		p = trie[p][str[k]-'a'];
		if (p == 0) return false;
	}
	return end[p];
}
// 二叉堆
int heap[SIZE], n;
void up(int p) {
	while (p > 1) {
		if (heap[p] > heap[p/2]) {
			swap(heap[p], heap[p/2]);
			p/=2;
		}
		else break;
	}	
}
void down(int p) {
	int s = p*2;
	while (s <= n) {
		if (s < n && heap[s] < heap[s+1]) s++;
		if (heap[s] > heap[p]) {
			swap(heap[s], heap[p]);
			p = s, s = p*2;
		}
		else break;
	}
}
void Insert(int val) {
	heap[++n] = val;
	up(n);
}
int GetTop() {
	return heap[1];
}
void Extract() {
	heap[1] = heap[n--];
	down(1);
}
void Remove(int k) {
	heap[k] = heap[n--];
	up(k), down(k);
}// 深度优先遍历框架
void dfs(int x) {
	v[x] = 1;
	for (int i = head[x]; i; i = next[i]) {
		int y = ver[i];
		if (v[y]) continue;
		dfs(y);
	}
}

// DFS序
void dfs(int x) {
	a[++m] = x;
	v[x] = 1;
	for (int i = head[x]; i; i = next[i]) {
		int y = ver[i];
		if (v[y]) continue;
		dfs(y);
	}
	a[++m] = x;
}

// 求树中各点的深度
void dfs(int x) {
	v[x] = 1;
	for (int i = head[x]; i; i = next[i]) {
		int y = ver[i];
		if (v[y]) continue; // 点y已经被访问过了
		d[y] = d[x] + 1;
		dfs(y);
	}
}

// 求树的重心
void dfs(int x) {
	v[x] = 1; size[x] = 1; // 子树x的大小
	int max_part = 0; // 删掉x后分成的最大子树的大小
	for (int i = head[x]; i; i = next[i]) {
		int y = ver[i];
		if (v[y]) continue; // 点y已经被访问过了
		dfs(y);
		size[x] += size[y];
		max_part = max(max_part, size[y]);
	}
	max_part = max(max_part, n - size[x]);
	if (max_part < ans) {
		ans = max_part;
		pos = x;
	}
}

// 划分图的连通块
void dfs(int x) {
	v[x] = cnt;
	for (int i = head[x]; i; i = next[i]) {
		int y = ver[i];
		if (v[y]) continue;
		dfs(y);
	}
}
for (int i = 1; i <= n; i++)
	if (!v[i]) {
		cnt++;
		dfs(i);
	}

// 广度优先遍历框架
void bfs() {
	memset(d, 0, sizeof(d));
	queue<int> q;
	q.push(1); d[1] = 1;
	while (q.size()) {
		int x = q.front(); q.pop();
		for (int i = head[x]; i; i = next[i]) {
			int y = ver[i];
			if (d[y]) continue;
			d[y] = d[x] + 1;
			q.push(y);
		}
	}
}

// 拓扑排序
void add(int x, int y) { // 在邻接表中添加一条有向边
	ver[++tot] = y, next[tot] = head[x], head[x] = tot;
	deg[y]++;
}
void topsort() {
	queue<int> q;
	for (int i = 1; i <= n; i++)
		if (deg[i] == 0) q.push(i);
	while (q.size()) {
		int x = q.front(); q.pop();
		a[++cnt] = x;
		for (int i = head[x]; i; i = next[i]) {
			int y = ver[i];
			if (--deg[y] == 0) q.push(y);
		}
	}
}
int main() {
	cin >> n >> m; // 点数、边数
	for (int i = 1; i <= m; i++) {
		int x, y;
		scanf("%d%d", &x, &y);
		add(x, y);
	}
	topsort();
	for (int i = 1; i <= cnt; i++)
		printf("%d ", a[i]);
	cout << endl;
}﻿// 试除法判断n是否为质数
bool is_prime(int n) {
    if (n < 2) return false;
    for (int i = 2; i <= sqrt(n); i++)
        if (n % i == 0) return false;
    return true;
}

// Eratosthenes筛法
void primes(int n) {
    memset(v, 0, sizeof(v)); // 合数标记
    for (int i = 2; i <= n; i++) {
        if (v[i]) continue;
        cout << i << endl; // i是质数
        for (int j = i; j <= n/i; j++) v[i*j] = 1;
    }
}

// 线性筛法
int v[MAX_N], prime[MAX_N];
void primes(int n) {
	memset(v, 0, sizeof(v)); // 最小质因子
	m = 0; // 质数数量
	for (int i = 2; i <= n; i++) {
		if (v[i] == 0) { // i是质数
			v[i] = i;
			prime[++m] = i;
		}
		// 给当前的数i乘上一个质因子
		for (int j = 1; j <= m; j++) {
			// i有比prime[j]更小的质因子，或者超出n的范围
			if (prime[j] > v[i] || prime[j] > n/i) break;
			// prime[j]是合数i*prime[j]的最小质因子
			v[i*prime[j]] = prime[j];
		}
	}
	for (int i = 1; i <= m; i++)
		cout << prime[i] << endl;
}

// 试除法分解质因数
void divide(int n) {
    m = 0;
    for (int i = 2; i*i <= n; i++) {
        if (n % i == 0) { // i是质数
            p[++m] = i, c[m] = 0;
            while (n % i == 0) n /= i, c[m]++; // 除掉所有的i
        }
    }
    if (n > 1) // n是质数
        p[++m] = n, c[m] = 1;
    for (int i = 1; i <= m; i++)
        cout << p[i] << '^' << c[i] <<endl;
}// 试除法求N的约数集合
int factor[1600], m = 0;
for (int i = 1; i*i <= n; i++) {
    if (n % i == 0) {
        factor[++m] = i;
        if (i != n/i) factor[++m] = n/i;
    }
}
for (int i = 1; i <= m; i++)
    cout << factor[i] <<endl;

// 倍数法求1~N每个数的约数集合
vector<int> factor[500010];
for (int i = 1; i <= n; i++)
    for (int j = 1; j <= n/i; j++)
        factor[i*j].push_back(i);
for (int i = 1; i <= n; i++) {
    for (int j = 0; j < factor[i].size(); j++)
        printf("%d ", factor[i][j]);
    puts("");
}


// 欧几里得算法求最大公约数
int gcd(int a, int b) {
    return b ? gcd(b, a % b) : a;
}

// 计算欧拉函数
int phi(int n) {
    int ans = n;
    for (int i = 2; i*i <= n; i++) {
        if (n % i == 0) { // i是质数
            ans = ans / i * (i-1);
            while (n % i == 0) n /= i;
        }
    }
    if (n > 1) // n是质数
        ans = ans / n * (n-1);
    return ans;
}

// 欧拉函数快速递推：Eratosthenes筛法
void euler(int n) {
	for (int i = 2; i <= n; i++) phi[i] = i;
	for (int i = 2; i <= n; i++)
		if (phi[i] == i)
			for (int j = i; j <= n; j += i)
				phi[j] = phi[j] / i * (i - 1);
}

// 欧拉函数快速递推：线形筛法
int v[MAX_N], prime[MAX_N], phi[MAX_N];
void euler(int n) {
	memset(v, 0, sizeof(v)); // 最小质因子
	m = 0; // 质数数量
	for (int i = 2; i <= n; i++) {
		if (v[i] == 0) { // i是质数
			v[i] = i, prime[++m] = i;
			phi[i] = i - 1;
		}
		// 给当前的数i乘上一个质因子
		for (int j = 1; j <= m; j++) {
			// i有比prime[j]更小的质因子，或者超出n的范围
			if (prime[j] > v[i] || prime[j] > n / i) break;
			// prime[j]是合数i*prime[j]的最小质因子
			v[i*prime[j]] = prime[j];
			phi[i*prime[j]] = phi[i] * (i%prime[j] ? prime[j]-1 : prime[j]);
		}
	}
	for (int i = 2; i <= n; i++)
		cout << i << ' ' << phi[i] << endl;
}
// 扩展欧几里得算法
int exgcd(int a, int b, int &x, int &y) {
    if (b == 0) { x = 1, y = 0; return a; }
    int d = exgcd(b, a%b, x, y);
    int z = x; x = y; y = z - y * (a / b);
    return d;
}


// Baby Step, Giant Step
// 同余方程 a^x mod p = b，求x的最小非负整数解，无解返回-1
int baby_step_giant_step(int a, int b, int p) {
	map<int, int> hash;
	hash.clear();
    b %= p;
    int t = (int)sqrt(p) + 1;
    for (int j = 0; j < t; j++) {
    	int val = (long long)b * power(a, j, p) % p; // b*a^j
		hash[val] = j;
	}
    a = power(a, t, p); // a^t
    if (a == 0) return b == 0 ? 1 : -1;
    for (int i = 0; i <= t; i++) {
    	int val = power(a, i, p); // (a^t)^i
        int j = hash.find(val) == hash.end() ? -1 : hash[val];
        if (j >= 0 && i * t - j >= 0) return i * t - j;
    }
    return -1;
}
// Mobius函数
for (int i = 1; i <= n; i++) miu[i] = 1, v[i] = 0;
for (int i = 2; i <= n; i++) {
	if (v[i]) continue;
	miu[i] = -1;
	for (int j = 2 * i; j <= n; j += i) {
		v[j] = 1;
		if ((j / i) % i == 0) miu[j] = 0;
		else miu[j] *= -1;
	}
}
// 并查集
int fa[SIZE];

for (int i = 0; i <= n; i++) fa[i] = i;

int get(int x) {
	if (x == fa[x]) return x;
	return fa[x] = get(fa[x]);
}

void merge(int x, int y) {
	fa[get(x)] = get(y);
}

// 边带权的并查集
int get(int x) {
	if (x == fa[x]) return x;
	int root = get(fa[x]);  // 递归计算集合代表
	d[x] += d[fa[x]];       // 维护d数组——对边权求和
	return fa[x] = root;    // 路径压缩 
}

void merge(int x, int y) {
	x = get(x), y = get(y);
	fa[x] = y, d[x] = size[y];
	size[y] += size[x];
}
// [1,x]分成的O(log(x))个小区间
while (x > 0) {
	printf("[%d, %d]\n", x - (x & -x) + 1, x);
	x -= x & -x;
}

// 树状数组查询前缀和
int ask(int x) {
	int ans = 0;
	for (; x; x -= x & -x) ans += c[x];
	return ans;
}

// 树状数组单点增加
void add(int x, int y) {
	for (; x <= N; x += x & -x) c[x] += y;
}

// 树状数组求逆序对
for (int i = n; i; i--) {
	ans += ask(a[i]);
	add(a[i], 1);
}
struct SegmentTree {
	int l, r;
	int dat;
} t[SIZE * 4]; // struct数组存储线段树

void build(int p, int l, int r) {
	t[p].l = l, t[p].r = r; // 节点p代表区间[l,r]
	if (l == r) { t[p].dat = a[l]; return; } // 叶节点
	int mid = (l + r) / 2; // 折半
	build(p*2, l, mid); // 左子节点[l,mid]，编号p*2
	build(p*2+1, mid+1, r); // 右子节点[mid+1,r]，编号p*2+1
	t[p].dat = max(t[p*2].dat, t[p*2+1].dat); // 从下往上传递信息
}

build(1, 1, n); // 调用入口

void change(int p, int x, int v) {
	if (t[p].l == t[p].r) { t[p].dat = v; return; } // 找到叶节点
	int mid = (t[p].l + t[p].r) / 2;
	if (x <= mid) change(p*2, x, v); // x属于左半区间
	else change(p*2+1, x, v); // x属于右半区间
	t[p].dat = max(t[p*2].dat, t[p*2+1].dat); // 从下往上更新信息
}

change(1, x, v); // 调用入口

int ask(int p, int l, int r) {
	if (l <= t[p].l && r >= t[p].r) return t[p].dat; // 完全包含，直接返回
	int mid = (t[p].l + t[p].r) / 2;
	int val = 0;
	if (l <= mid) val = max(val, ask(p*2, l, r)); // 左子节点有重叠
	if (r > mid) val = max(val, ask(p*2+1, l, r)); // 右子节点有重叠
	return val;
}

cout << ask(1, l, r) << endl; // 调用入口

// 动态开点的线段树
struct SegmentTree {
    int lc, rc; // 左右子节点的编号
	int dat;
} tr[SIZE * 2];
int root, tot;

int build() { // 新建一个节点
	tot++;
	tr[tot].lc = tr[tot].rc = tr[tot].dat = 0;
	return tot;
}

// 在main函数中
tot = 0;
root = build(); // 根节点

// 单点修改，在val位置加delta，维护区间最大值
void insert(int p, int l, int r, int val, int delta) {
    if (l == r) {
        tr[p].dat += delta;
        return;
    }
    int mid = (l + r) >> 1; // 代表的区间[l,r]作为递归参数传递
    if (val <= mid) {
        if (!tr[p].lc) tr[p].lc = build(); // 左子树不存在，动态开点
        insert(tr[p].lc, l, mid, val, delta);
    }
    else {
        if (!tr[p].rc) tr[p].rc = build(); // 右子树不存在，动态开点
        insert(tr[p].rc, mid + 1, r, val, delta);
    }
    tr[p].dat = max(tr[tr[p].lc].dat, tr[tr[p].rc].dat);
}

// 调用
insert(root, 1, n, val, delta);

// 合并两棵线段树
int merge(int p, int q, int l, int r) {
    if (!p) return q; // p,q之一为空
    if (!q) return p;
    if (l == r) { // 到达叶子
        tr[p].dat += tr[q].dat;
        return p;
    }
    int mid = (l + r) >> 1;
    tr[p].lc = merge(tr[p].lc, tr[q].lc, l, mid); // 递归合并左子树
    tr[p].rc = merge(tr[p].rc, tr[q].rc, mid + 1, r); // 递归合并右子树
    tr[p].dat = max(tr[tr[p].lc].dat, tr[tr[p].rc].dat); // 更新最值
    return p; // 以p为合并后的节点，相当于删除q
}
struct BST {
	int l, r; // 左右子节点在数组中的下标
	int val;  // 节点关键码
} a[SIZE]; // 数组模拟链表
int tot, root, INF = 1<<30;

int New(int val) {
	a[++tot].val = val;
	return tot;
}

void Build() {
	New(-INF), New(INF);
	root = 1, a[1].r = 2;
}

int Get(int p, int val) {
	if (p == 0) return 0; // 检索失败
	if (val == a[p].val) return p; // 检索成功
	return val < a[p].val ? Get(a[p].l, val) : Get(a[p].r, val);
}

void Insert(int &p, int val) {
	if (p == 0) {
		p = New(val); // 注意p是引用，其父节点的l或r值会被同时更新
		return;
	}
	if (val == a[p].val) return;
	if (val < a[p].val) Insert(a[p].l, val);
	else Insert(a[p].r, val);
}

int GetNext(int val) {
	int ans = 2; // a[2].val==INF
	int p = root;
	while (p) {
		if (val == a[p].val) { // 检索成功
			if (a[p].r > 0) { // 有右子树
				p = a[p].r;
				// 右子树上一直向左走
				while (a[p].l > 0) p = a[p].l;
				ans = p;
			}
			break;
		}
		// 每经过一个节点，都尝试更新后继
		if (a[p].val > val && a[p].val < a[ans].val) ans = p;
		p = val < a[p].val ? a[p].l : a[p].r;
	}
	return ans;
}

void Remove(int &p, int val) { // 从子树p中删除值为val的节点
	if (p == 0) return;
	if (val == a[p].val) { // 已经检索到值为val的节点
		if (a[p].l == 0) { // 没有左子树
			p = a[p].r; // 右子树代替p的位置，注意p是引用
		}
		else if (a[p].r == 0) { // 没有右子树
			p = a[p].l; // 左子树代替p的位置，注意p是引用
		}
		else { // 既有左子树又有右子树
			// 求后继节点
			int next = a[p].r;
			while (a[next].l > 0) next = a[next].l;
			// next一定没有左子树，直接删除
			Remove(a[p].r, a[next].val);
			// 令节点next代替节点p的位置
			a[next].l = a[p].l, a[next].r = a[p].r;
			p = next; // 注意p是引用
		}
		return;
	}
	if (val < a[p].val) {
		Remove(a[p].l, val);
	} else {
		Remove(a[p].r, val);
	}
}

void zig(int &p) {
	int q = a[p].l;
	a[p].l = a[q].r, a[q].r = p;
	p = q; // 注意p是引用
}

void zag(int &p) {
	int q = a[p].r;
	a[p].r = a[q].l, a[q].l = p;
	p = q; // 注意p是引用
}
﻿// 例题：POJ2104（基于值域的整体分治算法）
#include <iostream>
#include <cstdio>
#include <cstring>
#include <algorithm>
using namespace std;
const int N = 100010, INF = 1e9;
struct rec {int op, x, y, z;} q[2 * N], lq[2 * N], rq[2 * N];
int n, m, t, c[N], ans[N];

int ask(int x) {
    int y = 0;
    for (; x; x -= x & -x) y += c[x];
    return y;
}

void change(int x, int y) {
    for (; x <= n; x += x & -x) c[x] += y;
}

void solve(int lval, int rval, int st, int ed) {
    if (st > ed) return;
    if (lval == rval) {
        for (int i = st; i <= ed; i++)
            if (q[i].op > 0) ans[q[i].op] = lval;
        return;
    }
    int mid = (lval + rval) >> 1;
    int lt = 0, rt = 0;
    for (int i = st; i <= ed; i++) {
        if (q[i].op == 0) { // 是一次赋值操作
            if (q[i].y <= mid) change(q[i].x, 1), lq[++lt] = q[i];
            else rq[++rt] = q[i];
        } else { // 是一次询问
            int cnt = ask(q[i].y) - ask(q[i].x - 1);
            if (cnt >= q[i].z) lq[++lt] = q[i];
            else q[i].z -= cnt, rq[++rt] = q[i];
        }
    }
    for (int i = ed; i >= st; i--) { // 还原树状数组
        if (q[i].op == 0 && q[i].y <= mid) change(q[i].x, -1);
    }
    for (int i = 1; i <= lt; i++) q[st + i - 1] = lq[i];
    for (int i = 1; i <= rt; i++) q[st + lt + i - 1] = rq[i];
    solve(lval, mid, st, st + lt - 1);
    solve(mid + 1, rval, st + lt, ed);
}

int main() {
    cin >> n >> m;
    for (int i = 1; i <= n; i++) {
        int val; scanf("%d", &val);
        // a[i]=val等价于一次把第i个数赋值为val的操作
        q[++t].op = 0, q[t].x = i, q[t].y = val;
    }
    for (int i = 1; i <= m; i++) {
        int l, r, k; scanf("%d%d%d", &l, &r, &k);
        // 记录一次询问
        q[++t].op = i, q[t].x = l, q[t].y = r, q[t].z = k;
    }
    // 基于值域对t=n+m个操作进行整体分治
    solve(-INF, INF, 1, t);
    for (int i = 1; i <= m; i++) printf("%d\n", ans[i]);
}
﻿// 可持久化Trie，例题：BZOJ3261
const int N = 600010;
int trie[N*24][2], latest[N*24]; // latest和end可合并为一个数组
int s[N], root[N], n, m, tot;
// 本题需要统计子树latest，故使用递归插入s[i]，当前为s[i]的第k位
void insert(int i, int k, int p, int q) {
	if (k < 0) {
		latest[q] = i;
		return;
	}
	int c = s[i] >> k & 1;
	if (p) trie[q][c ^ 1] = trie[p][c ^ 1];
	trie[q][c] = ++tot;
	insert(i, k - 1, trie[p][c], trie[q][c]);
	latest[q] = max(latest[trie[q][0]], latest[trie[q][1]]);
}

int ask(int now, int val, int k, int limit) {
	if (k < 0) return s[latest[now]] ^ val;
	int c = val >> k & 1;
	if (latest[trie[now][c ^ 1]] >= limit)
		return ask(trie[now][c ^ 1], val, k - 1, limit);
	else
		return ask(trie[now][c], val, k - 1, limit);
}

int main() {
	cin >> n >> m;
	latest[0] = -1;
	root[0] = ++tot;
	insert(0, 23, 0, root[0]);
	for (int i = 1; i <= n; i++) {
		int x; scanf("%d", &x);
		s[i] = s[i - 1] ^ x;
		root[i] = ++tot;
		insert(i, 23, root[i - 1], root[i]);
	}
	for (int i = 1; i <= m; i++) {
		char op[2]; scanf("%s", op);
		if (op[0] == 'A') {
			int x; scanf("%d", &x);
			root[++n] = ++tot;
			s[n] = s[n - 1] ^ x;
			insert(n, 23, root[n - 1], root[n]);
		}
		else {
			int l, r, x; scanf("%d%d%d", &l, &r, &x);
			printf("%d\n", ask(root[r - 1], x ^ s[n], 23, l - 1));
		}
	}
}


// 可持久化线段树，建树
struct SegmentTree {
    int lc, rc; // 左右子节点编号
    int dat; // 区间最大值
} tree[MAX_MLOGN];
int tot, root[MAX_M]; // 可持久化线段树的总点数和每个根
int n, a[MAX_N];

int build(int l, int r) {
    int p = ++tot;
    if (l == r) { tree[p].dat = a[l]; return p; }
    int mid = (l + r) >> 1;
    tree[p].lc = build(l, mid);
    tree[p].rc = build(mid + 1, r);
    tree[p].dat = max(tree[tree[p].lc].dat, tree[tree[p].rc].dat);
    return p;
}
// 在main函数中
root[0] = build(1, n);


// 可持久化线段树，单点修改
int insert(int now, int l, int r, int x, int val) {
    int p = ++tot;
    tree[p] = tree[now]; 
    if (l == r) {
        tree[p].dat = val; 
        return p;
    }
    int mid = (l + r) >> 1;
    if (x <= mid) 
tree[p].lc = insert(tree[now].lc, l, mid, x, val);
    else 
tree[p].rc = insert(tree[now].rc, mid + 1, r, x, val);
    tree[p].dat = max(tree[tree[p].lc].dat, tree[tree[p].rc].dat);
    return p;
}
// 在main函数中
root[i] = insert(root[i - 1], 1, n, x, val);


// 可持久化线段树，例题：POJ2104 K-th Number，查询部分
// 在p, q两个节点上，值域为 [l,r]，求第k小数
int ask(int p, int q, int l, int r, int k) {
    if (l == r) return l; // 找到答案
    int mid = (l + r) >> 1;
    // 有多少个数落在值域 [l,mid] 内
    int lcnt = tree[tree[p].lc].cnt - tree[tree[q].lc].cnt;
    if (k <= lcnt) return ask(tree[p].lc, tree[q].lc, l, mid, k);
    else return ask(tree[p].rc, tree[q].rc, mid + 1, r, k - lcnt);
}
// 在main函数中，询问为(li,ri,ki)
int ans = ask(root[ri], root[li - 1], 1, t, ki);
// 例题：LCIS，O(N^3)
for (int i = 1; i <= n; i++)
    for (int j = 1; j <= m; j++)
        if (a[i] == b[j]) {
            for (int k = 0; k < j; k++)
                if (b[k] < a[i])
                    f[i][j] = max(f[i][j], f[i - 1][k] + 1);
        }
        else f[i][j] = f[i - 1][j];


// 例题：LCIS，O(N^2)
for (int i = 1; i <= n; i++) {
    // val是决策集合S(i,j)中f[i-1][k]的最大值
    int val = 0;
    // j=1时，0可以作为k的取值
    if (b[0] < a[i]) val = f[i - 1][0];
    for (int j = 1; j <= m; j++) {
        if (a[i] == b[j]) f[i][j] = val + 1;
        else f[i][j] = f[i - 1][j];
        // j即将增大为j+1，检查j能否进入新的决策集合
        if (b[j] < a[i]) val = max(val, f[i - 1][j]);
    }
}
﻿// 01背包 ==========================================

memset(f, 0xcf, sizeof(f)); // -INF
f[0][0] = 0;
for (int i = 1; i <= n; i++) {
    for (int j = 0; j <= m; j++)
        f[i][j] = f[i - 1][j];
    for (int j = v[i]; j <= m; j++)
        f[i][j] = max(f[i][j], f[i - 1][j - v[i]] + w[i]);
}

int f[2][MAX_M+1];
memset(f, 0xcf, sizeof(f)); // -INF
f[0][0] = 0;
for (int i = 1; i <= n; i++) {
    for (int j = 0; j <= m; j++)
        f[i & 1][j] = f[(i - 1) & 1][j];
    for (int j = v[i]; j <= m; j++)
        f[i & 1][j] = max(f[i & 1][j], f[(i - 1) & 1][j - v[i]] + w[i]);
}
int ans = 0;
for (int j = 0; j <= m; j++)
    ans = max(ans, f[n & 1][j]);

int f[MAX_M+1];
memset(f, 0xcf, sizeof(f)); // -INF
f[0] = 0;
for (int i = 1; i <= n; i++)
    for (int j = m; j >= v[i]; j--)
        f[j] = max(f[j], f[j - v[i]] + w[i]);
int ans = 0;
for (int j = 0; j <= m; j++)
    ans = max(ans, f[j]);


// 例题：数字组合 ==========================================

int f[MAX_M+1];
memset(f, 0, sizeof(f));
f[0] = 1;
for (int i = 1; i <= n; i++)
    for (int j = m; j >= a[i]; j--)
        f[j] += f[j - a[i]];
cout << f[m] <<endl;


// 完全背包 ==========================================

int f[MAX_M+1];
memset(f, 0xcf, sizeof(f)); // -INF
f[0] = 0;
for (int i = 1; i <= n; i++)
    for (int j = v[i]; j <= m; j--)
        f[j] = max(f[j], f[j - v[i]] + w[i]);
int ans = 0;
for (int j = 0; j <= m; j++)
    ans = max(ans, f[j]);


// 例题：自然数拆分Lunatic版 ==========================================

unsigned int f[MAX_M+1];
memset(f, 0, sizeof(f));
f[0] = 1;
for (int i = 1; i <= n; i++)
    for (int j = i; j <= n; j++)
        f[j] = (f[j] + f[j - i]) % 2147483648u;
cout << (f[n] > 0 ? f[n] - 1 : 2147483647) <<endl;


// 多重背包，直接拆分 ==========================================

unsigned int f[MAX_M+1];
memset(f, 0xcf, sizeof(f)); // -INF
f[0] = 0;
for (int i = 1; i <= n; i++)
    for (int j = 1; j <= c[i]; j++)
        for (int k = m; k >= v[i]; k--)
            f[k] = max(f[k], f[k - v[i]] + w[i]);
int ans = 0;
for (int i = 0; i <= m; i++)
    ans = max(ans, f[i]);


// 例题：POJ1742 Coins (1) ==========================================

bool f[100010];
memset(f, 0, sizeof(f));
f[0] = true;
for (int i = 1; i <= n; i++)
    for (int j = 1; j <= c[i]; j++)
        for (int k = m; k >= a[i]; k--)
            f[k] |= f[k - a[i]];
int ans = 0;
for (int i = 1; i <= m; i++)
    ans += f[i];


// 例题：POJ1742 Coins (2) ==========================================

int used[100010];
for (int i = 1; i <= n; i++) {
    for (int j = 0; j <= m; j++) used[j] = 0;
    for (int j = a[i]; j <= m; j++)
        if (!f[j] && f[j - a[i]] && used[j - a[i]] < c[i])
            f[j] = true, used[j] = used[j - a[i]] + 1;
}


// 分组背包 ==========================================

memset(f, 0xcf, sizeof(f));
f[0] = 0;
for (int i = 1; i <= n; i++)
    for (int j = m; j >= 0; j--)
        for (int k = 1; k <= c[i]; k++)
            if (j >= v[i][k])
                f[j] = max(f[j], f[j - v[i][k]] + w[i][k]);
// 例题：石子合并
memset(f, 0x3f, sizeof(f)); // INF
for (int i = 1; i <= n; i++) {
    f[i][i] = 0;
    sum[i] = sum[i-1] + a[i]; // 前缀和
}
for (int len = 2; len <= n; len++) // 阶段
    for (int l = 1; l <= n - len + 1; l++) { // 状态：左端点
        int r = l + len - 1; // 状态：右端点
        for (int k = l; k < r; k++) // 决策
            f[l][r] = min(f[l][r], f[l][k] + f[k+1][r]);
        f[l][r] += sum[r] - sum[l-1];
    }


// 例题：金字塔
const int MOD = 1000000000; // 对MOD取模
int f[310][310];

int solve(int l, int r) {
    if (l > r) return 0; // 递归边界
    if (l == r) return 1; // 递归边界
    if (f[l][r] != -1) return f[l][r]; // 记忆化
    f[l][r] = 0;
    for (int k = l + 2; k <= r; k++)
        f[l][r] = (f[l][r] + (long long)solve(l+1, k-1) * solve(k, r)) % MOD;
    return f[l][r];
}

memset(f, -1, sizeof(f)); // -1表示没有被计算过
solve(1, n);
// 例题：The Battle of Chibi
// 暴力枚举决策
const int mod = 1000000007;
memset(f, 0 ,sizeof(f));
a[0] = -(1<<30); // -INF
f[0][0] = 1;
for (int i = 1; i <= m; i++)
    for (int j = 1; j <= n; j++)
        for (int k = 0; k < j; k++)
            if (a[k] < a[j])
                f[i][j] = (f[i][j] + f[i-1][k]) % mod;
int ans = 0;
for (int i = 1; i <= n; i++)
    ans = (ans + f[m][i]) % mod;

// 树状数组优化
for (int i = 1; i <= m; i++) {
    memset(c, 0, sizeof(c));
    add(val(a[0]), f[i-1][0]); // 树状数组add操作
    for (int j = 1; j <= n; j++) {
        f[i][j] = ask(val(a[j]) - 1); // 树状数组ask操作
        add(val(a[j]), f[i-1][j]); // 树状数组add操作
    }
}
// 单调队列优化多重背包
#include<iostream>
#include<cstdio>
#include<cstring>
#include<algorithm>
#include<queue>
#include<string>
using namespace std;
int n, m, V[210], W[210], C[210];
int f[20010], q[20010];

int calc(int i, int u, int k) {
	return f[u + k*V[i]] - k*W[i];
}

int main() {
	cin >> n >> m;
	memset(f, 0xcf, sizeof(f)); // -INF
	f[0] = 0;
	// 物品种类
	for (int i = 1; i <= n; i++) {
		scanf("%d%d%d", &V[i], &W[i], &C[i]);
		// 除以V[i]的余数
		for (int u = 0; u < V[i]; u++) {
			// 建立单调队列
			int l = 1, r = 0;
			// 把最初的候选集合插入队列
			int maxp = (m - u) / V[i];
			for (int k = maxp - 1; k >= max(maxp - C[i], 0); k--) {
				while (l <= r && calc(i, u, q[r]) <= calc(i, u, k)) r--;
				q[++r] = k;
			}
			// 倒序循环每个状态
			for (int p = maxp; p >= 0; p--) {
				// 排除过时决策
				while (l <= r && q[l] > p - 1) l++;
				// 取队头进行状态转移
				if (l <= r)
					f[u + p*V[i]] = max(f[u + p*V[i]], calc(i, u, q[l]) + p*W[i]);
				// 插入新决策，同时维护队尾单调性
				if (p - C[i] - 1 >= 0) {
					while (l <= r && calc(i, u, q[r]) <= calc(i, u, p - C[i] - 1)) r--;
					q[++r] = p - C[i] - 1;
				}
			}
		}
	}
	int ans = 0;
	for (int i = 1; i <= m; i++) ans = max(ans, f[i]);
	cout << ans << endl;
}// Dijkstra算法，O(n^2)
#include <iostream>
#include <cstdio>
#include <cstring>
#include <algorithm>
using namespace std;
int a[3010][3010], d[3010];
bool v[3010];
int n, m;

void dijkstra() {
	memset(d, 0x3f, sizeof(d)); // dist数组
	memset(v, 0, sizeof(v)); // 节点标记
	d[1] = 0;
	for (int i = 1; i < n; i++) { // 重复进行n-1次
		int x = 0;
		// 找到未标记节点中dist最小的
		for (int j = 1; j <= n; j++)
			if (!v[j] && (x == 0 || d[j] < d[x])) x = j;
		v[x] = 1;
		// 用全局最小值点x更新其它节点
		for (int y = 1; y <= n; y++)
			d[y] = min(d[y], d[x] + a[x][y]);
	}
}

int main() {
	cin >> n >> m;
	// 构建邻接矩阵
	memset(a, 0x3f, sizeof(a));
	for (int i = 1; i <= n; i++) a[i][i] = 0;
	for (int i = 1; i <= m; i++) {
		int x, y, z;
		scanf("%d%d%d", &x, &y, &z);
		a[x][y] = min(a[x][y], z);
	}
	// 求单源最短路径
	dijkstra();
	for (int i = 1; i <= n; i++)
		printf("%d\n", d[i]);
}


// 堆优化Dijkstra算法，O(mlogn)
#include <iostream>
#include <cstdio>
#include <cstring>
#include <algorithm>
#include <queue>
using namespace std;
const int N = 100010, M = 1000010;
int head[N], ver[M], edge[M], Next[M], d[N];
bool v[N];
int n, m, tot;
// 大根堆（优先队列），pair的第二维为节点编号
// pair的第一维为dist的相反数（利用相反数变成小根堆，参见0x71节）
priority_queue< pair<int, int> > q;

void add(int x, int y, int z) {
	ver[++tot] = y, edge[tot] = z, Next[tot] = head[x], head[x] = tot;
}

void dijkstra() {
	memset(d, 0x3f, sizeof(d)); // dist数组
	memset(v, 0, sizeof(v)); // 节点标记
	d[1] = 0;
	q.push(make_pair(0, 1));
	while (q.size()) {
		// 取出堆顶
		int x = q.top().second; q.pop();
		if (v[x]) continue;
		v[x] = 1;
		// 扫描所有出边
		for (int i = head[x]; i; i = Next[i]) {
			int y = ver[i], z = edge[i];
			if (d[y] > d[x] + z) {
				// 更新，把新的二元组插入堆
				d[y] = d[x] + z;
				q.push(make_pair(-d[y], y));
			}
		}
	}
}

int main() {
	cin >> n >> m;
	// 构建邻接表
	for (int i = 1; i <= m; i++) {
		int x, y, z;
		scanf("%d%d%d", &x, &y, &z);
		add(x, y, z);
	}
	// 求单源最短路径
	dijkstra();
	for (int i = 1; i <= n; i++)
		printf("%d\n", d[i]);
}


// SPFA算法
#include <iostream>
#include <cstdio>
#include <cstring>
#include <algorithm>
#include <queue>
using namespace std;
const int N = 100010, M = 1000010;
int head[N], ver[M], edge[M], Next[M], d[N];
int n, m, tot;
queue<int> q;
bool v[N];

void add(int x, int y, int z) {
	ver[++tot] = y, edge[tot] = z, Next[tot] = head[x], head[x] = tot;
}

void spfa() {
	memset(d, 0x3f, sizeof(d)); // dist数组
	memset(v, 0, sizeof(v)); // 是否在队列中
	d[1] = 0; v[1] = 1;
	q.push(1);
	while (q.size()) {
		// 取出队头
		int x = q.front(); q.pop();
		v[x] = 0;
		// 扫描所有出边
		for (int i = head[x]; i; i = Next[i]) {
			int y = ver[i], z = edge[i];
			if (d[y] > d[x] + z) {
				// 更新，把新的二元组插入堆
				d[y] = d[x] + z;
				if (!v[y]) q.push(y), v[y] = 1;
			}
		}
	}
}

int main() {
	cin >> n >> m;
	// 构建邻接表
	for (int i = 1; i <= m; i++) {
		int x, y, z;
		scanf("%d%d%d", &x, &y, &z);
		add(x, y, z);
	}
	// 求单源最短路径
	spfa();
	for (int i = 1; i <= n; i++)
		printf("%d\n", d[i]);
}


// Floyd算法，(n^3)
#include <iostream>
#include <cstdio>
#include <cstring>
#include <algorithm>
using namespace std;
int d[310][310];
int n, m;

int main() {
	cin >> n >> m;
	// 把d数组初始化为邻接矩阵
	memset(d, 0x3f, sizeof(d));
	for (int i = 1; i <= n; i++) d[i][i] = 0;
	for (int i = 1; i <= m; i++) {
		int x, y, z;
		scanf("%d%d%d", &x, &y, &z);
		d[x][y] = min(d[x][y], z);
	}
	// floyd求任意两点间最短路径
	for (int k = 1; k <= n; k++)
		for (int i = 1; i <= n; i++)
			for (int j = 1; j <= n; j++)
				d[i][j] = min(d[i][j], d[i][k] + d[k][j]);
	// 输出
	for (int i = 1; i <= n; i++) {
		for (int j = 1; j <= n; j++) printf("%d ", d[i][j]);
		puts("");
	}
}


// Floyd传递闭包
#include <iostream>
#include <cstdio>
#include <cstring>
#include <algorithm>
using namespace std;
bool d[310][310];
int n, m;

int main() {
	cin >> n >> m;
	for (int i = 1; i <= n; i++) d[i][i] = 1;
	for (int i = 1; i <= m; i++) {
		int x, y;
		scanf("%d%d", &x, &y);
		d[x][y] = d[y][x] = 1;
	}
	for (int k = 1; k <= n; k++)
		for (int i = 1; i <= n; i++)
			for (int j = 1; j <= n; j++)
				d[i][j] |= d[i][k] & d[k][j];
}


// 例题：Floyd求最小环 (POJ1734)
#include <iostream>
#include <cstdio>
#include <cstring>
#include <algorithm>
#include <vector>
using namespace std;
int a[310][310], d[310][310], pos[310][310];
int n, m, ans = 0x3f3f3f3f;
vector<int> path; //具体方案
void get_path(int x, int y) {
	if (pos[x][y] == 0) return;
	get_path(x, pos[x][y]);
	path.push_back(pos[x][y]);
	get_path(pos[x][y], y);
}
int main() {
	cin >> n >> m;
	memset(a, 0x3f, sizeof(a));
	for (int i = 1; i <= n; i++) a[i][i] = 0;
	for (int i = 1; i <= m; i++) {
		int x, y, z;
		scanf("%d%d%d", &x, &y, &z);
		a[y][x] = a[x][y] = min(a[x][y], z);
	}
	memcpy(d, a, sizeof(a));
	for (int k = 1; k <= n; k++) {
		for (int i = 1; i < k; i++)
			for (int j = i + 1; j < k; j++)
				if ((long long)d[i][j] + a[j][k] + a[k][i] < ans) {
					ans = d[i][j] + a[j][k] + a[k][i];
					path.clear();
					path.push_back(i);
					get_path(i, j);
					path.push_back(j);
					path.push_back(k);
				}
		for (int i = 1; i <= n; i++)
			for (int j = 1; j <= n; j++)
				if (d[i][j] > d[i][k] + d[k][j]) {
					d[i][j] = d[i][k] + d[k][j];
					pos[i][j] = k;
				}
	}
	if (ans == 0x3f3f3f3f) {
		puts("No solution.");
		return 0;
	}
	for (int i = 0; i < path.size(); i++)
		printf("%d ", path[i]);
	puts("");
}
// Kruskal算法，O(mlogm)
#include <iostream>
#include <cstdio>
#include <cstring>
#include <algorithm>
using namespace std;
struct rec { int x, y, z; } edge[500010];
int fa[100010], n, m, ans;
bool operator <(rec a, rec b) {
    return a.z < b.z;
}
int get(int x) {
    if (x == fa[x]) return x;
    return fa[x] = get(fa[x]);
}
int main() {
    cin >> n >> m;
    for (int i = 1; i <= m; i++)
        scanf("%d%d%d", &edge[i].x, &edge[i].y, &edge[i].z);
    // 按照边权排序
    sort(edge + 1, edge + m + 1);
    // 并查集初始化
    for (int i = 1; i <= n; i++) fa[i] = i;
    // 求最小生成树
    for (int i = 1; i <= m; i++) {
        int x = get(edge[i].x);
        int y = get(edge[i].y);
        if (x == y) continue;
        fa[x] = y;
        ans += edge[i].z;
    }
    cout << ans << endl;
}


// Prim算法，O(n^2)
#include <iostream>
#include <cstdio>
#include <cstring>
#include <algorithm>
using namespace std;
int a[3010][3010], d[3010];
bool v[3010];
int n, m, ans;

void prim() {
	memset(d, 0x3f, sizeof(d));
	memset(v, 0, sizeof(v));
	d[1] = 0;
	for (int i = 1; i < n; i++) {
		int x = 0;
		for (int j = 1; j <= n; j++)
			if (!v[j] && (x == 0 || d[j] < d[x])) x = j;
		v[x] = 1;
		for (int y = 1; y <= n; y++)
			if (!v[y]) d[y] = min(d[y], a[x][y]);
	}
}

int main() {
	cin >> n >> m;
	// 构建邻接矩阵
	memset(a, 0x3f, sizeof(a));
	for (int i = 1; i <= n; i++) a[i][i] = 0;
	for (int i = 1; i <= m; i++) {
		int x, y, z;
		scanf("%d%d%d", &x, &y, &z);
		a[y][x] = a[x][y] = min(a[x][y], z);
	}
	// 求最小生成树
	prim();
    for (int i = 2; i <= n; i++) ans += d[i];
    cout << ans << endl;
}
// 树形DP求树的直径
void dp(int x) {
    v[x] = 1;
    for (int i = head[x]; i; i = Next[i]) {
        int y = ver[i];
        if (v[y]) continue;
        dp(y);
        ans = max(ans, d[x] + d[y] + edge[i]);
        d[x] = max(d[x], d[y] + edge[i]);
    }
}


// 树上倍增法求LCA (模板题：HDOJ2586)
#include<iostream>
#include<cstdio>
#include<cstring>
#include<algorithm>
#include<queue>
#include<cmath>
using namespace std;
const int SIZE = 50010;
int f[SIZE][20], d[SIZE], dist[SIZE];
int ver[2 * SIZE], Next[2 * SIZE], edge[2 * SIZE], head[SIZE];
int T, n, m, tot, t;
queue<int> q;

void add(int x, int y, int z) {
	ver[++tot] = y; edge[tot] = z; Next[tot] = head[x]; head[x] = tot;
}

// 预处理
void bfs() {
	q.push(1); d[1] = 1;
	while (q.size()) {
		int x = q.front(); q.pop();
		for (int i = head[x]; i; i = Next[i]) {
			int y = ver[i];
			if (d[y]) continue;
			d[y] = d[x] + 1;
			dist[y] = dist[x] + edge[i];
			f[y][0] = x;
			for (int j = 1; j <= t; j++)
				f[y][j] = f[f[y][j - 1]][j - 1];
			q.push(y);
		}
	}
}

// 回答一个询问
int lca(int x, int y) {
	if (d[x] > d[y]) swap(x, y);
	for (int i = t; i >= 0; i--)
		if (d[f[y][i]] >= d[x]) y = f[y][i];
	if (x == y) return x;
	for (int i = t; i >= 0; i--)
		if (f[x][i] != f[y][i]) x = f[x][i], y = f[y][i];
	return f[x][0];
}

int main() {
	cin >> T;
	while (T--) {
		cin >> n >> m;
		t = (int)(log(n) / log(2)) + 1;
		// 清空
		for (int i = 1; i <= n; i++) head[i] = d[i] = 0;
		tot = 0;
		// 读入一棵树
		for (int i = 1; i < n; i++) {
			int x, y, z;
			scanf("%d%d%d", &x, &y, &z);
			add(x, y, z), add(y, x, z);
		}
		bfs();
		// 回答问题
		for (int i = 1; i <= m; i++) {
			int x, y;
			scanf("%d%d", &x, &y);
			printf("%d\n", dist[x] + dist[y] - 2 * dist[lca(x, y)]);
		}
	}
}


// Tarjan算法离线求LCA (模板题：HDOJ2586)
#include<iostream>
#include<cstdio>
#include<cstring>
#include<algorithm>
#include<vector>
using namespace std;
const int SIZE = 50010;
int ver[2 * SIZE], Next[2 * SIZE], edge[2 * SIZE], head[SIZE];
int fa[SIZE], d[SIZE], v[SIZE], lca[SIZE], ans[SIZE];
vector<int> query[SIZE], query_id[SIZE];
int T, n, m, tot, t;

void add(int x, int y, int z) {
	ver[++tot] = y; edge[tot] = z; Next[tot] = head[x]; head[x] = tot;
}

void add_query(int x, int y, int id) {
	query[x].push_back(y), query_id[x].push_back(id);
	query[y].push_back(x), query_id[y].push_back(id);
}

int get(int x) {
	if (x == fa[x]) return x;
	return fa[x] = get(fa[x]);
}

void tarjan(int x) {
	v[x] = 1;
	for (int i = head[x]; i; i = Next[i]) {
		int y = ver[i];
		if (v[y]) continue;
		d[y] = d[x] + edge[i];
		tarjan(y);
		fa[y] = x;
	}
	for (int i = 0; i < query[x].size(); i++) {
		int y = query[x][i];
		int id = query_id[x][i];
		if (v[y] == 2) {
			int lca = get(y);
			ans[id] = min(ans[id], d[x] + d[y] - 2 * d[lca]);
		}
	}
	v[x] = 2;
}

int main() {
	cin >> T;
	while (T--) {
		cin >> n >> m;
		for (int i = 1; i <= n; i++) {
			head[i] = 0;
			query[i].clear(), query_id[i].clear();
			fa[i] = i, v[i] = 0;
		}
		tot = 0;
		for (int i = 1; i < n; i++) {
			int x, y, z;
			scanf("%d%d%d", &x, &y, &z);
			add(x, y, z), add(y, x, z);
		}
		for (int i = 1; i <= m; i++) {
			int x, y;
			scanf("%d%d", &x, &y);
			if (x == y) ans[i] = 0;
			else {
				add_query(x, y, i);
				ans[i] = 1 << 30;
			}
		}
		tarjan(1);
		for (int i = 1; i <= m; i++) printf("%d\n", ans[i]);
	}
}
// tarjan算法求无向图的桥、边双连通分量并缩点
#include<iostream>
#include<cstdio>
#include<cstring>
#include<algorithm>
#include<vector>
using namespace std;
const int SIZE = 100010;
int head[SIZE], ver[SIZE * 2], Next[SIZE * 2];
int dfn[SIZE], low[SIZE], c[SIZE];
int n, m, tot, num, dcc, tc;
bool bridge[SIZE * 2];
int hc[SIZE], vc[SIZE * 2], nc[SIZE * 2];

void add(int x, int y) {
	ver[++tot] = y, Next[tot] = head[x], head[x] = tot;
}

void add_c(int x, int y) {
	vc[++tc] = y, nc[tc] = hc[x], hc[x] = tc;
}

void tarjan(int x, int in_edge) {
	dfn[x] = low[x] = ++num;
	for (int i = head[x]; i; i = Next[i]) {
		int y = ver[i];
		if (!dfn[y]) {
			tarjan(y, i);
			low[x] = min(low[x], low[y]);
			if (low[y] > dfn[x])
				bridge[i] = bridge[i ^ 1] = true;
		}
		else if (i != (in_edge ^ 1))
			low[x] = min(low[x], dfn[y]);
	}
}

void dfs(int x) {
	c[x] = dcc;
	for (int i = head[x]; i; i = Next[i]) {
		int y = ver[i];
		if (c[y] || bridge[i]) continue;
		dfs(y);
	}
}

int main() {
	cin >> n >> m;
	tot = 1;
	for (int i = 1; i <= m; i++) {
		int x, y;
		scanf("%d%d", &x, &y);
		add(x, y), add(y, x);
	}
	for (int i = 1; i <= n; i++)
		if (!dfn[i]) tarjan(i, 0);
	for (int i = 2; i < tot; i += 2)
		if (bridge[i])
			printf("%d %d\n", ver[i ^ 1], ver[i]);

	for (int i = 1; i <= n; i++)
		if (!c[i]) {
			++dcc;
			dfs(i);
		}
	printf("There are %d e-DCCs.\n", dcc);
	for (int i = 1; i <= n; i++)
		printf("%d belongs to DCC %d.\n", i, c[i]);

	tc = 1;
	for (int i = 2; i <= tot; i++) {
		int x = ver[i ^ 1], y = ver[i];
		if (c[x] == c[y]) continue;
		add_c(c[x], c[y]);
	}
	printf("缩点之后的森林，点数 %d，边数 %d\n", dcc, tc / 2);
	for (int i = 2; i < tc; i += 2)
		printf("%d %d\n", vc[i ^ 1], vc[i]);
}


// tarjan算法求无向图的割点、点双连通分量并缩点
#include<iostream>
#include<cstdio>
#include<cstring>
#include<algorithm>
#include<vector>
using namespace std;
const int SIZE = 100010;
int head[SIZE], ver[SIZE * 2], Next[SIZE * 2];
int dfn[SIZE], low[SIZE], stack[SIZE], new_id[SIZE], c[SIZE];
int n, m, tot, num, root, top, cnt, tc;
bool cut[SIZE];
vector<int> dcc[SIZE];
int hc[SIZE], vc[SIZE * 2], nc[SIZE * 2];

void add(int x, int y) {
	ver[++tot] = y, Next[tot] = head[x], head[x] = tot;
}

void add_c(int x, int y) {
	vc[++tc] = y, nc[tc] = hc[x], hc[x] = tc;
}

void tarjan(int x) {
	dfn[x] = low[x] = ++num;
	stack[++top] = x;
	if (x == root && head[x] == 0) { // 孤立点
		dcc[++cnt].push_back(x);
		return;
	}
	int flag = 0;
	for (int i = head[x]; i; i = Next[i]) {
		int y = ver[i];
		if (!dfn[y]) {
			tarjan(y);
			low[x] = min(low[x], low[y]);
			if (low[y] >= dfn[x]) {
				flag++;
				if (x != root || flag > 1) cut[x] = true;
				cnt++;
				int z;
				do {
					z = stack[top--];
					dcc[cnt].push_back(z);
				} while (z != y);
				dcc[cnt].push_back(x);
			}
		}
		else low[x] = min(low[x], dfn[y]);
	}
}

int main() {
	cin >> n >> m;
	tot = 1;
	for (int i = 1; i <= m; i++) {
		int x, y;
		scanf("%d%d", &x, &y);
		if (x == y) continue;
		add(x, y), add(y, x);
	}
	for (int i = 1; i <= n; i++)
		if (!dfn[i]) root = i, tarjan(i);
	for (int i = 1; i <= n; i++)
		if (cut[i]) printf("%d ", i);
	puts("are cut-vertexes");
	for (int i = 1; i <= cnt; i++) {
		printf("v-DCC #%d:", i);
		for (int j = 0; j < dcc[i].size(); j++)
			printf(" %d", dcc[i][j]);
		puts("");
	}
	// 给每个割点一个新的编号(编号从cnt+1开始)
	num = cnt;
	for (int i = 1; i <= n; i++)
		if (cut[i]) new_id[i] = ++num;
	// 建新图，从每个v-DCC到它包含的所有割点连边
	tc = 1;
	for (int i = 1; i <= cnt; i++)
		for (int j = 0; j < dcc[i].size(); j++) {
			int x = dcc[i][j];
			if (cut[x]) {
				add_c(i, new_id[x]);
				add_c(new_id[x], i);
			}
			else c[x] = i; // 除割点外，其它点仅属于1个v-DCC
		}
	printf("缩点之后的森林，点数 %d，边数 %d\n", num, tc / 2);
	printf("编号 1~%d 的为原图的v-DCC，编号 >%d 的为原图割点\n", cnt, cnt);
	for (int i = 2; i < tc; i += 2)
		printf("%d %d\n", vc[i ^ 1], vc[i]);
}


// 求出欧拉图中的一条欧拉回路
#include<iostream>
#include<cstdio>
#include<cstring>
#include<algorithm>
using namespace std;
int head[100010], ver[1000010], Next[1000010], tot; // 邻接表
int stack[1000010], ans[1000010]; // 模拟系统栈，答案栈
bool vis[1000010];
int n, m, top, t;

void add(int x, int y) {
	ver[++tot] = y, Next[tot] = head[x], head[x] = tot;
}

void euler() {
	stack[++top] = 1;
	while (top > 0) {
		int x = stack[top], i = head[x];
		// 找到一条尚未访问的边
		while (i && vis[i]) i = Next[i];
		// 沿着这条边模拟递归过程，标记该边，并更新表头
		if (i) {
			stack[++top] = ver[i];
			head[x] = Next[i];
			vis[i] = vis[i ^ 1] = true;
		}		
		// 与x相连的所有边均已访问，模拟回溯过程，并记录于答案栈中
		else {
			top--;
			ans[++t] = x;
		}
	}
}

int main() {
	cin >> n >> m;
	tot = 1;
	for (int i = 1; i <= m; i++) {
		int x, y;
		scanf("%d%d", &x, &y);
		add(x, y), add(y, x);
	}
	euler();
	for (int i = t; i; i--) printf("%d\n", ans[i]);
}
// Tarjan算法求有向图强连通分量并缩点
#include<iostream>
#include<cstdio>
#include<cstring>
#include<algorithm>
#include<vector>
#include<queue>
using namespace std;
const int N = 100010, M = 1000010;
int ver[M], Next[M], head[N], dfn[N], low[N];
int stack[N], ins[N], c[N];
int vc[M], nc[M], hc[N], tc;
vector<int> scc[N];
int n, m, tot, num, top, cnt;

void add(int x, int y) {
	ver[++tot] = y, Next[tot] = head[x], head[x] = tot;
}

void add_c(int x, int y) {
	vc[++tc] = y, nc[tc] = hc[x], hc[x] = tc;
}

void tarjan(int x) {
	dfn[x] = low[x] = ++num;
	stack[++top] = x, ins[x] = 1;
	for (int i = head[x]; i; i = Next[i])
		if (!dfn[ver[i]]) {
			tarjan(ver[i]);
			low[x] = min(low[x], low[ver[i]]);
		}
		else if (ins[ver[i]])
			low[x] = min(low[x], dfn[ver[i]]);
	if (dfn[x] == low[x]) {
		cnt++; int y;
		do {
			y = stack[top--], ins[y] = 0;
			c[y] = cnt, scc[cnt].push_back(y);
		} while (x != y);
	}
}

int main() {
	cin >> n >> m;
	for (int i = 1; i <= m; i++) {
		int x, y;
		scanf("%d%d", &x, &y);
		add(x, y);
	}
	for (int i = 1; i <= n; i++)
		if (!dfn[i]) tarjan(i);
	for (int x = 1; x <= n; x++)
		for (int i = head[x]; i; i = Next[i]) {
			int y = ver[i];
			if (c[x] == c[y]) continue;
			add_c(c[x], c[y]);
		}
}


// 2-SAT构图并打印方案，解法一，自底向上拓扑排序 (POJ3683)
#include<iostream>
#include<cstdio>
#include<cstring>
#include<queue>
using namespace std;
const int u = 2010, w = 3000010;
int ver[w], Next[w], head[u], dfn[u], low[u], c[u], s[u], ins[u];
int ver2[w], Next2[w], head2[u], val[u], deg[u], opp[u];
int S[u], T[u], D[u], ex[w], ey[w];
int n, m, tot, tot2, num, t, p, e;
queue<int> q;

// 原图加边
void add(int x, int y) {
	ver[++tot] = y, Next[tot] = head[x], head[x] = tot;
    ex[++e] = x, ey[e] = y;
}

// 缩点后的图加边
void add2(int x, int y) {
    ver2[++tot2] = y, Next2[tot2] = head2[x], head2[x] = tot2;
}

void tarjan(int x) {
	dfn[x] = low[x] = ++num;
	s[++p] = x, ins[x] = 1;
	for (int i = head[x]; i; i = Next[i])
		if (!dfn[ver[i]]) {
			tarjan(ver[i]);
			low[x] = min(low[x], low[ver[i]]);
		}
		else if (ins[ver[i]])
			low[x] = min(low[x], low[ver[i]]);
	if (dfn[x] == low[x]) {
		t++; int y;
		do { y = s[p--], ins[y] = 0; c[y] = t; } while (x != y);
	}
}

void topsort() {
    memset(val, -1, sizeof(val));
    // 缩点，建反图
	for (int i = 1; i <= e; i++)
		if (c[ex[i]] != c[ey[i]])
			add2(c[ey[i]], c[ex[i]]), deg[c[ex[i]]]++;
    // 零入度点入队
	for (int i = 1; i <= t; i++)
		if (!deg[i]) q.push(i);
    // 拓扑排序
	while (q.size()) {
		int k = q.front(); q.pop();
        // 赋值标记
		if (val[k] == -1) val[k] = 0, val[opp[k]] = 1;
		for (int i = head2[k]; i; i = Next2[i])
			if (--deg[ver2[i]] == 0) q.push(ver2[i]);
	}
    // 输出最终结果
	for (int i = 1; i <= n; i++)
		if (val[c[i]] == 0) printf("%02d:%02d %02d:%02d\n",
			S[i] / 60, S[i] % 60,
			(S[i] + D[i]) / 60, (S[i] + D[i]) % 60);
		else printf("%02d:%02d %02d:%02d\n",
			(T[i] - D[i]) / 60, (T[i] - D[i]) % 60,
			T[i] / 60, T[i] % 60);
}

bool overlap(int a, int b, int c, int d) {
	if (a >= c&&a<d || b>c&&b <= d || a <= c&&b >= d) return 1;
	return 0;
}

int main() {
	cin >> n;
	for (int i = 1; i <= n; i++) 	{
        int sh, sm, th, tm;
		scanf("%d:%d %d:%d %d", &sh, &sm, &th, &tm, &D[i]);
		S[i] = sh * 60 + sm; T[i] = th * 60 + tm;
	}
	for (int i = 1; i < n; i++)
		for (int j = i + 1; j <= n; j++) {
            if (overlap(S[i], S[i] + D[i], S[j], S[j] + D[j]))
				add(i, n + j), add(j, n + i);
            if (overlap(S[i], S[i] + D[i], T[j] - D[j], T[j]))
				add(i, j), add(n + j, n + i);
            if (overlap(T[i] - D[i], T[i], S[j], S[j] + D[j]))
				add(n + i, n + j), add(j, i);
            if (overlap(T[i] - D[i], T[i], T[j] - D[j], T[j]))
				add(n + i, j), add(n + j, i);
        }
	for (int i = 1; i <= 2 * n; i++)
		if (!dfn[i]) tarjan(i);
	for (int i = 1; i <= n; i++) 	{
		if (c[i] == c[n + i]) { puts("NO"); return 0; }
		opp[c[i]] = c[n + i], opp[c[n + i]] = c[i];
	}
    puts("YES");
	topsort();
	return 0;
}


// 2-SAT构图并打印方案，解法二 (POJ3683)
#include<iostream>
#include<cstdio>
#include<cstring>
#include<algorithm>
using namespace std;
const int u = 2010, w = 3000010;
int ver[w], Next[w], head[u], dfn[u], low[u], c[u], s[u], ins[u];
int val[u], deg[u], opp[u], S[u], T[u], D[u];
int n, m, tot, num, t, p;

// 原图加边
void add(int x, int y) {
	ver[++tot] = y, Next[tot] = head[x], head[x] = tot;
}

void tarjan(int x) {
	dfn[x] = low[x] = ++num;
	s[++p] = x, ins[x] = 1;
	for (int i = head[x]; i; i = Next[i])
		if (!dfn[ver[i]]) {
			tarjan(ver[i]);
			low[x] = min(low[x], low[ver[i]]);
		}
		else if (ins[ver[i]])
			low[x] = min(low[x], low[ver[i]]);
	if (dfn[x] == low[x]) {
		t++; int y;
		do { y = s[p--], ins[y] = 0; c[y] = t; } while (x != y);
	}
}

bool overlap(int a, int b, int c, int d) {
	if (a >= c&&a<d || b>c&&b <= d || a <= c&&b >= d) return 1;
	return 0;
}

int main() {
	cin >> n;
	for (int i = 1; i <= n; i++) 	{
        int sh, sm, th, tm;
		scanf("%d:%d %d:%d %d", &sh, &sm, &th, &tm, &D[i]);
		S[i] = sh * 60 + sm; T[i] = th * 60 + tm;
	}
	for (int i = 1; i < n; i++)
		for (int j = i + 1; j <= n; j++) {
            if (overlap(S[i], S[i] + D[i], S[j], S[j] + D[j]))
				add(i, n + j), add(j, n + i);
            if (overlap(S[i], S[i] + D[i], T[j] - D[j], T[j]))
				add(i, j), add(n + j, n + i);
            if (overlap(T[i] - D[i], T[i], S[j], S[j] + D[j]))
				add(n + i, n + j), add(j, i);
            if (overlap(T[i] - D[i], T[i], T[j] - D[j], T[j]))
				add(n + i, j), add(n + j, i);
        }
	for (int i = 1; i <= 2 * n; i++)
		if (!dfn[i]) tarjan(i);
	for (int i = 1; i <= n; i++) 	{
		if (c[i] == c[n + i]) { puts("NO"); return 0; }
		opp[i] = n + i, opp[n + i] = i;
	}
    puts("YES");
    // 构造方案 
    for (int i = 1; i <= 2 * n; i++)
    	val[i] = c[i] > c[opp[i]];
    // 输出最终结果
	for (int i = 1; i <= n; i++)
		if (val[i] == 0) printf("%02d:%02d %02d:%02d\n",
			S[i] / 60, S[i] % 60,
			(S[i] + D[i]) / 60, (S[i] + D[i]) % 60);
		else printf("%02d:%02d %02d:%02d\n",
			(T[i] - D[i]) / 60, (T[i] - D[i]) % 60,
			T[i] / 60, T[i] % 60);
	return 0;
}

﻿// 二分图最大匹配：匈牙利算法
bool dfs(int x) {
	for (int i = head[x], y; i; i = next[i])
		if (!visit[y = ver[i]]) {
			visit[y] = 1;
			if (!match[y] || dfs(match[y])) {
                match[y]=x;
                return true;
            }
		}
	return false;
}

for (int i = 1; i <= n; i++) {
	memset(visit, 0, sizeof(visit));
    if (dfs(i)) ans++;
}

// 二分图带权最大匹配：KM算法
const int N = 105;
int w[N][N]; // 边权
int la[N], lb[N]; // 左、右部点的顶标
bool va[N], vb[N]; // 访问标记：是否在交错树中
int match[N]; // 右部点匹配了哪一个左部点
int n, delta, upd[N];

bool dfs(int x) {
	va[x] = 1; // 访问标记：x在交错树中
	for (int y = 1; y <= n; y++)
		if (!vb[y])
			if (la[x] + lb[y] - w[x][y] == 0) { // 相等子图
				vb[y] = 1; // 访问标记：y在交错树中
				if (!match[y] || dfs(match[y])) {
					match[y] = x;
					return true;
				}
			}
			else upd[y] = min(upd[y], la[x] + lb[y] - w[x][y]);
	return false;
}

int KM() {
	for (int i = 1; i <= n; i++) {
		la[i] = -(1 << 30); // -inf
		lb[i] = 0;
		for (int j = 1; j <= n; j++)
			la[i] = max(la[i], w[i][j]);
	}
	for (int i = 1; i <= n; i++)
		while (true) { // 直到左部点找到匹配
			memset(va, 0, sizeof(va));
			memset(vb, 0, sizeof(vb));
			delta = 1 << 30; // inf
			for (int j = 1; j <= n; j++) upd[j] = 1 << 30; 
			if (dfs(i)) break;
			for (int j = 1; j <= n; j++)
				if (!vb[j]) delta = min(delta, upd[j]);
			for (int j = 1; j <= n; j++) { // 修改顶标
				if (va[j]) la[j] -= delta;
				if (vb[j]) lb[j] += delta;
			}
		}
	int ans = 0;
	for (int i = 1; i <= n; i++)
		ans += w[match[i]][i];
	return ans;
}
// 最大流，Edmonds-Karp增广路算法
#include<iostream>
#include<cstdio>
#include<cstring>
#include<algorithm>
#include<vector>
#include<queue>
using namespace std;
const int inf = 1 << 29, N = 2010, M = 20010;
int head[N], ver[M], edge[M], Next[M], v[N], incf[N], pre[N];
int n, m, s, t, tot, maxflow;

void add(int x, int y, int z) {
	ver[++tot] = y, edge[tot] = z, Next[tot] = head[x], head[x] = tot;
	ver[++tot] = x, edge[tot] = 0, Next[tot] = head[y], head[y] = tot;
}

bool bfs() {
	memset(v, 0, sizeof(v));
	queue<int> q;
	q.push(s); v[s] = 1;
	incf[s] = inf; // 增广路上各边的最小剩余容量
	while (q.size()) {
		int x = q.front(); q.pop();
		for (int i = head[x]; i; i = Next[i])
			if (edge[i]) {
				int y = ver[i];
				if (v[y]) continue;
				incf[y] = min(incf[x], edge[i]);
				pre[y] = i; // 记录前驱，便于找到最长路的实际方案
				q.push(y), v[y] = 1;
				if (y == t) return 1;
			}
	}
	return 0;
}

void update() { // 更新增广路及其反向边的剩余容量
	int x = t;
	while (x != s) {
		int i = pre[x];
		edge[i] -= incf[t];
		edge[i ^ 1] += incf[t]; // 利用“成对存储”的xor 1技巧
		x = ver[i ^ 1];
	}
	maxflow += incf[t];
}

int main() {
	while (cin >> m >> n) {
		memset(head, 0, sizeof(head));
		s = 1, t = n;
		tot = 1; maxflow = 0;
		for (int i = 1; i <= m; i++) {
			int x, y, c;
			scanf("%d%d%d", &x, &y, &c);
			add(x, y, c);
		}
		while (bfs()) update();
		cout << maxflow << endl;
	}
}


// 最大流，Dinic算法
#include<iostream>
#include<cstdio>
#include<cstring>
#include<algorithm>
#include<vector>
#include<queue>
using namespace std;
const int inf = 1 << 29, N = 50010, M = 300010;
int head[N], ver[M], edge[M], Next[M], d[N];
int n, m, s, t, tot, maxflow;
queue<int> q;

void add(int x, int y, int z) {
	ver[++tot] = y, edge[tot] = z, Next[tot] = head[x], head[x] = tot;
	ver[++tot] = x, edge[tot] = 0, Next[tot] = head[y], head[y] = tot;
}

bool bfs() { // 在残量网络上构造分层图
	memset(d, 0, sizeof(d));
	while (q.size()) q.pop();
	q.push(s); d[s] = 1;
	while (q.size()) {
		int x = q.front(); q.pop();
		for (int i = head[x]; i; i = Next[i])
			if (edge[i] && !d[ver[i]]) {
				q.push(ver[i]);
				d[ver[i]] = d[x] + 1;
				if (ver[i] == t) return 1;
			}
	}
	return 0;
}

int dinic(int x, int flow) { // 在当前分层图上增广
	if (x == t) return flow;
	int rest = flow, k;
	for (int i = head[x]; i && rest; i = Next[i])
		if (edge[i] && d[ver[i]] == d[x] + 1) {
			k = dinic(ver[i], min(rest, edge[i]));
			if (!k) d[ver[i]] = 0; // 剪枝，去掉增广完毕的点
			edge[i] -= k;
			edge[i ^ 1] += k;
			rest -= k;
		}
	return flow - rest;
}

int main() {
	cin >> n >> m;
	cin >> s >> t; // 源点、汇点
	tot = 1;
	for (int i = 1; i <= m; i++) {
		int x, y, c;
		scanf("%d%d%d", &x, &y, &c);
		add(x, y, c);
	}
	int flow = 0;
	while (bfs())
		while (flow = dinic(s, inf)) maxflow += flow;
	cout << maxflow << endl;
}


// 费用流，例题：K取方格数 (POJ3422)
#include<iostream>
#include<cstdio>
#include<cstring>
#include<algorithm>
#include<queue>
using namespace std;
const int N = 5010, M = 200010;
int ver[M], edge[M], cost[M], Next[M], head[N];
int d[N], incf[N], pre[N], v[N];
int n, k, tot, s, t, maxflow, ans;

void add(int x, int y, int z, int c) {
	// 正向边，初始容量z，单位费用c
	ver[++tot] = y, edge[tot] = z, cost[tot] = c;
	Next[tot] = head[x], head[x] = tot;
	// 反向边，初始容量0，单位费用-c，与正向边“成对存储”
	ver[++tot] = x, edge[tot] = 0, cost[tot] = -c;
	Next[tot] = head[y], head[y] = tot;
}

int num(int i, int j, int k) {
	return (i - 1)*n + j + k*n*n;
}

bool spfa() {
	queue<int> q;
	memset(d, 0xcf, sizeof(d)); // -INF
	memset(v, 0, sizeof(v));
	q.push(s); d[s] = 0; v[s] = 1; // SPFA 求最长路
	incf[s] = 1 << 30; // 增广路上各边的最小剩余容量
	while (q.size()) {
		int x = q.front(); v[x] = 0; q.pop();
		for (int i = head[x]; i; i = Next[i]) {
			if (!edge[i]) continue; // 剩余容量为0，不在残量网络中，不遍历
			int y = ver[i];
			if (d[y]<d[x] + cost[i]) {
				d[y] = d[x] + cost[i];
				incf[y] = min(incf[x], edge[i]);
				pre[y] = i; // 记录前驱，便于找到最长路的实际方案
				if (!v[y]) v[y] = 1, q.push(y);
			}
		}
	}
	if (d[t] == 0xcfcfcfcf) return false; // 汇点不可达，已求出最大流
	return true;
}

// 更新最长增广路及其反向边的剩余容量
void update() {
	int x = t;
	while (x != s) {
		int i = pre[x];
		edge[i] -= incf[t];
		edge[i ^ 1] += incf[t]; // 利用“成对存储”的xor 1技巧
		x = ver[i ^ 1];
	}
	maxflow += incf[t];
	ans += d[t] * incf[t];
}

int main() {
	cin >> n >> k;
	s = 1, t = 2 * n * n;
	tot = 1; // 一会儿要从2开始“成对存储”，2和3是一对，4和5是一对
	for (int i = 1; i <= n; i++)
		for (int j = 1; j <= n; j++) {
			int c; scanf("%d", &c);
			add(num(i, j, 0), num(i, j, 1), 1, c);
			add(num(i, j, 0), num(i, j, 1), k - 1, 0);
			if (j<n) add(num(i, j, 1), num(i, j + 1, 0), k, 0);
			if (i<n) add(num(i, j, 1), num(i + 1, j, 0), k, 0);
		}
	while (spfa()) update(); // 计算最大费用最大流
	cout << ans << endl;
}
// 随机数据生成模板
#include<cstdlib>
#include<ctime>

int random(int n) {
    return (long long)rand() * rand() % n;
}

int main() {
    srand((unsigned)time(0));
    // ...具体内容...
}

// 实例：随机生成整数序列
// 不超过100000个绝对值在1000000000内的整数
int n = random(100000) + 1;
int m = 1000000000;
for (int i = 1; i <= n; i++) {
    a[i] = random(2 * m + 1) - m;
}

// 实例：随机生成区间列
for (int i = 1; i <= m; i++) {
    int l = random(n) + 1;
    int r = random(n) + 1;
    if (l > r) swap(l, r);
    printf("%d %d\n", l, r);
}

// 实例：随机生成树
for (int i = 2; i <= n; i++) {
    // 从 2~n 之间的每个点 i 向 1~i-1 之间的点随机连一条边
    int fa = random(i - 1) + 1;
    int val = random(1000000000) + 1;
    printf("%d %d %d\n", fa, i, val);
}

// 实例：随机生成图
// 无向图，连通，不含重边、自环
pair<int, int> e[1000005]; // 保存数据
map< pair<int, int>, bool > h; // 防止重边
// 先生成一棵树，保证连通
for (int i = 1; i < n; i++) {
    int fa = random(i) + 1;
    e[i] = make_pair(fa, i + 1);
    h[e[i]] = h[make_pair(i + 1, fa)] = 1;
}
// 再生成剩余的 m-n+1 条边
for (int i = n; i <= m; i++) {
    int x, y;
    do {
        x = random(n) + 1, y = random(n) + 1;
    } while (x == y || h[make_pair(x, y)]);
    e[i] = make_pair(x, y);
    h[e[i]] = h[make_pair(y, x)] = 1;
}
// 随机打乱，输出
random_shuffle(e + 1, e + m + 1);
for (int i = 1; i <= m; i++)
    printf("%d %d\n", e[i].first, e[i].second);


// Windows系统对拍程序
#include<cstdlib>
#include<cstdio>
#include<ctime>
int main() {
    for (int T = 1; T <= 10000; T++) {
        // 自行设定适当的路径
        system("C:\\random.exe");
        // 返回当前程序已经运行的CPU时间，windows下单位ms，类unix下单位s
        double st = clock();
        system("C:\\sol.exe");
        double ed = clock();
        system("C:\\bf.exe");
        if (system("fc C:\\data.out C:\\data.ans")) {
            puts("Wrong Answer");
            // 程序立即退出，此时data.in即为发生错误的数据，可人工演算、调试
            return 0;
        }
        else {
            printf("Accepted, 测试点 #%d, 用时 %.0lfms\n", T, ed - st);
        }
    }
}




```
