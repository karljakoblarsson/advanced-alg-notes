% Advanced Algorithms - TDA251
% Jakob Larsson <jakob@karljakoblarsson.com>
% Lp2 2017

Lecture 3 - 2017-11-06
======================


Todays problem:

We have a undirected graph of nodes who wants to send messages to
other nodes but the edge have a limited bandwidth. The edges can not
be shared. The goal is to serve as many communications requests as
possible. Nodes can be shared.

Disjoint paths
--------------

_Given:_
A directed graph $G = (V,E)$, $|E| =m$, $Sᵢ, tᵢ ∈ V (i = 1, …, k)$

_Find:_
Maximum number of edge-disjoint $Sᵢ­tᵢ$-path.

This problem is NP-complete, it's about indepentent paths.

It's a good strategy to choose one path at a time. It's also a good
idea to choose the shortest path since that has a lower risk to block
another path. However it can still block other paths.


A Greedy approach
-----------------

Choose some shortest path $P$ that connects some yet unconnected pair.
Remove the edges of $P$.
Repeat this as long as possible.


$I* := \{ i | sᵢ­tᵢ$­path exists in the optimal solution $\}$

$I := \{ i | sᵢ­tᵢ$­path exists in the greedy solution $\}$

$Pᵢ*, Pᵢ =$ the $sᵢ­tᵢ$­path.

$l(P)$ the length of path $P$.


Any directed path in the graph with less-equal then or more then at
least $\sqrt{m}$ is long or short, respectivly.

$I*_s$ is the set of short paths in he solution.

$I*_s ⊆ I*$, $I_s ⊆ I$ : set of short path in respective solution.


Consider any $i$ with $i ∈ I*_s$, $i ∉ I$. This is a bad thing. How
can the greedy algo miss this? (The greedy algo dosen't find any
part.) This can only happen if some edge on this path is already used
in another path. Call this $Pᵢ*$

There must exists a greedy path $Pⱼ$ which uses at least on of the
edges in $Pᵢ*$.

$l(Pⱼ) ≤ l(Pᵢ*)$, $Pⱼ ∈ I_s$

Every edge $e ∈ Pⱼ$ blocks $≤ 1$ path of $I*$. $Pⱼ$ blocks $≤
\sqrt{m}$ paths of $I*$.

$$| I* | ≤ | I* ∖ I*_s |  ≤ | I*_s ∖ I | + | I*_s ∩ I |$$

The first term on the right is bounded by $≤ \sqrt{m}$

The second term in the right is bounded by $≤ |I_s|·\sqrt{m}$.

The last term is bounded by $|I|$.

The entire right hand side is bounded by $≤ \sqrt{m} +
|I_s|·\sqrt{m} + |I|$ 

Which is equal to $≤ (2\sqrt{m} + 1)|I|$.

This is a so called souble counting argument where we count the same
thing in different ways.

This was the messy/hard problem.

In the remainging time we study a problem which can be approx very well.


Knapsack (revisited)
--------------------

_Given:_
A container with capacity $W$
and $n$ objects with weights $wᵢ ≤ W$
Values $vᵢ$ 

$i = 1, …, n$

_Find:_
A subset $S ⊆ \{1, …, n\}$ with
$$ \sum_{i ∈ S} wᵢ ≤ W, \max \sum_{i ∈ S} vᵢ$$

$v_{\text{max}} := \max_i vᵢ$

This problem is NP-complete as well and can be solved in $O(nW)$ time.


<!-- Break -->


Alternative Exakt Algo (Dynamic Programming)
--------------------------------------------

<!-- In the original dynamic programming we consider a subset of the items -->
<!-- and a limited capacity and find that solution. -->

```Pseudo
OPT(i, V) := min. capacity needed to achieve a total value ≥ V,
	        uaing a subset of the first i items.
		(infinity, if no solution exists)
```

How do we compute this from previous values.

```Pseudo
OPT(i, V) := min {OPT(i - 1, V), wᵢ + OPT(i - 1, max{V - vᵢ, 0})}
```

Computing each of these values cost constant time. How much time does
the whole algo cost.

$i ≤ n$, $V ≤ n·v_{\text{max}}$

$O(n²·v_{\text{max}})$

$v_{\text{max}} := \max_i vᵢ$

(Quantize the values)

$b = $ a free parameter, $b > 1$

$$vᵢ' := ⌈\frac{vᵢ}{b}⌉$$

$S*$ denotes some optimal solution for the original values $vᵢ$.
$S$ is the optimal solution for the new values $vᵢ'$.

What's the ratio?

$$ \sum_{i ∈ S*} \frac{vᵢ}{b} ≤ \sum}{i ∈ S*} vᵢ' ≤ \sum_{i ∈ S} vᵢ' ≤
\sum_{i ∈ S} \( \frac{vᵢ}{b} + 1\)$$

$S$ is the optimal solution for the changed values.

We used that we have just rounded the values and not increased them to much.

(Move the constant 1 outside the sum, it becames bounded by $n$)

$$ \sum_{i ∈ S*} vᵢ ≤ n·b + \sum_{i ∈ S} vᵢ $$

The $n·b$ term is annoying.

One simple observation: $v_{\text{max}} ≤ \sum_{i ∈ S*} vᵢ$

$ε := \frac{nb}{v_{\text{max}}}$


$$ nb ≤ ε · \sum_{i ∈ S*} vᵢ $$

$$ (1 - ε) \sum_{i ∈ S*}a vᵢ ≤ \sum_{i ∈ S} vᵢ$$

Our solution is no worse than $1-ε$ times the optimal solution. And
the $ε$ is not constant, it depends on $b$.

The running time:

$$ O(n²· \frac{v_{\text{max}}}{b}) = O(\frac{n³}{ε})$$

This is maybe not a very nice time-bound but it's a good thing in
practice that we can choose the tradeoff between running time and
approx quality.

Choose $ε$. If $ε ≤ \frac{n}{v_{\text{max}}}$ then use $b = 1$

$$v_{\text{max}} ≤ \frac{n}{ε}$$

$O(\frac{n³}{ε}$ time is still valid.


PTAS
----

A PTAS is an algorithm with the following properties.

It solves a maximization or minimization problem in polynomial time
for every fixed $ε$.

A PTAS solves a maximum or minimum problem in $p_{ε}(n)$
time. ($p_{ε}$ is polynomial time for every $ε$), and returnas a
selection within a factor $1+ε$ or $1-ε$ of optimum.

$$ O\( n^{\frac{1}{ε^20}} \) $$

The PTAS is the best possible we can hope for a NP-hard problem.

Optimization problems can behave very differently when approximating.
