---
layout: post
title:  "Suffix Automaton and its Application"
date:   2025-04-01 22:21:00 +0800
categories: CS Algorithm Blog 
tags: CS Algorithm Blog
---

*Author: Gorenstein, Gryffindor, Class of 2024*

> 一般有固定模式串的字符串处理问题和固定主串的字符串处理问题两大类问题。当固定模式串时，熟知的 AC 自动机算法便可以胜任这类问题。如果主串固定，一般采用对主串构造后缀树、后缀自动机来解决这一类问题。

《后缀自动机在字典树上的拓展》（国家候选队，2015，刘研绎）

------------

### 凡例

使用 $\rm Definition$ 表示一个定义；使用 $\rm Proposition$ 表示一个命题。具体给出定义和命题时，都会加上其标号，以方便后文表示。定义和命题一并标号。

尤为重要的命题使用 $\rm Theorem$（定理）突出。本文中仅出现一个定理，故不进行标号。

# 后缀自动机

## DFA 及 SAM 的定义

$\bf Definition\;1.\quad$一个**确定性有限状态自动机** (Deterministic Finite Automaton, **DFA**) 是指包含以下五部分的资料：
- 有限**状态集合** $Q$。
- 有限**字符集** $\Sigma$。
- **转移函数** $\delta(q,\sigma):Q\times\Sigma\to Q$。
- 初始状态 $q_0$。
- 接受状态集合 $F\subseteq Q$。

接下来给出一个字符串在自动机里可接受的定义。判断字符串是否被接受的过程称为计算。

$\bf Definition\;2.\quad$设 $M=(Q,\Sigma,\delta,q_0,F)$ 为一 DFA，$w:=w_1w_2\cdots w_n\;(w_i\in\Sigma)$ 为一字符串，记作 $w\in\Sigma^*$。$w$ 被称作在 $M$ 中是**可接受的**，或称 $\boldsymbol M$ **接受** $\boldsymbol w$，如果 $\exist r_0r_1\cdots r_n\in Q^*$ 使得：
- $r_0=q_0$。
- $\delta(r_i,w_{i+1})=r_{i+1}$。
- $r_n\in F$。

接下来定义后缀自动机的概念。

$\bf Definition\;3.\quad$一个由串 $S$ 诱导的**后缀自动机** (Suffix Automaton, **SAM**) 是指具有以下两个特征的 DFA：
- 接受且仅接受 $S$ 的所有后缀。
- 具有最小的状态集合和转移数量。

所谓**形式语言**或语言为 $\Sigma^*$ 的子集。若对于语言 $L$，有 $\forall w\in L$ 在 $M$ 中可接受，则 $L$ 称为 Regular 语言即正则语言。

## 自动机状态的细节及性质

### SAM 中状态须满足的条件

记 $SAM_{str}$ 为从初始状态读入 $str$ 后处于后缀自动机上的状态。先来考察 $SAM_{str}$ 的一些初等性质，随后我们再刻画 SAM 上状态的细节。
- 对于子串 $str$，$SAM_{str}$ 必为合法状态，且它可转移至 $S$ 的一个后缀，即一个可接受的状态。
- 若 $str$ 非 $S$ 的子串，则它不是合法状态，因为无论如何转移皆不可被接受，应当指向空状态。

因此，SAM 上的转移对应原串中的一个子串往后扩充一个字符，一条转移路径即指原串的一个子串。由此可见，SAM 上的一个状态应当代表某一族子串的某些后缀，或说 _一族具有相同后缀的子串_ ；由此亦可看出，SAM 包含了原串所有子串的信息，且不含冗余信息。

$\bf Note.\quad$这里称子串是不同的，指的是它们作为字符串 _本质不同_ 。对于本质相同而出现位置不同的子串，这里称为不同的 _子段_ 。

### $\bf endpos$ 等价类：状态的确切意义

$\bf Definition\;4.\quad$记 $\mathrm{endpos}(str)$ 为子串 $str$ 每一次出现的右端点位置的集合，亦即 $\bf endpos$ **集**。

