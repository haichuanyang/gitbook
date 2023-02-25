---
description: 选择比努力重要，和谁去比去哪儿更重要！
---

# notes

[https://mindyourdecisions.com/blog/2020/06/30/a-magical-triangle-think-outside-the-box/](https://mindyourdecisions.com/blog/2020/06/30/a-magical-triangle-think-outside-the-box/) think outside the box

[https://mp.weixin.qq.com/s/0UBhD3\_F8V5B-mebE8Wc4w](https://mp.weixin.qq.com/s/0UBhD3\_F8V5B-mebE8Wc4w) byd&#x20;



binary search, two pointer, prefix sum, depth first search, breadth first search

AOV - activity on vertex; AOE -activity on edge

c++ has list allows O(1) insert time at any position

{% embed url="https://www.geeksforgeeks.org/difference-between-vector-and-list/" %}

[https://users.cs.northwestern.edu/\~riesbeck/programming/c++/stl-summary.html](https://users.cs.northwestern.edu/\~riesbeck/programming/c++/stl-summary.html)





[https://github.com/zhedahht](https://github.com/zhedahht) 《剑指Offer》第二版源代码 oeis.org



{% embed url="http://e-maxx.ru/bookz/files/dsu/Efficiency%20of%20a%20Good%20But%20Not%20Linear%20Set%20Union%20Algorithm.%20Tarjan.pdf" %}

## Tarjan - Efficiency of a Good But Not Linear Set Union Algorithm





{% embed url="https://leopoldacc.github.io/Blogs/2020/12/05/%E5%8D%A2%E5%8D%A1%E6%96%AF%E5%AE%9A%E7%90%86/" %}

[https://leopoldacc.github.io/Blogs/2020/12/04/%E5%8D%95%E8%B0%83%E6%A0%88/](https://leopoldacc.github.io/Blogs/2020/12/04/%E5%8D%95%E8%B0%83%E6%A0%88/)

{% embed url="https://www.acwing.com/file_system/file/content/whole/index/content/1507452/" %}

[https://www.acwing.com/user/myspace/index/25/](https://www.acwing.com/user/myspace/index/25/)

某条河长S，水流速为v1，船速为v2。船从下游终点去装上游起点漂流下来的货物，装到货物后再开回下游终点，使用的时间可以证明得到是2S/（v1+v2），请问怎么直观理解这个答案？为什么这个模型就相当于两车相向在平地相遇的问题啊？







```
#pragma once
#pragma GCC diagnostic error "-std=c++11"
#pragma GCC optimize(3,"Ofast","inline")
#pragma GCC optimize(2)
#pragma GCC target("avx")
#pragma GCC optimize("Ofast")
#pragma GCC optimize("inline")
#pragma GCC optimize("-fgcse")
#pragma GCC optimize("-fgcse-lm")
#pragma GCC optimize("-fipa-sra")
#pragma GCC optimize("-ftree-pre")
#pragma GCC optimize("-ftree-vrp")
#pragma GCC optimize("-fpeephole2")
#pragma GCC optimize("-ffast-math")
#pragma GCC optimize("-fsched-spec")
#pragma GCC optimize("unroll-loops")
#pragma GCC optimize("-falign-jumps")
#pragma GCC optimize("-falign-loops")
#pragma GCC optimize("-falign-labels")
#pragma GCC optimize("-fdevirtualize")
#pragma GCC optimize("-fcaller-saves")
#pragma GCC optimize("-fcrossjumping")
#pragma GCC optimize("-fthread-jumps")
#pragma GCC optimize("-funroll-loops")
#pragma GCC optimize("-fwhole-program")
#pragma GCC optimize("-freorder-blocks")
#pragma GCC optimize("-fschedule-insns")
#pragma GCC optimize("inline-functions")
#pragma GCC optimize("-ftree-tail-merge")
#pragma GCC optimize("-fschedule-insns2")
#pragma GCC optimize("-fstrict-aliasing")
#pragma GCC optimize("-fstrict-overflow")
#pragma GCC optimize("-falign-functions")
#pragma GCC optimize("-fcse-skip-blocks")
#pragma GCC optimize("-fcse-follow-jumps")
#pragma GCC optimize("-fsched-interblock")
#pragma GCC optimize("-fpartial-inlining")
#pragma GCC optimize("no-stack-protector")
#pragma GCC optimize("-freorder-functions")
#pragma GCC optimize("-findirect-inlining")
#pragma GCC optimize("-fhoist-adjacent-loads")
#pragma GCC optimize("-frerun-cse-after-loop")
#pragma GCC optimize("inline-small-functions")
#pragma GCC optimize("-finline-small-functions")
#pragma GCC optimize("-ftree-switch-conversion")
#pragma GCC optimize("-foptimize-sibling-calls")
#pragma GCC optimize("-fexpensive-optimizations")
#pragma GCC optimize("-funsafe-loop-optimizations")
#pragma GCC optimize("inline-functions-called-once")
#pragma GCC optimize("-fdelete-null-pointer-checks")
#include <iostream>
using namespace std;
int main() {

    return 0;
}
```

```
余数的和等于和的余数：a%p+b%p=(a+b)%p
余数的差等于差的余数：a%p-b%p=(a-b)%p
余数的积等于积的余数： a%p * b%p=(a*b)%p

意义在于当一个数A过大时，可以利用和差积把A拆分为几个数字分别求余，再组合到一起。

乘法逆元：
看到上面的几个推理，是否想商是否也用同样的事情呢？遗憾的是，没有。因为：12/3 % 7 = 4但12%7=5,3%7=3,(5/3)%7遇到5/3不能得整数的尴尬局面。
为了搞定这个问题，想办法找到等式成立的x,3x % 7 = 1,这样等式(5/3 * 3x)%7 = (5/3)%7，因为乘1值不变。
乘3再除以3可以消去，得到5x%7 = (5/3)%7，ok，除法变乘法，x就是5在模7意义下的乘法逆元。
肉眼观察x=5时，3*5%7=1成立，故(5/3)%7=(5*5)%7=4，看到没，再回到最初的问题上，12/3=4,4%7=4等价于12/3%7=>12%7,3%7,5/3%7,5*5%7=4

```

According to \[12], this algorithm is named after Mo Tao, a Chinese competitive programmer, but the technique has appeared earlier in the literature \[44].

{% embed url="http://codeforces.com/blog/entry/20032" %}

\[43] P. W. Kasteleyn. The statistics of dimers on a lattice: I. The number of dimer arrangements on a quadratic lattice. Physica, 27(12):1209–1225, 1961.

\[44] C. Kent, G. M. Landau and M. Ziv-Ukelson. On the complexity of sparse exon assembly. Journal of Computational Biology, 13(5):1013–1027, 2006.

\[45] J. Kleinberg and É. Tardos. Algorithm Design, Pearson, 2005.\




LeetCode 402. 移掉K位数字

```
class Solution {
public:
    string removeKdigits(string num, int k) {
        //单调递增栈
        vector<int> stk(num.size());
        int top = -1;//栈顶
        if(num.size()==k) return "0";
        string ans;
        for(int i = 0; i < num.size(); i++){
            while(top!=-1&&stk[top]>num[i]&&k) top--,k--;//高位可换成更小的时，则去替换高位值，最多k次
            stk[++top] = num[i];
        }
        while(k--) top--;//若还没删够，则删除末尾的k个数
        int i = 0;
        while(stk[i]=='0'&&i < top) i++;//去除前导零
        while(i <= top) ans.push_back(stk[i++]);
        return ans;

    }
}
```

[https://zhuanlan.zhihu.com/p/181361741](https://zhuanlan.zhihu.com/p/181361741) 胡光胡船长

[https://zhuanlan.zhihu.com/p/182642052](https://zhuanlan.zhihu.com/p/182642052)海贼科技的创始人：胡光，大学期间就读哈尔滨工程大学，在校期间，代表学校参加竞赛，并斩获2008-2011年各届省赛、东北赛一等奖，ACM亚洲区金牌，曾两次代表哈尔滨工程大学参加ACM全球总决赛。

string.size() same as length()

deque is enhanced vector

Intro to probability Jessica Hwang of stanford

[https://drive.google.com/file/d/1VmkAAGOYCTORq1wxSQqy255qLJjTNvBI/view](https://drive.google.com/file/d/1VmkAAGOYCTORq1wxSQqy255qLJjTNvBI/view)

A free online version of the second edition of the book based on Stat 110, _Introduction to Probability_ by Joe Blitzstein and Jessica Hwang, is now available at [http://probabilitybook.net](http://probabilitybook.net/)

{% embed url="https://projects.iq.harvard.edu/stat110" %}

hoeffding not mentioned any textbook on probability

[http://cs229.stanford.edu/extra-notes/hoeffding.pdf](http://cs229.stanford.edu/extra-notes/hoeffding.pdf)

probability/statistics textbook which covers modern theories - markov and bayes stanford chinese second author only on the other laptop - google backup and sync not working!

Matrix cookbook - peterson

神经网络与深度学习》 Neural Networks and Deep Learning fudan https://nndl.github.io/

深度学习花书 deep learning 圣经 张志华： deep learning bible [http://www.deeplearningbook.org/](http://www.deeplearningbook.org/) 中文版 [https://github.com/exacity/deeplearningbook-chinese](https://github.com/exacity/deeplearningbook-chinese)

李航《统计学习方法》: 感K朴决逻， 支提E隐条

机器学习周志华 西瓜书

Christopher Bishop的 Pattern Recognition and Machine Learning ： 回分神核稀， 图混近采连，顺组

Murphy/Machine\_Learning-\_A\_Probabilistic\_Perspective.pdf 百科全书

Competitive Programmer’s Handbook by Antti Laaksonen

CP 3 book - singapore univ\


C++与数据结构 BIT Univ.pdf

2005冬令营(刘汝佳) too old stuff - not up to date《概率论与数理统计》- 陈希孺 ustc textbook, didn't understand at that time, can't use now, can only understand with English version, such as regression

cartesian and polar coordinate systems all have an origin point - what if there is no origin point? what can you do to establish a reference system when there is no reference point?



**Eulerian circuit** traverses every edge **in a** graph exactly once, but may repeat vertices, while a **Hamiltonian circuit** visits each vertex **in a** graph exactly once but may repeat edges.

{% embed url="https://brilliant.org/wiki/bijection-injection-and-surjection/?quiz=bijection-injection-and-surjection-definition#" %}

injection has each output mapped to by at most one input - one to one

surjection includes the entire possible range in the output - onto

{% embed url="https://www.mathsisfun.com/sets/injective-surjective-bijective.html" %}



[https://zh.wikipedia.org/wiki/%E5%8D%95%E5%B0%84%E3%80%81%E5%8F%8C%E5%B0%84%E4%B8%8E%E6%BB%A1%E5%B0%84](https://zh.wikipedia.org/wiki/%E5%8D%95%E5%B0%84%E3%80%81%E5%8F%8C%E5%B0%84%E4%B8%8E%E6%BB%A1%E5%B0%84)

[https://math.libretexts.org/Bookshelves/Mathematical\_Logic\_and\_Proof/Book%3A\_Mathematical\_Reasoning\_\_Writing\_and\_Proof\_(Sundstrom)/6%3A\_Functions/6.3%3A\_Injections%2C\_Surjections%2C\_and\_Bijections](https://math.libretexts.org/Bookshelves/Mathematical\_Logic\_and\_Proof/Book%3A\_Mathematical\_Reasoning\_\_Writing\_and\_Proof\_\(Sundstrom\)/6%3A\_Functions/6.3%3A\_Injections%2C\_Surjections%2C\_and\_Bijections)





{% embed url="https://proofwiki.org/wiki/Handshake_Lemma" %}





**Handshaking theorem** states that the sum of degrees of the vertices of a graph is twice the number of edges. ... Since every edge is incident with exactly two vertices,each edge gets counted twice,once at each end. Thus the sum of the degrees is equal twice the number of edges.

propositional logic - 命题逻辑； predicate logic

set theory - cardinality, natural numbers X0; real numbers X





[https://lyl0724.github.io/2020/01/25/1/](https://lyl0724.github.io/2020/01/25/1/) recursion understanding [https://www.acwing.com/file\_system/file/content/whole/index/content/1487119/](https://www.acwing.com/file\_system/file/content/whole/index/content/1487119/)

{% embed url="https://github.com/alicex2020/Chinese-Landscape-Painting-Dataset" %}

Discrete Mathematics and Its Applications 7th by Rosen

离散数学第2版 \[屈婉玲，耿素云，张立昂 编著] 2015年版

combination number is in video&#x20;



[https://www.chaoswang.cn/](https://www.chaoswang.cn/) think twice code once

math 3, first 55 minutes is gauss elimination; then is combination numbers

[https://www.youtube.com/watch?v=CyXIIrR49-g](https://www.youtube.com/watch?v=CyXIIrR49-g) pku discrete math book

BIT fenwick tree same as 树状数组

; segment tree 线段树

斜率优化=convex hull trick!

CP3 book

3 steps: idea - code - AC saber work within time limit

{% embed url="https://encyclopediaofmath.org/wiki/Interval_and_segment" %}

interval区间segment线段 range

C++ primer 5th edition - book&#x20;

bitset

Eliminating Duplicates

To eliminate the duplicated words, we will first sort the vector so that duplicated words appear adjacent to each other. Once the vector is sorted, we can use another library algorithm, named unique, to reorder the vector so that the unique elements appear in the first part of the vector. Because algorithms cannot do container operations, we’ll use the erase member of vector to actually remove the elements:

\
&#x20;`// sort words alphabetically so we can find the duplicates sort(words.begin(), words.end());`\
&#x20;`// unique reorders the input range so that each word appears once in the front portion of the range and returns an iterator one past the unique range`&#x20;

`auto end_unique = unique(words.begin(), words.end());`&#x20;

`// erase uses a vector operation to remove the nonunique elements`&#x20;

`words.erase(end_unique, words.end());`

g++ -std=c++11 your\_file.cpp -o your\_program

