% Advanced Algorithms - TDA251
% Jakob Larsson <jakob@karljakoblarsson.com>
% Lp2 2017

This is a work in progress. Specifically the first part is very
messy. (08:00 lectures aren't my thing.)

Lecture 2 - 2017-11-01
======================

We continue with subsets and covering.

Set Cover
---------

_Given:_
A set $u$,
$| u | = n$,
$s₁, …, s_m ⊆ u$,
each $sᵢ$ has weight $wᵢ > 0$

_Find:_
$C ⊆ \{s₁, …, s_m\}$
with $\bigcup_{sᵢ ∈ C} sᵢ = u$
and $\min \sum_{sᵢ ∈ C} wᵢ$


This problem is NP-complete by

Vertex Cover $≤_p$ Set Cover (It's reducible in polynomial time to)


### Greedy Algorithm ###

The first step is as always a greedy algorithm. A Greedy algo is
almost never the optimal solution but it's often a good starting point.

This algo is given more formally because we will need the notation
later.

```Pseudo
C := ∅
R := u -- The remainder set

while R ≠ ∅
	take some Sᵢ with minimal wᵢ / | Sᵢ ∩ R |
	C := C ∪ { Sᵢ }
	R := R ∖ Sᵢ

```


### Improvment ###

$C$ is the greedy solution and $C*$ is the optimal solution and $Wᵢ$
is the optimal weight.

#### Lemma: Harmonic Sum ####
$$H(n) = \sum_{i=1}^n \frac{1}{i} ∼ \ln n$$


The elements pay for being covered. (This is a general trix in
optimization, to transfer the costs somehow.

For a element $s ∈ u$ the price is $c_s := \frac{wᵢ}{|Sᵢ ∩ R |}$

Now the cost of the greedy solution is:
$$ \sum_{Sᵢ ∈ C} wᵢ = \sum_{s ∈ u} c_s$$
The costs are the same.

The first step is to consider only a single set. Any $s_k$. (Maybe
this set is part of the optimal solution $s_k ∈ C*$)

How much has this set $s_k$ paid for being covered by the greedy algo
compared to the cost for the set itself $w_k$.

$s_k = \{s₁, …, s_d\}$ (where the elements are covered in this order
by the greedy algo)

Consider any fixed $j$. Now we want to estimate the cost of covering.

Just before $s_j$ gets covered: 

Meanwhile, the uncovered part of our set $|S_k ∩ R| ≥ d-j+1$

$$ \frac{w_k}{|s_k ∩ R|} ≤ \frac{w_k}{d-j+1}$$

$sᵢ$: the greedy set that covers $s_j$

$$ \frac{w_i}{|s_i ∩ R|} ≤ \frac{w_k}{d-j+1}$$

Next we sum up all the parts.

Consider the sum which is equal to the harmonic sum by a constant:

$$ \sum_{s ∈ s_k} c_s ≤ \sum_{j=1}^n \frac{w_k}{d-j-1} = H(d)·w_k$$

Now let $d := \max_k | s_k ∣$.

$$ \sum_{s_k ∈ C⋆} \sum_{s ∈ s_k} c_s ≤ H(d)·w⋆ $$

$$ \sum_{s ∈ u} c_s ≤ H(d)·w⋆ $$

This is an ok algorithm but it's only a worst case analysis. $d$ is
often small which is good. $H(d)$ is already the optimal ratio, there
is no algorithm which approximates set cover with a better upperbound
than this. (Given that P ≠ NP.)

This is quite counter-intuitive, that this simple algo is in some way
the best we can do.

<!-- Break -->

Pricing Method - Example: Weighted Vertex Cover
-----------------------------------------------

_Given:_
A graph $G = (V, E)$, $V = \{1, …, n\}$
node $i)4 has weight $wᵢ > 0$

_Find:_ vertex cover with minimum total weight.

- NP-complete

But we solved Set Cover approximation and since Vertex Cover is reducible to
Set Cover we already have a $H(d)$-approximation.

$d$ is the maximum degree in the graph.

Approximation ratios do not carry over polynomial-reducability in general.

But since Vertex cover is a special case of Set Cover we can deduce
the approx ratio.

Now the verticies pay for being covered, not only in the algorithm but
in the problem itself.

Edge $e$ pays a __price__ $p_e$ for being covered.

A pricing is fair if it dosen't exceed the actual cost.
Prices $p_e, e∈ E$, is __fair__ if
$$∀i : \sum_{e = (i,j)} p_e ≤ wₒ$$

$S$ is the vertex cover.
$w(s)$ is its weight.

$$ \sum_{i ∈ S} \sum_{e = (i,j)} p_e $$

$S$ is a vertex cover so all edges are covered at least once.

$$ \sum_{e ∈ E} p_e ≤ w(s) $$

This sum is very central for the whole
field of optimization.

This inequality says: the sum of any fair prices is the lower
bound. This holds for any fair prices so now we can maximize the fair
prices. The original problem is NP-hard but the price maximization
problem is simpler.

The inequality gives rise to a dual problem: maximum fair prices. It's
hard to define what a dual problem means. It somehow means that there
is a one to one corresponance and somehow easier or at least different.

We can solve the dual problem and then use that solution to construct
a solution of the primary problem.

But this is only an inequality, it says nothing about how tight the
bound is.


Node $i$ are tight if:

$$ \sum_{e =(i,j)} p_e = wᵢ $$

### Algos ###

<!-- is or are -->
```Pseudo
for all e ∈ E do
	p_e ::= 0
	while some edge lacks tight end nodes
		take one such edge e := (i, j)
		and raise the price p_e until i, j or both are tight
	
	S := set of tight nodes
```

This algo runs in polynominal time if you implement it thoughtfully.
But I don't really understand why.

$S$ is a vertex cover.

$$ \sum_{i ∈ S} \sum_{e = (i,j)} p_e  = \sum_{i ∈ S} wᵢ = w(S)$$

$$ w(S) ≤ 2 \sum_{e ∈ E} p_e ≤ 2 w(S)$$

It's hard to approx Vertex Cover better than this. But it's
complicated to explain why.