考虑子串 $A$ 与 $B:=a+A$。若 $\mathrm{endpos}(A)=\mathrm{endpos}(B)$，则子串 $A$ 与 $B$ 在 $S$ 上往后扩展一个字符所得的新子串必相同，即后缀自动机上 $A$ 与 $B$ 的转移应当相同。基于这点，我们给出 $\rm endpos$ 等价类的定义，以它作为 SAM 上一个状态代表的准确意义。

$\bf Definition\;5.\quad$令 $\mathrm{endpos}$ 集合相同的那些子串共同构成一个 $\boldsymbol{\mathbf{endpos}}$ **等价类**。

根据上述讨论，我们可以概括出如下刻画后缀自动机的状态的定理，使状态满足前文的 $\rm Definition\,3$。这也是本文唯一一个定理。

$\bf Theorem.\quad$**后缀自动机上的状态即代表一个 $\mathbf{endpos}$ 等价类。**

由此我们亦记状态 $s$ 代表的 $\mathrm{endpos}$ 等价类为 $\mathrm{endpos}_s$。

### 等价类的性质与后缀链接树

${\bf Proposition\;6.\quad}\mathrm{endpos}$ 有以下基本的性质：
1. 对于两个子串 $A,B$，它们的 $\mathrm{endpos}$ 集合或者相包含，或者无交。
2. 若 $A$ 的 $\mathrm{endpos}$ 集合包含于 $\mathrm{endpos}(B)$，则必有 $A$ 与 $B$ 共享一段后缀，且 $A$ 长于 $B$。
3. 在第二点的基础上，设 $A$ 所在的 $\mathrm{endpos}$ 等价类为 $K$，$B$ 所在的 $\mathrm{endpos}$ 等价类为 $L$，则 $K,L$ 满足 $\forall A\in K,B\in L$ 有 $B$ 是 $A$ 的后缀。
4. 一个 $\mathrm{endpos}$ 等价类中没有等长的字符串。
5. 一个 $\mathrm{endpos}$ 等价类中包含的字符串长度为连续的一段区间，且短者为长者的真后缀

由此，对于状态 $s$，寻找一个状态 $fa_s$ 满足 $\mathrm{endpos}_s\subsetneq\mathrm{endpos}_{fa_s}$ 且 $\left|\mathrm{endpos}_{fa_s}\right|$ 最小。称 $fa_s$ 为状态 $s$ 的**父状态**，记 $\mathrm{link}(s):=fa_s$ 为**后缀链接**；所有状态由后缀链接构成一棵树，即所谓 $\bf Parent$ **树**。$(\bf Definition\;7.)$显见：
- 一个状态 $s$ 在 $\rm Parent$ 树上的子树表示以 $s$ 中最短者为后缀的子串之集合。
- 一个状态 $s$ 在 $\rm Parent$ 树上到根节点的链代表作为 $s$ 中最长者的后缀的子串之集合。

定义空串为所有串的后缀，空状态的 $\mathrm{endpos}$ 为所有位置。从而空状态在 $\mathrm{Parent}$ 树上为根节点。

## 增量法构建 SAM

记 $len_s$ 为状态 $s$ 代表的最长子串的长度。显见 $s$ 代表的子串的长度区间为 $[len_{\mathrm{link}(s)}+1,len_s]$。

构建 $S$ 的后缀自动机的一个常用且简单的方法是增量法。

设已经构建结束前 $|S|-1$ 个字符，且现在子串 $[1,|S|-1]$ 的位于状态 $last=:p$。现在插入第 $|S|$ 个字符 $c$；记此前最后一个字符为 $c'$。

为插入 $c$，我们新建一个状态 $u$；显然 $len_u=len_p+1=|S|$。

