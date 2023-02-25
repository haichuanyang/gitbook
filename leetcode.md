---
description: weekly
---

# leetcode

[Biweekly Contest 44](https://leetcode.com/contest/biweekly-contest-44)  acw-yxc 57 rank

if a^b=c, then a=b^c; proof: apply ^b on both sides to get a^b^b=c^b, b^b=0, a^0=a, so a =c^b

```
//Biweekly Contest 44  
class Solution {
public:
    int largestAltitude(vector<int>& gain) {
        int res = 0, h = 0;
        for (auto x: gain) {
            h += x;
            res = max(res, h);
        }
        return res;
    }
};
class Solution {
public:
    int minimumTeachings(int n, vector<vector<int>>& lg, vector<vector<int>>& fs) {
        int m = lg.size();
        vector<vector<bool>> g(m + 1, vector<bool>(n + 1));
        for (int i = 0; i < lg.size(); i ++ ) {
            for (auto x: lg[i]) {
                g[i + 1][x] = true;
            }
        }
        
        set<int> ps;
        vector<int> cnt(n + 1);
        for (auto& f: fs) {
            int a = f[0], b = f[1];
            bool flag = false;
            for (int i = 1; i <= n; i ++ )
                if (g[a][i] && g[b][i]) {
                    flag = true;
                    break;
                }
            if (flag) continue;
            ps.insert(a), ps.insert(b);
        }
        
        int res = 0;
        for (auto x: ps) {
            for (int i = 1; i <= n; i ++ ) {
                if (g[x][i]) {
                    cnt[i] ++ ;
                    res = max(res, cnt[i]);
                }
            }
        }
        return ps.size() - res;
    }
};


int son[2100000][2];

class Solution {
public:
    int idx;
    
    void insert(int x) {
        int p = 0;
        for (int i = 20; i >= 0; i -- ) {
            int u = x >> i & 1;
            if (!son[p][u]) son[p][u] = ++ idx;
            p = son[p][u];
        }
    }
    
    int query(int x) {
        int res = 0, p = 0;
        for (int i = 20; i >= 0; i -- ) {
            int u = x >> i & 1;
            if (son[p][!u]) p = son[p][!u], res = res * 2 + 1;
            else p = son[p][u], res *= 2;
        }
        return res;
    }
    
    vector<int> decode(vector<int>& b) {
        int n = b.size() + 1;
        idx = 0;
        memset(son, 0, sizeof son);
        for (int i = 1; i < b.size(); i ++ ) b[i] ^= b[i - 1];
        unordered_set<int> hash;
        for (auto x: b) hash.insert(x), insert(x);
        
        vector<int> res;
        for (int i = 1; i <= n; i ++ ) {
            if (!hash.count(i)) {
                if (query(i) <= n) {
                    res.push_back(i);
                    for (int j = 0; j < b.size(); j ++ )
                        res.push_back(i ^ b[j]);
                    break;
                }
            }
        }
        
        return res;
    }
};

//Code
typedef long long LL;
const int N = 10010;

class Solution {
public:
    int mod = 1e9 + 7;
    int f[N], g[N];
    vector<int> ds;
    
    int qmi(int a, int b) {
        int res = 1;
        while (b) {
            if (b & 1) res = (LL)res * a % mod;
            a = (LL)a * a % mod;
            b >>= 1;
        }
        return res;
    }
    
    int dfs(int n, int m, int p, int u) {
        int res = 0;
        if (m == 1) res = (LL)p * g[n] % mod;
        if (u == ds.size() || m < ds[u]) return res;
        
        int d = ds[u];
        res = (res + dfs(n, m, p, u + 1)) % mod;
        for (int i = 1; i <= n; i ++ ) {
            if (m % d == 0) {
                m /= d;
                res = (res + dfs(n - i, m, (LL)p * g[i] % mod, u + 1)) % mod;
            } else {
                break;
            }
        }
        return res;
    }
    
    vector<int> waysToFillArray(vector<vector<int>>& queries) {
        f[0] = g[0] = 1;
        for (int i = 1; i < N; i ++ ) {
            f[i] = (LL)f[i - 1] * i % mod;
            g[i] = qmi(f[i], mod - 2);
        }
        
        vector<int> res;
        for (auto& q: queries) {
            int n = q[0], m = q[1];
            ds.clear();
            for (int i = 1; i * i <= m; i ++ ) {
                if (m % i == 0) {
                    ds.push_back(i);
                    if (i != m / i) ds.push_back(m / i);
                }
            }
            sort(ds.begin(), ds.end());
            res.push_back(dfs(n, m, f[n], 1));
        }
        
        return res;
    }
};

//cuiaoxiang
class Solution {
public:
    int largestAltitude(vector<int>& a) {
        int cur = 0, best = 0;
        for (auto& x : a) {
            cur += x;
            best = max(best, cur);
        }
        return best;
    }
};

const int N = 500;
class Solution {
public:
    int minimumTeachings(int n, vector<vector<int>>& a, vector<vector<int>>& e) {
        int m = a.size();
        vector<bitset<N>> skill(m);
        for (int i = 0; i < m; ++i) {
            for (auto& x : a[i]) {
                skill[i][x - 1] = 1;
            }
        }
        vector<bool> flag(e.size());
        for (int i = 0; i < (int)e.size(); ++i) {
            auto& v = e[i];
            int x = v[0] - 1, y = v[1] - 1;
            if ((skill[x] & skill[y]).count() == 0) flag[i] = true;
        }
        int ret = 1e9;
        for (int k = 0; k < n; ++k) {
            vector<bool> mark(m);
            for (int i = 0; i < (int)e.size(); ++i) {
                if (flag[i]) {
                    int x = e[i][0] - 1, y = e[i][1] - 1;
                    if (!skill[x][k]) mark[x] = true;
                    if (!skill[y][k]) mark[y] = true;
                }
            }
            int cur = 0;
            for (int i = 0; i < m; ++i) cur += mark[i];
            ret = min(ret, cur);
        }
        return ret;
    }
};

class Solution {
public:
    vector<int> decode(vector<int>& a) {
        int n = a.size() + 1;
        int sum = 0;
        for (int i = 1; i <= n; ++i) sum ^= i;
        for (int i = n - 2; i >= 0; i -= 2) sum ^= a[i];
        vector<int> ret(n);
        ret[0] = sum;
        for (int i = 0; i < n - 1; ++i) ret[i + 1] = ret[i] ^ a[i];
        return ret;
    }
};


const int MOD = 1e9 + 7;
typedef long long int64;

const int N = 1e5 + 10;
int fact[N], ifact[N], inv[N];

struct comb_init {
  comb_init() {
    inv[1] = 1;
    for (int i = 2; i < N; ++i) {
      inv[i] = (MOD - MOD / i) * (int64)inv[MOD % i] % MOD;
    }
    fact[0] = ifact[0] = 1;
    for (int i = 1; i < N; ++i) {
      fact[i] = (int64)fact[i - 1] * i % MOD;
      ifact[i] = (int64)ifact[i - 1] * inv[i] % MOD;
    }
  }
} comb_init_;

int64 comb(int n, int m) {
  if (n < m || m < 0) return 0;
  return (int64)fact[n] * ifact[m] % MOD * ifact[n - m] % MOD;
}

class Solution {
public:
    vector<int> factorize(int m) {
        vector<int> ret;
        for (int i = 2; i * i <= m; ++i) {
            if (m % i == 0) {
                int cnt = 0;
                while (m % i == 0) {
                    m /= i;
                    ++cnt;
                }
                ret.push_back(cnt);
            }
        }
        if (m > 1) ret.push_back(1);
        return ret;
    }
    vector<int> waysToFillArray(vector<vector<int>>& q) {
        vector<int> ret;
        for (auto& v : q) {
            int n = v[0], m = v[1];
            auto f = factorize(m);
            int cur = 1;
            for (auto& x : f) {
                cur = cur * comb(x + n - 1, n - 1) % MOD;
            }
            ret.push_back(cur);
        }
        return ret;
    }
};
```





dfs
