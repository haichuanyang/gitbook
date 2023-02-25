# dancing links

```
AcWing 1067. 精确覆盖问题 舞蹈链解法 Dancing links v2  


题目描述
算法1
自写指针版本 DLX
舞蹈链 DLX解析 可看此链接
这里是自己整理重新画图的V2版本 DLX学习心得

C++ 代码
// DLX_2.cpp : 此文件包含 "main" 函数。程序执行将在此处开始并结束。
//

#include <iostream>
#include <vector>
#include <assert.h>

using namespace std;

const int N = 510;


/*
3 3
0 1 0
0 0 1
1 0 0

1 2 3

4 4
0 0 0 1
1 0 0 0
1 1 0 1
0 1 0 0

No Solution!
*/


struct DLX_NODE {
    struct DLX_NODE* up;
    struct DLX_NODE* down;
    struct DLX_NODE* left;
    struct DLX_NODE* right;
    int row; int col;
    int count;
    int ele;

    DLX_NODE() {
        up = NULL; down = NULL; left = NULL; right = NULL;
        col = -1; row = -1; ele = -1; count = 0;
    }
};

struct DLX_NODE table[N][N];

int n, m;

void init()
{
    for (int i = 0; i < n; i++) {
        table[i][0].down = &table[i + 1][0];
        table[i + 1][0].up = &table[i][0];
        table[i][0].row = i; table[i][0].col = 0;
        table[i + 1][0].row = i + 1; table[i + 1][0].col = 0;
    }

    for (int i = 0; i < m; i++) {
        table[0][i].right = &table[0][i + 1];
        table[0][i + 1].left = &table[0][i];
        table[0][i].row = 0; table[0][i].col = i;
        table[0][i+1].row = 0; table[0][i+1].col = i+1;
    }


}

void printDLX()
{
    struct DLX_NODE* p = &table[0][0];
    cout << "=================================================>\n";
    p = p->right;
    while (p != NULL) {
        struct DLX_NODE* pp = p;
        if (pp->count != 0) {
            cout << "colBase.count = " << pp->count << endl;
        }
        while (pp != NULL) {
            if (pp->ele == 1)
                cout << "pp->col = " << pp->col << ". pp->row = " << pp->row << ".pp->ele = " << pp->ele << endl;
            pp = pp->down;
        }
        p = p->right;
    }
    cout << endl << endl;
    p = &table[0][0];
    p = p->down;
    while (p != NULL) {
        struct DLX_NODE* pp = p;
        while (pp != NULL) {
            if (pp->ele == 1)
                cout << "pp->col = " << pp->col << ". pp>row = " << pp->row << ".pp->ele = " << pp->ele << endl;
            pp = pp->right;
        }
        p = p->down;
    }
    cout << endl ;
}



void FindColInsert(int row,int col)
{
    struct DLX_NODE* p = &table[0][col];
    while (p != NULL) {
        struct DLX_NODE* next = p->down;
        //找到链表结尾或者 next比当前元素行数更大 P的下面即是插入的位置
        if (next == NULL)  break;
        else if (next->row > row) break;
        else if (next->row == row) {
            //行列都相同则说明重复插入了
            assert(0);
        }
        p = next;
    }
    assert(p != NULL);
    //插入
    struct DLX_NODE* node = &table[row][col];
    node->down = p->down;
    if (p->down != NULL) p->down->up = node;

    node->up = p;
    p->down = node;

    table[0][col].count++;
}


void FindRowInsert(int row, int col)
{
    struct DLX_NODE* p = &table[row][0];
    while (p != NULL) {
        struct DLX_NODE* next = p->right;
        //找到链表结尾或者 next比当前元素列数更大 P的右面即是插入的位置
        if (next == NULL)  break;
        else if (next->col > col) break;
        else if (next->col == col) {
            //行列都相同则说明重复插入了
            assert(0);
        }
        p = next;
    }
    assert(p != NULL);
    //插入
    struct DLX_NODE* node = &table[row][col];
    node->right = p->right;
    if (p->right != NULL) p->right->left = node;

    node->left = p;
    p->right = node;
}

void addNode(int num, int x, int y)
{
    table[x][y].row = x;
    table[x][y].col = y;
    table[x][y].ele = num;

    //插入列中
    FindColInsert(x,y);

    //插入行中
    FindRowInsert(x,y);
}

void resume(struct DLX_NODE* pToResume)
{
    struct DLX_NODE* pdown = pToResume->down;
    while (pdown != NULL) {
        int row = pdown->row;
        assert(row != -1);
        struct DLX_NODE* nextResume = &table[row][0];
        while (nextResume != NULL) {
            if (nextResume->col > 0)
                table[0][nextResume->col].count++;
            nextResume->up->down = nextResume;
            if (nextResume->down != NULL) nextResume->down->up = nextResume;
            nextResume = nextResume->right;
        }
        pdown = pdown->down;
    }


    if (pToResume->right != NULL) pToResume->right->left = pToResume;
    pToResume->left->right = pToResume;


}

void remove(struct DLX_NODE* pToRemove)
{
    if (pToRemove->right != NULL) { pToRemove->right->left = pToRemove->left; }
    pToRemove->left->right = pToRemove->right;

    struct DLX_NODE* pdown = pToRemove->down;
    while (pdown != NULL) {
        int row = pdown->row;
        assert(row != -1);
        struct DLX_NODE* nextRemove = &table[row][0];
        while (nextRemove != NULL) {
            if(nextRemove->col != 0)
                table[0][nextRemove->col].count--;
            assert(table[0][nextRemove->col].count >= 0);
            if (nextRemove->col == pToRemove->col) {
                nextRemove = nextRemove->right;
                continue;
            }
            if (nextRemove->down != NULL) nextRemove->down->up = nextRemove->up;
            nextRemove->up->down = nextRemove->down;
            nextRemove = nextRemove->right;
        }
        pdown = pdown->down;
    }
}

vector<int> ans;
bool dfs()
{
    int noEle = 1;  int minCount = 99999; struct DLX_NODE* selectP = NULL;
    struct DLX_NODE* p = table[0][0].right;
    if (p == NULL) 
        return true;
    while (p != NULL) {
        if (p->count != -1 && p->count < minCount) {
            minCount = p->count;
            selectP = p;
        }
        p = p->right;
    }


    remove(selectP);
    {
        p = selectP->down;
        while (p != NULL) {
            ans.push_back(p->row);
            struct DLX_NODE* tmp = table[p->row][0].right;
            while (tmp != NULL) {
                if(tmp->col != selectP->col)
                    remove(&table[0][tmp->col]);
                tmp = tmp->right;
            }
            if (dfs()) return true;

            tmp = table[p->row][0].right;
            while (tmp != NULL) {
                if (tmp->col != selectP->col)
                    resume(&table[0][tmp->col]);
                tmp = tmp->right;
            }
            ans.pop_back();
            p = p->down;
        }
    }

    resume(selectP);

    return false;
}



int main()
{
    cin >> n >> m;
    init();
    for (int i = 1; i <= n; i++) {
        for (int j = 1; j <= m; j++) {
            int x; cin >> x;
            if (x) {
                addNode(x, i, j);
            }
        }
    }
    //printDLX();
    if (dfs())
    {
        for(auto& e:ans)  
            printf("%d ", e);
        puts("");
    }
    else puts("No Solution!");



    return 0;
}


实际使用建议使用Y总模板
使用了链式前向星的十字链表DLX

#include <iostream>
#include <cstring>
#include <algorithm>

using namespace std;

const int N = 5510;

int n, m;
int l[N], r[N], u[N], d[N], s[N], row[N], col[N], idx;
int ans[N], top;

void init()
{
    for (int i = 0; i <= m; i ++ )
    {
        l[i] = i - 1, r[i] = i + 1;
        u[i] = d[i] = i;
    }
    l[0] = m, r[m] = 0;
    idx = m + 1;
}

void add(int& hh, int& tt, int x, int y)
{
    row[idx] = x, col[idx] = y, s[y] ++ ;
    u[idx] = y, d[idx] = d[y], u[d[y]] = idx, d[y] = idx;
    r[hh] = l[tt] = idx, r[idx] = tt, l[idx] = hh;
    tt = idx ++ ;
}

void remove(int p)
{
    r[l[p]] = r[p], l[r[p]] = l[p];
    for (int i = d[p]; i != p; i = d[i])
        for (int j = r[i]; j != i; j = r[j])
        {
            s[col[j]] -- ;
            u[d[j]] = u[j], d[u[j]] = d[j];
        }
}

void resume(int p)
{
    for (int i = u[p]; i != p; i = u[i])
        for (int j = l[i]; j != i; j = l[j])
        {
            u[d[j]] = j, d[u[j]] = j;
            s[col[j]] ++ ;
        }
    r[l[p]] = p, l[r[p]] = p;
}

bool dfs()
{
    if (!r[0]) return true;
    int p = r[0];
    for (int i = r[0]; i; i = r[i])
        if (s[i] < s[p])
            p = i;
    remove(p);
    for (int i = d[p]; i != p; i = d[i])
    {
        ans[ ++ top] = row[i];
        for (int j = r[i]; j != i; j = r[j]) remove(col[j]);
        if (dfs()) return true;
        for (int j = l[i]; j != i; j = l[j]) resume(col[j]);
        top -- ;
    }
    resume(p);
    return false;
}

int main()
{
    scanf("%d%d", &n, &m);
    init();
    for (int i = 1; i <= n; i ++ )
    {
        int hh = idx, tt = idx;
        for (int j = 1; j <= m; j ++ )
        {
            int x;
            scanf("%d", &x);
            if (x) add(hh, tt, i, j);
        }
    }

    if (dfs())
    {
        for (int i = 1; i <= top; i ++ ) printf("%d ", ans[i]);
        puts("");
    }
    else puts("No Solution!");

    return 0;
}


作者：itdef
链接：https://www.acwing.com/solution/content/26693/

```