将 $p$ 不断沿 $\rm Parent$ 树向上移动，直至移动到 $\delta(p,c)$ 先前已非零为止。根据前述 $\mathrm{endpos}$ 等价类的性质，此时的 $p$ 的祖先 $F_p$ 必然存在非零的 $\delta(F_p,c)$。对于 $p$ 途中所遇状态 $p'$，令 $\delta(p',c)=u$。此后分为两种情形：
1. $p$ 移动到了初始状态 $p_0$ 且 $\delta(p_0,c)$ 仍然为零，则说明此前不存在 $c'+c$ 这个后缀。从而令 $\delta(p_0,c)=u,\,\mathrm{link}(u)=p_0$。至此，对于此种情形我们完成了 $c$ 的插入。
2. 否则，说明后缀 $c'+c$ 已存在。

对于第二种情形，设 $q=\delta(p,c)$，则 $\mathrm{endpos}_q$ 由于 $c$ 的加入而变化。此时仍分为两种情形：
1. $len_q=len_p+1$。这说明（只考虑组成子串的字符及其顺序本身）$p$ 代表的子串为对应 $q$ 代表的子串的最长真前缀。从而加入 $c$ 后所有 $q$ 中子串的 $\mathrm{endpos}$ 都会增加一个 $|S|$，原先的 $\mathrm{endpos}$ 等价类不变。
2. $len_q>len_p+1$。在 $q$ 代表的子串中，长度不超过 $len_p+1$ 者的 $\mathrm{endpos}$ 集会增加一个 $|S|$，而对于长度在 $(len_p+1,len_q]$ 间的子串则不会。因此我们需将 $q$ 分裂。

对于第二种情形，记分裂出的状态为 $v$ 原先的 $\mathrm{endpos}$ 等价类中加入 $|S|$ 的部分，即其前缀出现在 $p$ 中的部分。先令 $v$ 的转移继承 $q$，并令 $\mathrm{link}(v)=\mathrm{link}(q)$。显然 $len_v=len_p+1$，且 $\mathrm{link}(u)=\mathrm{link}(q)=v$。随后再将 $p$ 沿 $\rm Parent$ 树上移直至 $\delta(p,c)\neq q$，并将沿途的 $\delta(p,c)$ 置为 $v$。

```cpp
int SAMadd(int c){
	int u=++ncnt,p=lst;
	lst=u,Len[u]=Len[p]+1,sz[u]=1;
	for(;p&&!tre[p][c];p=Fa[p])tre[p][c]=u;
	if(!p)Fa[u]=1;
	else{
		int q=tre[p][c];
		if(Len[q]==Len[p]+1)Fa[u]=q;
		else{
			int Splt=++ncnt;
			for(int i=0;i<26;i++)
				tre[Splt][i]=tre[q][i];
			Fa[Splt]=Fa[q],
			Len[Splt]=Len[p]+1,Fa[q]=Fa[u]=Splt;
			for(;p&&tre[p][c]==q;p=Fa[p])tre[p][c]=Splt;
		}
	}
	return u;
}
```

## 一些初等应用

从上述对 SAM 的讨论中立即得出：
- $len_s-len_{\mathrm{link}(s)}$ 为状态 $s$ 包含的 $\mathrm{endpos}$ 集的数量，亦即包含的不同子串数量。
  - 对每个状态的 $len_s-len_{\mathrm{link}(s)}$ 求和即得原串 $S$ 的不同子串数量。
- 记 $ed_s$ 为状态 $s$ 的 $\mathrm{endpos}$ 集的大小。令不是分裂所得的非初始状态的 $ed$ 为 $1$，然后在 $\mathrm{Parent}$ 树上做子树求和即可得到最终的 $ed_s$。
  - 对于一个询问的模式串 $T$，在 SAM 上找到状态 $SAM_T$，则 $ed_{SAM_T}$ 即为该模式串在文本串 $S$ 中的出现次数 $f(T)$。
- $ed_s\!\left(len_s-len_{\mathrm{link}(s)}\right)$ 为状态 $s$ 代表的不同子段数量。
  - 对其求和得到 $S$ 的所有子段数量，它应当等于 $\frac{|S|(|S|+1)}{2}$；它也是所有不同模式串在 $S$ 中出现次数之和，即 $\sum_{T}f(T)$。
- 在自动机上 DP 可求得从每个状态 $s$ 出发可走到的子串 $sum_s$。

