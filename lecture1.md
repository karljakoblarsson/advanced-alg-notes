% Advanced Algorithms - TDA251
% Jakob Larsson <jakob@karljakoblarsson.com>
% Lp2 2017

Lecture 1 - 2017-10-30
======================


"My problem is NP-compleete - what now?"

Ex1: Load Balancing
-------------------

_Given:_ $n$ jobs. With processing times (job lengths) $t₁, …, t_n$

Assign them to $m$ machines. (The machines are identical with the same
speed.) We can't split the jobs. If a job starts on oen machine it
must end on the same machine.

$T_i =$ load of machine $i$

We want to minimize the running time. Such that the last jobs finish
as early as possible.

Minimize $T$ = $\text{max}_i T_i$

This is the subset sum problem. This is reducible to Load Balancine
with $m = z$ ($≤_p$


_A greedy approach:_

```Pseudo
for j = 1 to m
	assign job j to the machine i with the smallest T_i
```

_Ex:_

`3, 3, 2, 2, 2; m = 2`

Machine 1: 3 + 2 + 2
Machine 2: 3 + 2

But this is not the optimal solution. That is easy to find by hand.

Machine 1: 3 + 3
Machine 2: 2 + 2 + 2

This is not suprising, since the problem is hard and the algorithm is simple.

Is this really a good algortihm?

We could use a priority queue to improve the running time. But that is
more of a soulution from a datastructures course.


$T*$: optimal makespan.

Analyse $T - T*$? I.e. ask what is the worst case.
This makes no sense since the worst-case can become arbitrily
large. So this quantity is meningless.

Instead we study the ratio:

$$\frac{T}{T*}$$

This ration dosen't change when multiplied by a scaling factor.

Is there any hope to get a result here?

The problem is NP-hard so we are looking for
approwimization. (spelling is hard as well)


We call this ratio the __approximation ratio__ of the algorithm.

$\text{max} \frac{T}{T*}$ (The maximum of all instances. (The supreme
maximum really))

This holds for minimization problems. For maximization we instead
consider the min of the approx ratio.

We want a good upper bound of the true approx ratio. (Not a too
generous bound) We can split this problem to find a bound of $T$ and
$T*$ seperatly. A upper bound for $T$ and a lower bound for $T*$. $T$
depends on the algorithm, $T*$ depends only on the problem.

A bound of $T*$ gives us a hint of what kind of upper bound we want to
find for $T$.

_Ex:_
What ever the solution of our porblem, this is a lower bound:
$$
T* ≥ \sum_{k=1}^n \frac{t_k}{m}
$$

Our bounds must work for every possible instance.

Suppose we have one very long job and several very short jobs. Then
the short jobs won't affect the running time since we can't split
jobs. So this is also a bound:

$$
T* ≥ \text{max}_k t_k
$$

They are good in different instances. So we can combine the bounds to
a better but more complicated bounds. We can also devise many more
lower bounds, but for this problem they are not nessescary.

_One advice: always think in pictures_

This is a scheduling problem. So let's draw a schedule.

$i$: machine with $T_i = T$
$j$: last job on machine $i$

Every load is at least: $\sum^n_{k=1} T_k ≥ m(T - T_j)$

$T ≤ T* + t_j ≤ 2 T*$

This analysis can actually not be improved, there are instances where
the greedy algorithm actully is this bad that is takes about twice the
ideal time. The weak point is actually the algorithm and not the
analysis.

<!-- Break -->

Approximation algorithms are algortihms witch runs in polynominals
time and has a upperbound on the worst-case.

_Improve the algorithm_

First sort the jobs and the run the greedy algorithm. Now the analysis
is not so simple anymore.
($ t₁ ≤ t₂ … t_n$)

If we have enough machines the problem is trivial, the each machine
does one job.

If $m ≥ n$ then $T = T*$. Therefore we can assume that $m < n$. The
first $m$ jobs are assigned to all $m$ machines. At least two $m+1$
largest jobs are assigned to the same machine. The makespan can't be
shorter the makespan for this machine. Therefore $T* ≥ 2
t_{m+1}$. But $m+1$ is not $j$. 

If machine $i$ does only job $j$, then the schedual is optimal, $T =
T*$. Now we can conclude that machine $i$ does $≥ 2$ jobs. $j ≤ m+1$
and $t_j ≤ t_{m+1} ≤ ½ T*$. Then $T ≤ \frac{3}{2} T*$. (What is the
state of the art here?)

This is the techniue to get bounds of the approximation.


### Another problem.

<!-- There will be an assignment about approximation algorithms. -->

#### Center Selection.

_Given:_ A set of points in a metric space. (A space with a distance
function, one that fulfills the triangle equality.)

A set of $n$ points (Sites). An integer $k$

_Problem:_ We want to find another set $C$ of $k$ points (Centers)
such that the following is minimized:
$$ \text{max}_{s ∈ S} \text{min}_{c ∈ C} \text{dist}(s, c) $$

We want to place the centers such that the distance to each customer
(Sites) is minimized.

We can come up with many greedy rules that fails.

This problems is also NP-complete.

This is easier if we reformulate the problem. Consider if we already know the
minimum distance, but not the soulution. Then the problem is
simpler. Fix $r$ and ask: Can we cover all sites by $k$ disks of
radious $r$. For a fixed $r$ this is the same problem. Instead of a
min-max-min problem we have a covering problem.

_A greedy algo for a covering problem:_
We place $k$ centers.

```Pseudo
repeat k times:
	take any uncovered site s ∈ S and put this site s in C
```
(As a remark: this greedy algo produces a solution C ⊆ S)

We have to double the radious to $2r$ to make sure that this algo
covers every site. $s ∈ S$ is covers if some $c ∈ C$ exists with
$\text{dist}(s, c) ≤ 2r$

_The final algo:_
```Pseudo
repeat k times:
    Take a s ∈ S with maximum min dist to all centers d(s, c) {c ∈ C}
	And C := C ∪ {s}.

	take any uncovered site s ∈ S and put this site s in C
```

<!-- Next time we will get an algo for the general set covering -->
<!-- problem. Many problems can be reduced to set covering so it's -->
<!-- useful to know how to approx set covering. -->
