# Codeforces Round 660 (CF1388) 题解

## Problem A - Captain Flint and Crew Recruitment

### 题目描述

给定一个正整数$n$，问能否将其表示为四个互不相等的正整数的和，要求其中至少三个正整数可以表示为$p\cdot q$的形式（$p$和$q$是不相等的质数）。

### 题解

这题乍一看有点恐怖，毕竟跟质数沾上了边，怎么想也不像是Div.2A。再看题目，关键点是**至少三个**，也就是说，我们只要保证三个数符合条件，剩下一个数直接用剩下的差就行。

考虑符合条件的三个数，我们选尽可能小的，那就是$6=2\times3$，$10=2\times5$，$14=2\times7$。这三个数的和为$30$，因此剩下那个数为$n-30$，所以$n\leq30$时无解。

还要考虑几个特殊情况，也就是剩下那个数$n-30$跟已有的三个数中某一个相等。此时，把$14$换成$15$，剩下那个数换成$n-31$即可。

经验就是CF的题号和难度对应关系是非常明确的，在Div.2的A/B题感觉卡住了的话，通常可以认为是有关键条件没有注意到，导致想复杂了。

::: spoiler 参考代码（C++）

<<< @/docs/editorial/codeforces/1388/src/a.cpp

:::

## Problem B - Captain Flint and a Long Voyage

### 题目描述

求出能够使得逐位二进制展开再去掉最后$n$位的结果对应的二进制数最大的最小$n$位数。

例如，$9$逐位展开的结果是$1001$，再去掉最后$1$位，变为$100$。

### 题解

因为二进制展开结果不会有先导$0$，所以肯定位数越多结果越大，因此我们的$n$位数每一位只能选$8$或$9$，否则位数就损失了。什么情况下要选$8$呢？因为$8$对应$1000$，$9$对应$1001$，所以只要最后一位是要被删去的，$8$和$9$的结果就没有区别，此时为了使得这一$n$位数最小，这一位上就要选$8$。

所以就是很简单的贪心了，最后$\left\lceil\frac{n}{4}\right\rceil$位放$8$，前面都放$9$。

::: spoiler 参考代码（C++）

<<< @/docs/editorial/codeforces/1388/src/b.cpp

:::

## Problem C - Uncle Bogdan and Country Happiness

### 题目描述

一棵根为$1$的树，一开始所有人都在根节点处，接下来他们会沿着最短路径回家。已知家在第$i$个节点的人有$p_i$个，第$i$个节点处的快乐值为$h_i$表示在该节点处快乐的人和不快乐的人的个数差。已知一个人只会从快乐变成不快乐，问给定的$h$是否有可能成立。

### 题解

很显然是一道DFS的题目，关键是如何提取出约束条件。因为一个人只会从快乐变为不快乐，所以任意子树的根节点处，快乐的人数一定不少于这一子树的所有子节点的快乐人数之和。另一方面，快乐的人数也不会超过经过该节点的总人数。除了这两个比较显而易见的约束，还有一个不那么明显的约束。事实上，我们是可以得到下面的等式的

$$happy[i] - (cnt[i] - happy[i]) = h_i$$

其中$happy[i]$表示第$i$个节点处快乐的人数，$cnt[i]$表示第$i$个节点处的总人数。

我们进一步可以得到

$$happy[i]=\frac{h_i+cnt[i]}{2}$$

因为$happy[i]$需要为非负整数，所以$h_i+cnt[i]$必须为偶数。

然后在DFS过程中判断上面三个条件是否都满足就行了。

::: spoiler 参考代码（C++）

<<< @/docs/editorial/codeforces/1388/src/c.cpp

:::

## Problem D - Captain Flint and Treasure

### 题目描述

有$n$个数$a_1\cdots a_n$，以及对应的$n$个序号$b_1\cdots b_n$。要求对$n$个数按照某一顺序依次操作一次，使得得到的总和最大，求出这个总和并给出这一顺序。

其中，每次操作会使得总和增加$a_i$，如果$b_i\neq-1$，还会使得$a_{b_i}+=a_i$。

对于任意的$i$，$b_i,b_{b_i},b_{b_{b_i}},\cdots$不构成循环。

### 题解

题目明确说了$b$中不存在环，所以我们可以按照$b$所描述的依赖关系进行拓扑排序，然后依次处理。

对于当前处理到的$i$，如果$a_i>0$，显然我们希望把它加到后面的依赖项上，因此我们应当把$i$排在前面（从左往右排）；反之，我们则应当把$i$排在后面（从右往左排）。

显然，这样得到的结果一定是最优的。

::: spoiler 参考代码（C++）

<<< @/docs/editorial/codeforces/1388/src/d.cpp

:::

## Problem E - Uncle Bogdan and Projections

### 题目描述

X轴上方有$n$条互不相交且平行于X轴的线段。现在要求将这些线段互不相交地沿同一方向投影到$X$轴上，问最后得到的投影在X轴上的左端点和右端点间的距离最短为多少。

### 题解

实际上就是要找到最优的倾角。

显然，对于任意两条线段，可以计算出不可行的倾角区间$(\theta_l,\theta_r)$。将这些区间进行合并，可以得到总的不可行区间。

接下来，我们要选择的倾角一定是某个区间的端点（有一种特殊情况是所有线段的$y$都相同，此时可以选择任意倾角）；如果不选择端点，那么在其相邻的端点中，一定有一个比它更优。（证明……略。）

所以，就枚举合并后剩下区间的端点，对每一个倾角的取值，枚举所有线段，计算得到最后的左端点和右端点。

为了避免误差，所有的倾角都用分数表示。

这实际上还是$O(n^3)$的暴力，不过用C++ 64可以在时限内通过。

::: spoiler 参考代码（C++）

<<< @/docs/editorial/codeforces/1388/src/e.cpp

:::

官方题解$O(n^2\log n)$的凸包方法还没学会。