### 计数排序法

事实上 $\rm Parent$ 树上子状态的 $len$ 必然小于父状态，因此对 $len$ 计数排序后倒序扫描即可，而不需要进行 DFS。（不可以直接倒着扫，因为中间会分裂状态）事实上由于转移表示新增字符，因此只可能转移到 $len$ 更大者。因此自动机上亦适用此方法。

至此可以完成 [P3804](https://www.luogu.com.cn/problem/P3804) [SP8222](https://www.luogu.com.cn/problem/SP8222) [SP705](https://www.luogu.com.cn/problem/SP705) [P4070](https://www.luogu.com.cn/problem/P4070) [CF802I](https://www.luogu.com.cn/problem/CF802I) [CF123D](https://www.luogu.com.cn/problem/CF123D) [P4341](https://www.luogu.com.cn/problem/P4341)。

```cpp
void Sort(int flg){
	for(int i=1;i<=ncnt;i++)t[Len[i]]++;
	for(int i=1;i<=ncnt;i++)t[i]+=t[i-1];
	for(int i=1;i<=ncnt;i++)A[t[Len[i]]--]=i;
   	for(int i=ncnt;i;i--)sz[Fa[A[i]]]+=sz[A[i]];
	for(int i=2;i<=ncnt;i++)sum[i]=flg?sz[i]:1;
	for(int i=ncnt;i;i--)
		for(int j=0;j<30;j++)
			if(tre[A[i]][j])sum[A[i]]+=sum[tre[A[i]][j]];
}
```

## 自动机规模和时空平衡

### 对自动机大小为线性的证明

根据 $\mathrm{endpos}$ 的定义， $\mathrm{Parent}$ 树上至多有 $n$ 个叶子节点；而树上一个结点必有至少 $2$ 个子结点，因此总状态数不会超过 $2n-1$。

考虑自动机上以初始状态为根的最长路径生成树，其边数不会超过 $2n-2$，因此只需考虑非树边。从初始结点开始到终止状态的任意一条路径，它是一个后缀。设其间包含某个转移 $(p,q)$ 为非树边，令此种决定的字符串为 $u+c+w$。所有 $u+c+w$ 都是不同的，而后缀只有 $n$ 个，且从根结点到终止状态的后缀不包含非树边，从而非树边不超过 $n-1$ 条。

由此可得总转移不超过 $3n-3$ 个。事实上转移数最多的字符串形如 $\texttt{abb}\cdots\texttt{bbc}$，其转移数为 $3n-4$。因此总转移数不超过 $3n-4$。

### 时空平衡

- 一般的写法时间 $O(n)$，空间 $O(n|\Sigma|)$。
- 使用 $\rm vector$ 或链式前向星存储每个状态的转移，操作时暴力查找，时间 $O(n|\Sigma|)$，空间则为 $O(n)$。
- 对状态进行根号分治，较小的链式前向星，较大的使用数组，时空平衡为 $O\Big(n\sqrt{|\Sigma|}\Big)$。
- 使用 $\rm map$ 存转移，时间 $O(n\log|\Sigma|)$，空间 $O(n)$，但空间常数大，适用于字符集较大时。
- 状压字符集，手写哈希表统一存储转移 $\delta(p,c)$，时空复杂度均为 $O(n)$，要求字符集较小。

至此可以完成 [P5357](https://www.luogu.com.cn/problem/P5357)。

## 基本应用

### 子串 Kth 和最小后缀

**第 $\boldsymbol k$ 小子串。**$k$ 小子串对应 SAM 上字典序第 $k$ 小的路径。预处理出每个状态能走到的子串或子段个数，DFS 即可。至此可以完成 [SP7258](https://www.luogu.com.cn/problem/SP7258) [P3975](https://www.luogu.com.cn/problem/P3975)。

```cpp
//注意：需将 sz[1] 置 0, 且若求本质不同子串，则所有非零的 sz 应等于 1。
void Kth(int x,int k){
	if(k<=sz[x])return;k-=sz[x];
	for(int i=0;i<26;i++){
		int y=tre[x][i];if(!y)continue;
		if(k>sum[y])k-=sum[y];
		else{
			putchar(i+'a'),Kth(y,k);
			return;
		}
	}
}
```

**最小循环表示。** 将子串复制一倍接在原串后，对复制后的串建立 SAM，并贪心地选择最小的转移往后走。走过 $n$ 个转移后经过的字符串即为最小循环表示。

### 最长公共子串

**双串最长公共子段。** 对第一个串 $A$ 建立 SAM，将第二个串 $B$ 在 SAM 上进行匹配。记 $lcsl$ 为以 $B$ 第 $i$ 个字符结尾的 LCS。若存在转移，则 $lcsl$ 加 $i$，并进行转移。否则在 $\rm Parent$ 树上向上走，直到存在转移；设第一个存在转移的状态为 $p$，令 $lcsl\leftarrow len_p+1$，并从 $p$ 往 $\delta(p,B_i)$ 转移。若不存在这样的 $p$，$lcsl\leftarrow 0$。至此可以完成 [SP1811](https://www.luogu.com.cn/problem/SP1811)。

如何求解**多个串的 $\bf LCS$？**

我们要对除了文本串之外的每个串求一遍上述过程。大致思路是：设 $\circ_s$ 表示对于当前一个模式串的以它结尾的最长公共字段。令 $mn_s$ 为对所有 $s$ 的 $\circ_s$ 的最小值，则所有 $mn$ 的最大值即为答案。

同双串一样的做法是不好处理的，因为文本串 $A$ 中 SAM 的状态存储的是一族子段而不是结束位置。由此，因此考虑将上述 $\circ$ 的信息直接存到 SAM 的状态里。从而设 $mx_s$ 为状态 $s$ 包含的子串能与当前模式串匹配的最长长度。
- 显然，因为 $s$ 存储的是一些前缀的后缀，因此它自然兼容了 $lcsl$ 的定义中“以某处结尾的串”的涵义。且它不再依赖模式串 $B$，而是可以存储在只与文本串 $A$ 有关的 SAM 中。

先按求 $lcsl_s$ 一样求 $mx_s$。这样我们就求出了 $B$ 在 SAM 上经过的状态 $s$ 的 $mx_s$。然后按拓扑序更新，令：
$$mx_{\mathrm{link}(s)}\;\longleftarrow\;\min(\max(mx_s,mx_{\mathrm{link}(s)}),len_{\mathrm{link}(s)})$$
拓扑序的过程中同时更新 $mn_s$。
- 对于 $B$ 未直接经过但能够匹配的状态，即直接经过的状态的祖先状态，我们也要更新。这是因为这些状态虽然对于当前串不是最优（因为它们代表的子段的长度小于直接经过的状态），但也能够匹配；如果另外一些模式串 $C$ 直接经过了这些先前未曾被直接经过但能匹配的点，且这些点不存储与当前串 $B$ 的匹配长度，在对每个状态计算 $mn$ 时就会出错。
- 应当选取最短串作为文本串。

至此可以完成 [SP1812](https://www.luogu.com.cn/problem/SP1812) [SP10570](https://www.luogu.com.cn/problem/SP10570)。

```cpp
void LCS(char *str,int n){
	int x=1,l=0;
	for(int i=1;i<=ncnt;i++)mx[i]=0;
	for(int i=0;i<n;i++){
		int ch=str[i]-'a';
		for(;x&&!tre[x][ch];x=Fa[x],l=Len[x]);
		if(x)x=tre[x][ch],mx[x]=max(mx[x],++l);
		else x=1,l=0;
	}
	for(int i=ncnt;i;i--)
		mx[Fa[A[i]]]=min(max(mx[Fa[A[i]]],mx[A[i]]),Len[Fa[A[i]]]),
		mn[A[i]]=min(mn[A[i]],mx[A[i]]);
}

for(int i=1;i<=T;i++)if(p^i)sam.work(str[i],len[i]);
for(int i=1;i<=sam.ncnt;i++)ans=max(ans,sam.mn[i]);
```