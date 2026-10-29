# quantum-computing-complexity-theory-and-algorithmics (verified cards)

Cards kept after the 2026W check against the downloaded lecture material. Unverified cards and prose were removed.

## A01-asymptotics-and-recurrences

```card id=qc-alg1-blocking-pair
Stable matching: when does a pair $\{u,w\}\notin M$ block the matching $M$?
---
When both would rather be together: $u$ is unmatched or prefers $w$ to $M(u)$, and $w$ is unmatched or prefers $u$ to $M(w)$. $M$ is stable if no pair blocks it.
```

```card id=qc-alg1-gale-shapley
Gale-Shapley (propose and reject), one round.
---
Take a free man $u$; he proposes to the best woman $w$ still on his list. If $w$ is matched, her partner becomes free again; $w$ is matched to $u$, and every man she ranks below $u$ is deleted from her list (and she from theirs). Repeat until no free man has a non-empty list.
```

```card id=qc-alg1-gs-runtime
Running time of Gale-Shapley with $n$ men and $n$ women, and what makes it possible?
---
$O(n^2)$: at most $n^2$ proposals, each $O(1)$ with a queue of free men, rank and inverse rank arrays per person (where $x$ stands in $v$'s list, and who stands at position $i$), partner arrays and for each man a pointer to the next woman to try.
```

```card id=qc-alg1-gs-termination
Why does Gale-Shapley stop after at most $n^2$ iterations of its loop?
---
Use as measure the number of man-woman pairs already considered: every proposal uses up one pair, since a man never proposes to the same woman twice (if she later takes someone better, the pair is deleted), and there are only $n^2$ pairs. Queue length and matching size do not change strictly in every iteration, so they give no bound.
```

```card id=qc-alg1-gs-stable
Proof idea that Gale-Shapley outputs a stable matching.
---
Assume $\{u,w\}$ blocks the output. Whether $u$ ended unmatched (his list ran empty) or prefers $w$ to his partner, he reached $w$ or $w$ was deleted from his list; both only happen because $w$ got a proposal from someone she likes more than $u$, and her partner only improves from then on. So $w$ does not prefer $u$: contradiction.
```

```card id=qc-alg1-o-notation-abuse
Why is $T(n)=O(f(n))$ an abuse of notation, and what is the cleaner form?
---
The equals sign is not symmetric or transitive here: $5n^3=O(n^3)$ and $3n^2=O(n^3)$, yet $5n^3\ne 3n^2$. Better: $T(n)\in O(f(n))$, a set of functions.
```

```card id=qc-alg1-additivity
Additivity rules for $O$, $\Omega$ and $\Theta$.
---
If $f=O(h)$ and $g=O(h)$ then $f+g=O(h)$; the same for $\Omega$. If $f=\Theta(h)$ and $g=O(h)$ then $f+g=\Theta(h)$. Also transitivity holds for each of $O$, $\Omega$, $\Theta$.
```

```card id=qc-alg1-largest-gap
Largest empty interval among $n$ arrival time stamps: algorithm and running time.
---
Sort the time stamps, then scan once and take the largest gap between neighbours: $O(n\log n)$, dominated by sorting.
```

```card id=qc-alg1-max-indset-brute
Brute-force running time for maximum independent set.
---
Try all $2^n$ vertex subsets, each checked in $O(n^2)$: $O(2^n n^2)$, exponential.
```

```card id=qc-alg1-roommates
Does every stable roommates instance have a stable matching?
---
No. Three people who rank each other cyclically and all rank a fourth last: whoever is paired with the fourth forms a blocking pair with someone. Existence is still decidable in polynomial time (Irving 1985).
```

```card id=qc-alg1-five-problems
The five representative problems and their complexity.
---
Interval scheduling: greedy $O(n\log n)$. Weighted interval scheduling: DP $O(n\log n)$. Bipartite matching: via max flow (general graphs: Edmonds' blossom). Independent set: NP-complete. Competitive facility location: PSPACE-complete.
```

```card id=qc-alg1-fib-naive
Running time of the naive recursive Fibonacci algorithm, and the fix.
---
$T(n)=T(n-1)+T(n-2)+c$, so $T(n)=\Theta(F_n)=\Theta(2^{0.694n})$; the tree has between $2^{n/2}$ and $2^n$ nodes. Storing results in a table $f[0..n]$ gives $O(n)$ additions.
```

```card id=qc-alg1-poly-time
Definition: an algorithm runs in polynomial time.
---
There are constants $c,d>0$ such that on every input of size $N$ it takes at most $cN^d$ steps. Equivalently: doubling the input size slows it down by at most a constant factor ($2^d$).
```

```card id=qc-alg1-big-o
Define $T(n)=O(f(n))$, $\Omega(f(n))$ and $\Theta(f(n))$.
---
$O$: there are $c>0$, $n_0\ge 0$ with $T(n)\le c f(n)$ for all $n\ge n_0$. $\Omega$: same with $T(n)\ge c f(n)$. $\Theta$: both.
```

```card id=qc-alg1-typecheck
Why is "every comparison sort needs at least $O(n\log n)$ comparisons" a meaningless statement?
---
$O$ is an upper bound, so "at least $O(\cdot)$" says nothing. A lower bound is written with $\Omega$: $\Omega(n\log n)$ comparisons.
```

```card id=qc-alg1-log-poly-exp
How do logarithms, polynomials and exponentials compare asymptotically?
---
$\log n = O(n^x)$ for every $x>0$; $n^d = O(r^n)$ for every $r>1$ and $d>0$. The base of a logarithm does not matter: $O(\log_a n)=O(\log_b n)$.
```

```card id=qc-alg1-k-indset
Brute-force running time for "is there an independent set of size $k$" with constant $k$?
---
Check all $\binom nk \le n^k/k!$ subsets, each in $O(k^2)$: $O(k^2 n^k/k!) = O(n^k)$. Polynomial for fixed $k$, but useless for $k=17$.
```

## A02-graphs

```card id=qc-alg2-matrix-vs-list
Adjacency matrix vs adjacency list: space, edge test, listing all edges.
---
Matrix: $\Theta(n^2)$ space, edge test $\Theta(1)$, all edges $\Theta(n^2)$. List: $\Theta(n+m)$ space, edge test $O(\deg u)$, all edges $\Theta(n+m)$. Sparse graphs ($m=O(n)$) favour lists.
```

```card id=qc-alg2-tree-two-of-three
Characterise trees among undirected graphs on $n$ vertices.
---
Any two of these imply the third: $G$ is connected; $G$ has no cycle; $G$ has $n-1$ edges.
```

```card id=qc-alg2-bfs-layers
BFS tree property for an edge $\{x,y\}$ of $G$?
---
The BFS levels of $x$ and $y$ differ by at most 1.
```

```card id=qc-alg2-bfs-time
Why does BFS run in $O(n+m)$ with adjacency lists?
---
Each vertex enters the queue at most once, and scanning $u$ costs $O(\deg u)$; $\sum_u \deg u = 2m$, since every edge is counted once at each end.
```

```card id=qc-alg2-bipartite
When is a graph bipartite, and how do you test it in linear time?
---
Iff it has no odd-length cycle. Run BFS from $s$: if no edge joins two vertices of the same layer, colour layers by parity; otherwise that edge plus the two tree paths to the lowest common ancestor form an odd cycle of length $2(j-i)+1$.
```

```card id=qc-alg2-strong-connectivity
How do you decide in $O(n+m)$ whether a digraph is strongly connected?
---
Pick any $s$. BFS from $s$ in $G$ and BFS from $s$ in $G^{\mathrm{rev}}$ (all arcs reversed). Strongly connected iff both reach every vertex, because $u\to s\to v$ then exists for all $u,v$.
```

```card id=qc-alg2-dag-topo
Which directed graphs have a topological order?
---
Exactly the DAGs. A cycle would need its lowest-indexed vertex to come after its predecessor on the cycle; conversely every DAG has a source, which can go first, and induction finishes.
```

```card id=qc-alg2-dag-source
Why does every DAG have a source (a vertex with no in-arc)?
---
If every vertex had an in-arc, walking backwards along in-arcs from any vertex would never stop, so some vertex repeats and the walk between its two visits is a directed cycle.
```

```card id=qc-alg2-topo-time
How is topological sorting done in $O(n+m)$?
---
Count the remaining in-arcs of each vertex and keep the set of vertices with count 0. Repeatedly remove one, output it, and decrement its out-neighbours' counts, adding those that reach 0.
```

```card id=qc-alg2-path-cycle-length
How are the length of a path and of a cycle measured in the lecture?
---
Path $(v_1,\dots,v_k)$: number of vertices minus one, so $k-1$ edges. Cycle $(v_1,\dots,v_k)$ with $v_1=v_k$, $k>2$ and the first $k-1$ vertices distinct: its length is the number of its vertices, which equals its number of edges.
```

```card id=qc-alg2-bfs-distance
What exactly does layer $L_i$ of BFS from $s$ contain, and how do you read off an $s$-$t$ path?
---
All vertices at distance exactly $i$ from $s$. There is an $s$-$t$ path iff $t$ lies in some layer; storing for each new vertex the vertex it was found from (parent array) and following it back gives a shortest $s$-$t$ path.
```

```card id=qc-alg2-cc-algorithm
Generic algorithm for the connected component of $s$, and why it is correct.
---
Start with $R=\{s\}$ and add $v$ while some edge $\{u,v\}$ has $u\in R$, $v\notin R$; return $G[R]$. Every added vertex has a path from $s$ (induction). Nothing is missing: a path from $s$ to a vertex outside $R$ would have a first edge leaving $R$, which the loop would still add. BFS or DFS implement it in $O(n+m)$.
```

```card id=qc-alg2-dfs
DFS: idea, running time, and what it does not give you.
---
From $s$, follow one unvisited neighbour as deep as possible and backtrack when stuck (recursive call per unvisited neighbour). $O(n+m)$ with adjacency lists. Unlike BFS it need not find shortest paths.
```

```card id=qc-alg2-weak-strong
Strongly vs weakly connected digraph.
---
Strongly connected: every two vertices are mutually reachable by directed paths. Weakly connected: the underlying undirected graph is connected.
```

```card id=qc-alg2-induced-subgraph
Subgraph induced by a vertex set $V'$, and by an edge set $E'$.
---
$G[V']$ has vertices $V'$ and every edge of $G$ with both ends in $V'$. $G[E']$ has edges $E'$ and exactly the vertices that are endpoints of edges in $E'$.
```

## A03-greedy

```card id=qc-alg3-interval-scheduling
Interval scheduling: which greedy order is optimal, and which natural orders are not?
---
Earliest finish time first is optimal. Earliest start time, fewest conflicts and shortest interval all fail on small counterexamples.
```

```card id=qc-alg3-interval-proof
Proof idea that earliest-finish-first is optimal for interval scheduling.
---
Take an optimum agreeing with the greedy on the first $r$ jobs, $r$ maximal. The greedy's job $i_{r+1}$ finishes no later than the optimum's $j_{r+1}$, so swapping it in keeps the solution feasible and optimal but longer in agreement: contradiction.
```

```card id=qc-alg3-depth
Interval partitioning: lower bound on the number of rooms, and why greedy by start time meets it.
---
The depth (maximum number of intervals containing one point). Greedy opens room $d$ only for a lecture that conflicts with all $d-1$ open rooms; those lectures started earlier and are still running, so $d$ intervals overlap right after its start.
```

```card id=qc-alg3-heap-array
Binary min-heap in an array $H[1..n]$: where are parent and children of $i$, and how high is it?
---
Parent $\lfloor i/2\rfloor$, children $2i$ and $2i+1$; height $\lfloor\log_2 n\rfloor$. Heap order: every node's priority is at most its children's, so the minimum is at the root.
```

```card id=qc-alg3-heap-ops
Binary heap: how do insert and extract-min work, and what do they cost?
---
Insert: append at the end and swap with the parent while smaller (sift up). Extract-min: remove the root, move the last element there and swap with the smaller child while larger (sift down). Both $O(\log n)$.
```

```card id=qc-alg3-dijkstra
Dijkstra: invariant and running time with a binary heap.
---
For every explored $u\in S$, $d(u)$ is the shortest $s$-$u$ distance (non-negative arc lengths). Next vertex: smallest $\min_{(u,v),u\in S} d(u)+\ell_{uv}$. $O((n+m)\log n)$.
```

```card id=qc-alg3-cut-property
MST cut property (distinct edge costs).
---
For any vertex set $S$ (non-empty, not all of $V$), the cheapest edge with exactly one endpoint in $S$ belongs to the MST. Exchange: otherwise adding it closes a cycle that crosses the cut a second time at a dearer edge $f$; swap $f$ out.
```

```card id=qc-alg3-cycle-property
MST cycle property (distinct edge costs).
---
The most expensive edge $f$ on any cycle $C$ is in no MST. Exchange: removing $f$ from a tree splits it into a cut that $C$ crosses again at a cheaper edge $e$; adding $e$ gives a cheaper spanning tree.
```

```card id=qc-alg3-prim-kruskal
Prim vs Kruskal: rule and running time.
---
Prim: grow one tree from a root, always adding the cheapest edge leaving it (cut property); $O(n^2)$ with an array, $O(m\log n)$ with a heap. Kruskal: edges by increasing cost, skip those closing a cycle (cycle property), components in union-find; $O(m\log n)$ for the sort (as $m\le n^2$, $\log m=O(\log n)$) plus $O(m\,\alpha(m,n))$ for the union-find operations.
```

```card id=qc-alg3-cayley
How many spanning trees does $K_n$ have?
---
$n^{n-2}$ (Cayley), which is why MST by enumeration is hopeless.
```

```card id=qc-alg3-lateness
Minimising maximum lateness on one machine: which greedy rule, and why not the other two?
---
Earliest due time first. Shortest processing time ignores deadlines ($t=(1,10)$, $d=(100,10)$); smallest slack $d_j-t_j$ fails on $t=(1,10)$, $d=(2,10)$.
```

```card id=qc-alg3-inversion-swap
Key lemma in the earliest-due-time proof.
---
In a schedule without idle time, an inversion ($d_i<d_j$ but $j$ before $i$) implies an adjacent one, and swapping two adjacent inverted jobs removes one inversion without increasing the maximum lateness. So an optimum with no inversions exists, which is the greedy schedule.
```

```card id=qc-alg3-proof-strategies
Standard ways to prove a greedy algorithm optimal.
---
Greedy stays ahead (after every step at least as good as any other solution); structural bound (every solution needs at least $X$, greedy achieves $X$); exchange argument (turn any optimum into the greedy solution step by step without loss). Matroids are named as the general structure under which a greedy algorithm is optimal.
```

```card id=qc-alg3-heap-update
Binary heap: how does decreasing a priority work (as in Dijkstra and Prim), and how is the element found?
---
Set the smaller priority and sift the element up like an insert, $O(\log n)$. To find it in the array, keep its position $pos[v]$ and update it on every swap.
```

```card id=qc-alg3-heap-smaller-child
Why does extract-min swap the moved element with the smaller child?
---
The smaller child becomes the parent of the other child, so the heap order holds again at that node.
```

```card id=qc-alg3-dijkstra-chain
Dijkstra correctness: the inequality chain for an arbitrary $s$-$v$ path $P$ whose first arc leaving $S$ is $(x,y)$, and what justifies each step.
---
$\ell(P)\ge\ell(P')+\ell(x,y)\ge \mathrm{dist}(x)+\ell(x,y)\ge\pi(y)\ge\pi(v)$, with $P'$ the part of $P$ up to $x$. Steps: non-negative lengths; induction hypothesis ($x\in S$); definition of $\pi(y)$; Dijkstra picked $v$ rather than $y$.
```

```card id=qc-alg3-dijkstra-array-heap
Dijkstra's running time with an array versus a binary heap as priority queue.
---
Array: insert and update $O(1)$, extract-min $O(n)$, total $O(n^2)$. Binary heap: every operation $O(\log n)$, $n$ extract-mins and at most $m$ updates, total $O((n+m)\log n)$. The array wins on dense graphs.
```

```card id=qc-alg3-cutset
Cut and cutset in the MST chapter.
---
A cut is a non-empty vertex set $A\subsetneq V$; its cutset $D(A,V\setminus A)$ is the set of edges with exactly one endpoint in $A$. A cycle that uses one edge of a cutset must cross back and so uses a second one; the cut and cycle property proofs swap these two edges.
```

```card id=qc-alg3-reverse-delete
Reverse-delete algorithm for MST.
---
Start with all edges; go through them by non-increasing cost and delete an edge unless that disconnects the graph. Each deleted edge is the most expensive on some remaining cycle, so the cycle property justifies it.
```

```card id=qc-alg3-kruskal-correct
Why is Kruskal correct?
---
Edge $e=\{u,v\}$ in increasing cost order. If it closes a cycle, it is the most expensive edge on that cycle: discard by the cycle property. Otherwise take $S$ = component of $u$; $e$ is the cheapest edge leaving $S$ (cheaper ones were already handled), so it is in the MST by the cut property.
```

```card id=qc-alg3-tiebreak
How do Prim and Kruskal cope with equal edge costs?
---
Perturb costs by tiny amounts (for integer costs, add $i/n^2$ to edge $e_i$); both algorithms only compare costs, so the MST for the perturbed costs is an MST for the original ones. In practice: compare by cost, break ties by edge index.
```

## A04-divide-and-conquer

```card id=qc-alg4-mergesort-rec
Merge sort: recurrence for the number of comparisons, its solution, and two ways to prove it.
---
$T(n)\le T(\lceil n/2\rceil)+T(\lfloor n/2\rfloor)+O(n)$, so $T(n)=O(n\log n)$.
Recursion tree: $\log_2 n$ levels, each costing $cn$. Induction on $k$ with $n=2^k$: $T(2^{k+1})=2T(2^k)+O(2^{k+1})=O(2^{k+1}(k+1))$.
```

```card id=qc-alg4-master-restricted
Restricted master theorem for $T(n)=x\,T(\lceil n/y\rceil)+O(n^d)$, with $x>0$, $y>1$, $d\ge0$.
---
$d>\log_y x$: $O(n^d)$. $d=\log_y x$: $O(n^d\log n)$. $d<\log_y x$: $O(n^{\log_y x})$.
```

```card id=qc-alg4-master-log
Master theorem extension: $T(n)=x\,T(\lceil n/y\rceil)+O(n^{\log_y x}\log^k n)$ with $k>-1$.
---
$T(n)=O(n^{\log_y x}\log^{k+1} n)$: one more log factor. Example: $2T(n/2)+O(n\log n)=O(n\log^2 n)$.
```

```card id=qc-alg4-inversions-example
Count the inversions of $1,5,4,8,10,2 \mid 6,9,12,11,3,7$ by divide and conquer.
---
Left half 5 (5-4, 5-2, 4-2, 8-2, 10-2), right half 8, across the halves 9.
Across: merge $1,2,4,5,8,10$ with $3,6,7,9,11,12$, adding $4+2+2+1+0+0$. Total 22.
```

```card id=qc-alg4-ranking-distance
Why count inversions, and what related problem is hard?
---
The number of inversions between two rankings is a distance (non-negative, symmetric, triangle inequality), used to compare tastes or rankings.
Finding one ranking with least total distance to a set of rankings (Kemeny ranking) is NP-hard.
```

```card id=qc-alg4-closest-1d-quadrants
Closest pair: how is the 1D case solved, and why not split the plane into quadrants?
---
1D: sort and compare consecutive points, $O(n\log n)$.
Quadrants cannot guarantee about $n/4$ points per piece; a vertical line through the median $x$ gives an even split.
```

```card id=qc-alg4-strip-11
Closest pair: why is it enough to compare each strip point with the next 11 in $y$-order?
---
Boxes of side $\delta/2$ hold at most one point each (two would be closer than $\delta$ within one half). The strip is 4 boxes wide, so 12 or more positions later means two full rows lie in between, and the $y$-distance alone is $\ge\delta$.
```

```card id=qc-alg4-split-inversions
In merge-and-count, what is added to the count when $R[j]$ is emitted before $L[i]$, and why?
---
$|L|-i$: the remaining left elements are all larger than $R[j]$ and precede it in the input.
Each split inversion is counted once, when its right element is emitted.
```

```card id=qc-alg4-inversions-sorted
Why must the recursive calls of inversion counting return their halves sorted?
---
Counting split inversions on unsorted halves is $\Theta(n^2)$ again.
Sorting does not change the split count, which depends only on which elements are in $L$ vs $R$.
```

```card id=qc-alg4-closest-time
True or false: re-sorting the strip by $y$ at every level keeps closest pair at $O(n\log n)$.
---
False: that gives $O(n\log^2 n)$.
Carrying the pre-sorted list $P_y$ down by filtering keeps $T(n)=2T(n/2)+O(n)=O(n\log n)$.
```

## A05-dynamic-programming

```card id=qc-alg5-weighted-intervals
Weighted interval scheduling: state the recurrence and define $p(j)$.
---
$\mathrm{OPT}(j)=\max(v_j+\mathrm{OPT}(p(j)),\ \mathrm{OPT}(j-1))$, $\mathrm{OPT}(0)=0$.
$p(j)$: largest $i<j$ with $f_i\le s_j$, else 0 (intervals sorted by finish).
```

```card id=qc-alg5-memo-vs-naive
Why is naive recursion for weighted intervals exponential but memoisation $O(n)$ after sorting?
---
Naive: two branches per level, Fibonacci-like ($p(j)=j-2$).
Memoised: only $n+1$ distinct subproblems, each $O(1)$. Total $O(n\log n)$ with sorting and computing $p$ by binary search.
```

```card id=qc-alg5-wis-greedy
Why does earliest-finish-first greedy fail for weighted interval scheduling?
---
It ignores weights: a light short job finishing first can block one heavy overlapping job.
It is only optimal when all weights are equal.
```

```card id=qc-alg5-wis-recovery
The DP table $T$ for weighted intervals holds only values. How is an optimal job set recovered, and at what cost?
---
Walk back from $j=n$: if $v_j+T[p(j)]>T[j-1]$ take $j$ and jump to $p(j)$, else go to $j-1$.
At most $n$ steps, so $O(n)$ after the table; $O(n\log n)$ overall.
```

```card id=qc-alg5-knapsack
0/1 knapsack (sizes $s_i$, capacity $S$): recurrence for $\mathrm{OPT}(i,s)$ when $s_i\le s$, and the running time.
---
$\max(\mathrm{OPT}(i-1,s),\ v_i+\mathrm{OPT}(i-1,s-s_i))$; if $s_i>s$ only the first; $\mathrm{OPT}(0,s)=0$.
Fill an $n$ by $S$ table: $\Theta(nS)$.
```

```card id=qc-alg5-knapsack-state
Why is $\mathrm{OPT}(i)$ = best value from items $1..i$ not enough for knapsack?
---
Whether item $i$ fits depends on how much room the earlier picks used, which $\mathrm{OPT}(i)$ forgets.
Fix: add the remaining capacity as a second parameter, $\mathrm{OPT}(i,s)$.
```

```card id=qc-alg5-knapsack-greedy
Does greedy by value-to-size ratio solve knapsack? Give the idea of a counterexample.
---
No. Capacity 6, items (size, value) (2,3), (3,4), (4,5): ratio greedy takes the first two for 7, but the first and third give 8.
```

```card id=qc-alg5-partition
Show that integer partition is a special case of knapsack.
---
Items with $s_i=v_i=a_i$, capacity $S=\frac12\sum a_i$.
A fair split exists iff the optimal value equals $S$.
```

```card id=qc-alg5-knapsack-restricted
Knapsack restricted cases: all sizes equal, and all value-to-size ratios equal. Easy or hard?
---
Equal sizes (or equal values): polynomial, sort and take the best.
Equal ratios: still NP-hard (it contains partition).
```

```card id=qc-alg5-pseudo-poly
True or false: the $O(nW)$ DP shows knapsack is in P.
---
False. Input size is $\Theta(n\log W)$ bits; $W$ is exponential in that.
$O(nW)$ is pseudo-polynomial; the decision version of knapsack is NP-complete.
```

```card id=qc-alg5-bellman-ford
Bellman-Ford: meaning of $\mathrm{OPT}(i,v)$, number of rounds, running time.
---
Shortest $v$-$t$ path with at most $i$ arcs (destination $t$); $\mathrm{OPT}(i,v)=\min(\mathrm{OPT}(i-1,v),\min_{(v,w)}\mathrm{OPT}(i-1,w)+c_{vw})$.
$n-1$ rounds (shortest paths are simple without negative cycles), each $O(m)$: $\Theta(mn)$ time, $\Theta(n^2)$ table.
```

```card id=qc-alg5-bf-one-array
How does Bellman-Ford get by with $O(n)$ memory, and what invariant keeps it correct?
---
Keep one array $M[v]$, update in place, only relax arcs into vertices whose value changed, stop when a round changes nothing.
$M[v]$ is always the length of some real $v$-$t$ path and after $i$ rounds at most the best with $\le i$ arcs.
```

```card id=qc-alg5-no-dijkstra
Shortest paths with negative arc costs: why not Dijkstra, and why not add a constant to every arc?
---
Dijkstra may settle a vertex before a later negative arc makes a cheaper route.
A uniform shift adds the constant once per arc, so it favours paths with few arcs and can change the shortest path.
```

```card id=qc-alg5-negative-cycle
How does Bellman-Ford detect and find a negative cycle?
---
Add a new target $\tau$ with 0-cost arcs from every vertex; run $n$ rounds towards $\tau$.
Negative cycle iff $\mathrm{OPT}(n,v)<\mathrm{OPT}(n-1,v)$ for some $v$; extract it from that $v$-$\tau$ path. $O(mn)$.
```

```card id=qc-alg5-negcycle-lemmas
Negative-cycle test: why do values stay fixed after round $n-1$ without a negative cycle, and why does some value drop in round $n$ with one?
---
Without: a path with $\ge n$ arcs has a cycle of cost $\ge0$; removing it does not raise the cost.
With: if no value dropped, $\mathrm{OPT}(n-1,v)\le\mathrm{OPT}(n-1,w)+c_{vw}$ on every cycle arc; summing gives cycle cost $\ge0$, contradiction.
```

```card id=qc-alg5-arbitrage
How does currency arbitrage become a negative-cycle question?
---
Arc cost $-\log(\text{rate})$. A cycle with rate product above 1 has negative cost sum.
```

```card id=qc-alg5-alignment
Sequence alignment recurrence for $\mathrm{OPT}(i,j)$ and base cases.
---
$\min(\alpha_{x_iy_j}+\mathrm{OPT}(i-1,j-1),\ \delta+\mathrm{OPT}(i-1,j),\ \delta+\mathrm{OPT}(i,j-1))$.
$\mathrm{OPT}(i,0)=i\delta$, $\mathrm{OPT}(0,j)=j\delta$; $O(mn)$ time.
```

```card id=qc-alg5-hirschberg
How does Hirschberg get linear space while keeping $O(mn)$ time?
---
Value alone: keep only the previous row, $O(m+n)$ space.
Alignment: split $Y$ in the middle, find the best split index of $X$ with forward and backward two-row passes, recurse on both halves (D&C plus DP). Area halves per level: $O(mn)$ time.
```

```card id=qc-alg5-edit-distance
Edit distance as an alignment problem: which costs, and what is it for kitten and sitting?
---
Gap cost $\delta=1$, mismatch cost 1 for different letters, 0 for equal ones.
kitten to sitting: 3 (substitute k and e, insert g).
```

```card id=qc-alg5-techniques
Name the three DP techniques of the chapter with their example problems.
---
Binary choice: weighted interval scheduling.
Adding a new variable: knapsack.
Combining with divide and conquer: sequence alignment (Hirschberg).
```

## A06-network-flow

```card id=qc-alg6-cut-capacity
Define the capacity of a cut $(A,B)$. Do edges from $B$ to $A$ count?
---
$c(A,B)=\sum_{e\text{ out of }A}c(e)$. No, only $A\to B$ edges.
Flow value across a cut subtracts backward flow: $v(f)=f^{\rm out}(A)-f^{\rm in}(A)$.
```

```card id=qc-alg6-residual
What edges does the residual graph $G_f$ contain?
---
Forward $(u,v)$ with capacity $c(e)-f(e)$ if $f(e)<c(e)$; backward $(v,u)$ with capacity $f(e)$ if $f(e)>0$. At most $2m$ edges.
```

```card id=qc-alg6-weak-duality
Prove $v(f)\le c(A,B)$ for every flow $f$ and every $s$-$t$ cut $(A,B)$.
---
Flow value lemma: $v(f)=f^{\rm out}(A)-f^{\rm in}(A)$ (sum conservation over $A$).
Drop the subtracted term, then use $f(e)\le c(e)$ on arcs out of $A$. Equality certifies max flow and min cut.
```

```card id=qc-alg6-greedy-stuck
Why does plain greedy augmentation (only arcs with $f(e)<c(e)$) not find a max flow?
---
It can never undo flow, so an early bad path can block the optimum (worked example: path $s,u,v,t$ first gives 20, the maximum is 30).
Residual backward arcs fix this.
```

```card id=qc-alg6-integrality
State the integrality theorem for max flow and why it holds.
---
Integer capacities: some max flow has all $f(e)$ integral.
Ford-Fulkerson keeps flows and residual capacities integral and terminates, so its output is such a flow.
```

```card id=qc-alg6-maxflow-mincut
State the max-flow/min-cut three-way equivalence.
---
(1) $f$ is maximum; (2) no augmenting path in $G_f$; (3) some cut has $v(f)=c(A,B)$.
Hence $\max v(f)=\min c(A,B)$.
```

```card id=qc-alg6-find-min-cut
After Ford-Fulkerson terminates, how is a minimum cut obtained and why is it tight?
---
$A^*$ = vertices reachable from $s$ in $G_f$ (one BFS).
Edges out of $A^*$ are saturated, edges into $A^*$ carry 0, so $v(f)=c(A^*,B^*)$.
```

```card id=qc-alg6-ff-time
Running time of Ford-Fulkerson with integer capacities. Is it polynomial?
---
$O(mC^*)$, $C^*=\sum_{e\text{ out of }s}c(e)\le nC$, so $O(mnC)$. No, pseudo-polynomial (input size uses $\log C$).
The diamond with middle edge 1 needs $2C$ iterations.
```

```card id=qc-alg6-irrational
True or false: Ford-Fulkerson always terminates with a maximum flow.
---
False for irrational capacities: it may not terminate, although max-flow/min-cut still holds. Integrality and termination are about integer inputs.
```

```card id=qc-alg6-scaling-lemmas
Capacity scaling: number of phases, gap to the optimum after a phase, augmentations per phase.
---
$1+\lceil\log_2C\rceil$ phases ($\Delta$ halves). After a $\Delta$-phase $v(f^*)\le v(f)+m\Delta$ (cut at the set reachable in $G_f(\Delta)$).
Next phase: each augmentation adds $\ge\Delta/2$, so $\le2m$ of them. Total $O(m\log C)$ augmentations.
```

```card id=qc-alg6-bipartite
Reduce bipartite matching to max flow. Running time?
---
$s\to$ every $\ell\in L$, $L\to R$ edges, every $r\to t$, all capacity 1; integral max flow = max matching.
FF with $C^*\le n$: $O(mn)$.
```

```card id=qc-alg6-hall
State Hall's marriage theorem and sketch the flow proof of the hard direction.
---
$|U|=|W|$: perfect matching iff $|N(S)|\ge|S|$ for all $S\subseteq U$.
No perfect matching: min cut $<n$ with no $U_A\to W_B$ arc, so $|U_B|+|W_A|<n$, hence $|U_A|>|W_A|\ge|N(U_A)|$.
```

```card id=qc-alg6-menger
How are edge-disjoint $s$-$t$ paths found with flow, and why is the count exact?
---
Unit capacity on every arc; max flow value = max number of edge-disjoint paths (Menger).
Paths give a flow; an integral 0-1 flow of value $k$ splits into $k$ edge-disjoint paths by following flow arcs from $s$.
```

```card id=qc-alg6-circulation
When is a circulation with demands $d(v)$ feasible?
---
$\sum_v d(v)=0$ and the max flow with super-source/sink saturates all source edges (value $D=\sum_{d(v)>0}d(v)$).
Lower bounds: push $\ell(e)$ first, adjust demands and capacities.
```

```card id=qc-alg6-circulation-cut
What certifies that no circulation with demands exists?
---
A partition $(A,B)$ with $\sum_{v\in B}d(v)>c(A,B)$: demand in $B$ exceeds what can flow in.
```

```card id=qc-alg6-multicommodity
Multicommodity flow versus a single circulation: complexity?
---
Each sink must be fed by its own source. Integral version NP-hard; fractional version solvable by LP.
```

```card id=qc-alg6-survey
Survey design as a flow problem: which network?
---
$s\to$ product $[p_i,p'_i]$, product $\to$ owning consumer $[0,1]$, consumer $\to t$ $[c_j,c'_j]$, $t\to s$ unbounded.
Feasible survey iff an integral circulation with lower bounds exists.
```

```card id=qc-alg6-image-segmentation
Image segmentation as min cut: which arcs and capacities?
---
$s\to i$ with $a_i$, $i\to t$ with $b_i$, each neighbour pair as two opposite arcs with $p_{ij}$.
A cut costs $\sum_{B}a+\sum_{A}b+\sum_{\text{separated}}p$, the penalty to minimise; $A$ = foreground.
```

## B01-models-and-np-completeness

```card id=qc-cx1-multitape
Overhead of simulating a $k$-tape TM with time $T(n)$ on a 1-tape TM?
---
$O(T(n)^2)$ time; space preserved up to a constant.
So polynomial time does not depend on the number of tapes.
```

```card id=qc-cx1-np-verifier
Define NP via verifiers.
---
$x\in L\iff\exists c\in\{0,1\}^{p(|x|)}:V(x,c)=1$, with polynomial $p$ and polynomial-time $V$.
In the lectures: a certificate relation $R$ that is polynomially balanced ($|c|\le|x|^k$) and decidable in polynomial time, with $x$ a yes-instance iff some $(x,c)\in R$.
```

```card id=qc-cx1-ntm-verifier
Why does an NTM definition of NP give a verifier?
---
The certificate is a polynomially long accepting computation of the NTM on $x$ (equivalently the sequence of nondeterministic choices).
The verifier checks it step by step in polynomial time.
```

```card id=qc-cx1-karp
Define a Karp reduction $A\le_pB$. How does it differ from a Cook reduction?
---
Polynomial-time $f$ with $x\in A\iff f(x)\in B$: one query, answer taken as is.
Cook/Turing reductions allow many adaptive oracle queries.
```

```card id=qc-cx1-direction
To prove a new problem $B$ NP-hard, which direction must the reduction go?
---
Known-hard $A\le_pB$: map $A$-instances to $B$-instances.
Reducing $B$ to SAT only shows $B\in$ NP.
```

```card id=qc-cx1-sat-3sat
True or false: the SAT to 3SAT clause splitting produces an equivalent formula.
---
False: equisatisfiable only. It adds fresh variables $y_i$; the yes/no answer is preserved, not the set of models.
```

```card id=qc-cx1-3sat-is
3SAT to Independent Set: construct $G$ and $k$.
---
A triangle per clause on its literal occurrences, edges between all $x$ and $\neg x$ occurrences; $k=m$ (clauses).
VC then with $|V|-k=2m$.
```

```card id=qc-cx1-halting-diag
Proof idea: the Halting problem is undecidable.
---
Assume a decider $H(\Pi,I)$. Build $D(\Pi)$: loop forever if $H(\Pi,\Pi)$ says halts, else stop.
Run $D$ on its own code: it halts iff it does not halt. Contradiction.
```

```card id=qc-cx1-undecidable-reduction
How do you prove a new problem $P$ undecidable, and what may the reduction cost?
---
Reduce Halting to $P$: a decider for $P$ plus the reduction would decide Halting.
The reduction only has to be computable, no time or space bound.
```

```card id=qc-cx1-np-complete-in-p
Why does one NP-complete problem in P give P = NP?
---
Every $A\in$ NP reduces to it in polynomial time; compute the reduction, then run the polynomial algorithm.
So NP $\subseteq$ P, and P $\subseteq$ NP always.
```

```card id=qc-cx1-tm-quadruple
How do the 2026W lectures define a deterministic Turing machine?
---
$M=(K,\Sigma,\delta,s)$, $\delta:K\times\Sigma\to(K\cup\{h,\text{yes},\text{no}\})\times\Sigma\times\{\leftarrow,\rightarrow,-\}$.
One-sided tape starting with $\triangleright$ (never overwritten, cursor never left of it), then the input.
```

```card id=qc-cx1-two-sided
Simulating a two-way infinite tape on a one-sided one: cost and trick?
---
Same time $O(f(n))$. Store cell $z\ge0$ at position $2z+1$ and cell $z<0$ at $-2z$; each step costs a constant number of moves.
```

```card id=qc-cx1-palindrome
Palindromes: running time on a 1-string versus a 2-string TM?
---
1-string: $\Theta(n^2)$, erase first and last symbol and walk back each round.
2-string: $\Theta(n)$, copy the input, then compare forward against backward.
```

```card id=qc-cx1-ntm-asymmetry
When does an NTM accept and when does it reject?
---
Accept if some sequence of choices reaches yes; reject only if no sequence does.
Acceptance and rejection are asymmetric.
```

```card id=qc-cx1-ntime-exp
Deterministic simulation of an NTM running in time $f(n)$?
---
$\mathrm{NTIME}(f)\subseteq\bigcup_{c>1}\mathrm{TIME}(c^{f(n)})$: branching is bounded by a constant $d$, so at most $d^{f(n)}$ paths, explored one by one.
```

```card id=qc-cx1-cook-levin-atoms
Cook-Levin as proved in the 2026W lectures: which atoms and which groups of conjuncts?
---
Atoms $\mathit{symbol}_\sigma[\tau,\pi]$, $\mathit{cursor}[\tau,\pi]$, $\mathit{state}_s[\tau]$ for $\tau,\pi\le N=q(|x|)$.
Groups: initialisation, transition rules, uniqueness, inertia (cells off the cursor unchanged), acceptance (state yes at time $N$).
```

```card id=qc-cx1-validity
Complexity of VALIDITY, and why?
---
coNP-complete. $\varphi$ is valid iff $\neg\varphi$ is unsatisfiable, so VALIDITY and co-SAT reduce to each other.
```

```card id=qc-cx1-normal-form-easy
For which normal forms are SAT and VALIDITY easy?
---
SAT on DNF and VALIDITY on CNF are in P.
SAT on 3-CNF stays NP-complete, VALIDITY on 3-DNF stays coNP-complete.
```

```card id=qc-cx1-sat-cnf
Why not convert a formula to an equivalent CNF before reducing SAT, and what is used instead?
---
The equivalent CNF can be exponentially larger, e.g. for $(x_1\wedge y_1)\vee\dots\vee(x_n\wedge y_n)$.
Instead, a sat-equivalent rewrite: $(A\wedge B)\vee C$ becomes $(z\vee A)\wedge(z\vee B)\wedge(\neg z\vee C)$ with fresh $z$.
```

```card id=qc-cx1-clause-split
How is a long clause $l_1\vee l_2\vee l_3\vee R$ shortened towards 3-CNF?
---
Replace it by $(z\vee l_1\vee l_2)\wedge(\neg z\vee l_3\vee R)$ with fresh $z$, and repeat.
Set $z$ false if $l_1$ or $l_2$ is true, else true.
```

```card id=qc-cx1-is-clique-vc
How are INDEPENDENT SET, CLIQUE and VERTEX COVER reduced to each other?
---
$I$ is independent in $G$ iff $I$ is a clique in the complement graph.
$I$ is independent iff $V\setminus I$ is a vertex cover, so $K$ becomes $|V|-K$.
```

```card id=qc-cx1-coloring
Complexity of $k$-COLORABILITY?
---
NP-complete for every fixed $k\ge3$.
2-COLORABILITY is in P: colour one vertex, give neighbours the opposite colour, fail on a conflict.
```

```card id=qc-cx1-co-reduction
If $A$ reduces to $B$ by a many-one reduction, what about the complements?
---
The same function reduces co-$A$ to co-$B$: $x\in A\iff R(x)\in B$ is the same as $x\notin A\iff R(x)\notin B$.
```

```card id=qc-cx1-co-classes
Which classes are known to equal their co-class?
---
Deterministic ones, e.g. P = co-P (swap yes and no).
For nondeterministic classes this may fail: NP versus coNP is open, inequality believed.
```

```card id=qc-cx1-functional-complete
What does functional completeness of $\{\wedge,\vee,\neg\}$ say?
---
Every Boolean function $\{0,1\}^n\to\{0,1\}$ is expressed by a formula using only these connectives (e.g. read a DNF off the truth table).
```

## B02-pspace-and-exptime

```card id=qc-cx2-inclusions
State the basic inclusion chain for constructible $f\ge\log n$.
---
$\mathrm{DTIME}(f)\subseteq\mathrm{SPACE}(f)\subseteq\mathrm{NSPACE}(f)\subseteq\mathrm{DTIME}(2^{O(f)})$.
Last step: BFS on the $2^{O(f)}$-vertex configuration graph.
```

```card id=qc-cx2-tqbf-membership
Why is TQBF in PSPACE?
---
Recursive evaluation (nested loops, one per variable) tries $x_1=0$ then $1$, OR for $\exists$, AND for $\forall$, reusing space.
Polynomial space, $2^n$ time. The lectures (as QSAT) count $O(n)$ for the assignment, $O(n\log n)$ for the call stack of indices, linear space to evaluate the matrix.
```

```card id=qc-cx2-p-exptime
Prove P $\ne$ EXPTIME.
---
$\mathrm P\subseteq\mathrm{DTIME}(2^n)$ and $n2^n=o(2^{n^2})$.
So $\mathrm{DTIME}(2^n)\subsetneq\mathrm{DTIME}(2^{n^2})\subseteq\mathrm{EXPTIME}$.
```

```card id=qc-cx2-strict-chain
Which inclusion of NL $\subseteq$ P $\subseteq$ NP $\subseteq$ PSPACE is known to be strict?
---
None individually. NL $\ne$ PSPACE (Savitch plus space hierarchy) shows at least one is strict.
```

```card id=qc-cx2-games
Which games are PSPACE-complete and which EXPTIME-complete?
---
PSPACE: generalised Geography, Hex, Go with polynomial move bound, Reversi.
EXPTIME: exponentially long plays, e.g. generalised chess, Go with ko.
```

```card id=qc-cx2-chain-2026
State the inclusion chain from L to EXPSPACE and which separations are known.
---
L $\subseteq$ NL $\subseteq$ P $\subseteq$ NP $\subseteq$ PSPACE $\subseteq$ EXPTIME $\subseteq$ NEXPTIME $\subseteq$ EXPSPACE.
No single step is known strict; classes an exponential apart differ: L $\ne$ PSPACE, P $\ne$ EXPTIME, NP $\ne$ NEXPTIME.
```

```card id=qc-cx2-np-pspace
Why is NP $\subseteq$ PSPACE?
---
$\mathrm{NTIME}(f)\subseteq\mathrm{SPACE}(f)$: enumerate the candidate certificates (or branches) one after another, reusing the same space for each check.
```

```card id=qc-cx2-qsat
Define QSAT and state its complexity.
---
Given a CNF $\psi$, is $\exists x_1\forall x_2\exists x_3\cdots Qx_n\,\psi$ true (a QBF with strict alternation)?
PSPACE-complete.
```

```card id=qc-cx2-strategy-games
When is a two-player strategy game in PSPACE, and why is generalised Go only placed in EXPTIME?
---
When plays have polynomially bounded length: search the game tree depth first with polynomially deep nested loops.
Go on $n\times n$ boards has no polynomial bound on the length of a play.
```

```card id=qc-cx2-intractable
Intractable versus provably intractable: which complete problems are which?
---
NP-complete: no efficient algorithm known or expected, but not excluded.
EXPTIME-complete: no polynomial algorithm, provably, since P $\ne$ EXPTIME.
```

## B03-inside-p-l-and-nl

```card id=qc-cx3-logspace-tm
Define a log-space TM. Which tape counts toward space?
---
Read-only input tape, read-write work tapes, and (in the lectures) a write-only output tape; only the work tapes count.
$\mathrm L=\mathrm{SPACE}(\log n)$, $\mathrm{NL}=\mathrm{NSPACE}(\log n)$.
```

```card id=qc-cx3-nl-in-p
Why is NL $\subseteq$ P?
---
A log-space machine has polynomially many configurations.
Write down the configuration graph and run BFS from start to accept.
```

```card id=qc-cx3-reduction-type
Why must NL-completeness use log-space rather than polynomial-time reductions?
---
NL $\subseteq$ P, so under $\le_p$ every nontrivial problem in NL would be complete.
```

```card id=qc-cx3-path-nl
NL algorithm for PATH: what does it store, and why the counter?
---
Only the current vertex and a step counter ($2\log n$ bits), guessing a neighbour each step.
The counter forces every branch to terminate (paths need at most $n-1$ steps).
```

```card id=qc-cx3-l-memory
What may a log-space program store, and why is its running time polynomial?
---
A constant number of pointers, counters and Boolean flags.
Logarithmic work space gives only polynomially many configurations, so a halting run is polynomially long.
```

```card id=qc-cx3-find-node
Why is FIND-NODE (does a labelled tree contain label $n$) in L?
---
Traverse the tree with two node pointers (current, next) and one bit for the direction (down or up), checking each label.
```

```card id=qc-cx3-path-hardness
Idea of the NL-hardness of REACHABILITY, and what follows for NL versus P?
---
The configurations and transitions of an NL machine on $x$ form a graph; acceptance is a path from the start to an accepting configuration.
REACHABILITY is in P, so NL $\subseteq$ P.
```

## B04-circuit-complexity

```card id=qc-cx4-circuit-eval
Why is CIRCUIT EVAL (circuit plus input values, is the output true?) in P?
---
Evaluate bottom-up: a gate is computed once all its children are known, each gate once.
Linearly many rounds of polynomial work; linear time with suitable data structures.
```

```card id=qc-cx4-model-checking
Why is checking whether an assignment satisfies a propositional formula in P?
---
The formula is a circuit with one gate per subformula; this reduces it in polynomial time to CIRCUIT EVAL, which is in P.
```

## chapter 6 annotations, 2026W

```card id=qc-alg6-summary-simplex
In the lecturer's max-flow summary table, what is the 1951 simplex row and its worst-case bound?
---
A handwritten note on the slide points from simplex to "LP method", so it is the linear programming approach to max flow. The table gives $O(m^3C)$ (Dantzig).
```

```card id=qc-alg6-shortest-aug-path
Worst-case bound for augmenting along shortest paths (Dinitz, Edmonds-Karp, 1970)?
---
$O(m^2 n)$.
```

```card id=qc-alg6-cap-scaling-time
Worst-case bound for capacity scaling (Edmonds-Karp, 1972)?
---
$O(m^2\log C)$.
```

```card id=qc-alg6-orlin-2013
Which 2013 result gives $O(nm)$ worst case for max flow, and what does the compact-network variant give?
---
Orlin: the compact network alone gives $O(nm+m\log n)$; combined with King-Rao-Tarjan it gives $O(nm)$.
```

```card id=qc-alg6-latest-bound
Best worst-case bound for max flow in the lecture's summary table, and its method?
---
$m^{1+o(1)}\log C$, from 2022: an interior point method using min-ratio cycles.
```

```card id=qc-alg6-hopcroft-karp
Worst-case running time for bipartite maximum matching via shortest augmenting paths?
---
$O(m\sqrt n)$ (Hopcroft-Karp, 1973). Generic augmenting paths give $O(mn)$ here.
```

```card id=qc-alg6-matching-scaling
Running time of bipartite matching via capacity scaling, and why does it simplify?
---
$O(m^2\log C)$ with $C=1$ for unit capacities, so $O(m^2)$.
```

```card id=qc-alg6-blossom
Running time of Edmonds' blossom algorithm for general (non-bipartite) maximum matching, and the best known bound?
---
Blossom algorithm (Edmonds, 1965): $O(n^4)$. Best known: $O(m\sqrt n)$ (Micali-Vazirani, 1980). Non-bipartite structure is harder than the bipartite case, but understood via Tutte-Berge and Edmonds-Gallai.
```

## chapter 7, 2026W

```card id=qc-alg7-rho-approx
What does a $\rho$-approximation algorithm guarantee, and what is the proof obstacle?
---
It runs in polynomial time on every instance and returns a solution within ratio $\rho$ of the optimum. The obstacle: the bound must hold without knowing the optimum value, so the analysis compares the output with a computable bound on OPT.
```

```card id=qc-alg7-mvc-def
State the optimisation version of Minimum Vertex Cover.
---
Given an undirected graph, find a vertex set of minimum size such that every edge has at least one endpoint in it.
```

```card id=qc-alg7-maximal-matching
When is a matching maximal, and how does that differ from maximum?
---
Maximal: no matching properly contains it, so no edge can be added. Maximum: largest possible number of edges. Every maximum matching is maximal, but a maximal matching need not be maximum.
```

```card id=qc-alg7-mm-algo
What does MM-Algo output for vertex cover, and why is the output a cover?
---
Find a maximal matching and output the endpoints of all its edges. An edge not in the matching must share an endpoint with a matching edge (otherwise the matching could be extended), so it is covered.
```

```card id=qc-alg7-mm-bound
Why is any maximal matching at most the minimum vertex cover, and what approximation follows?
---
Matching edges share no endpoint, so any vertex cover needs a distinct vertex for each one: $|M|\le OPT$. MM-Algo outputs $2|M|$ vertices, a factor-2 approximation.
```

```card id=qc-alg7-mvc-tight
What do the slides say about the factor 2 for minimum vertex cover?
---
They remark that the 2-approximation is tight.
```

```card id=qc-alg7-hdf-weakness
Describe the highest-degree-first heuristic for vertex cover and its weakness.
---
Repeatedly put a vertex of highest current degree into the cover and delete the edges it covers, until no edge remains. It always returns a cover, but it can be worse than the optimum by a factor of order $\ln|V|$.
```

```card id=qc-alg7-hdf-bound
Upper bound on the ratio of highest-degree-first for vertex cover?
---
$\ln|E|+1$ times the minimum vertex cover.
```

```card id=qc-alg7-np-opt-def
What does an NP-optimisation problem $\Pi$ specify?
---
Valid instances, recognisable in polynomial time. For each instance, feasible solutions of polynomial length, with a polynomial-time feasibility check. A polynomial-time objective with non-negative rational values. $\Pi$ is a minimisation or a maximisation problem.
```

```card id=qc-alg7-factor-delta
Define a factor-$\delta$ approximation for a minimisation and for a maximisation problem.
---
Minimisation ($\delta\ge1$): $f(I,S)\le\delta\cdot OPT(I)$. Maximisation ($\delta\le1$): $f(I,S)\ge\delta\cdot OPT(I)$. $S$ must be feasible, and the algorithm must run in time polynomial in $|I|$.
```

```card id=qc-alg7-decision-link
How does the decision version of an NP-optimisation problem relate to its hardness?
---
The decision version asks whether the optimum meets a bound $k$. NP-hardness of the decision version implies NP-hardness of the optimisation problem.
```

```card id=qc-alg7-ptas-fptas
Compare a PTAS and an FPTAS.
---
Both take an instance and an error parameter $\epsilon$ and return a solution within $(1\pm\epsilon)$ of the optimum. PTAS: polynomial in $|I|$ for each fixed $\epsilon$, with the exponent allowed to depend on $\epsilon$. FPTAS: polynomial in $|I|$ and $1/\epsilon$ together.
```

```card id=qc-alg7-techniques
Which techniques for designing approximation algorithms does the lecture list?
---
Greedy algorithms with a tailored analysis, dynamic programming, and LP-based methods with deterministic or randomised rounding. The list is not exhaustive.
```

```card id=qc-alg7-knap-greedy
Why does greedy by value-to-size ratio fail for maximum-value knapsack?
---
It can fill the bag with good ratios and leave no room for a large valuable item. In the lecture's five-item example with bound 11, greedy gets 35 and the optimum is 40.
```

```card id=qc-alg7-knap-best-two
What is the best-of-two fix for maximum-value knapsack, and what does it guarantee?
---
Sort by value-to-size ratio. Take the longest prefix $a_1,\dots,a_{k-1}$ that fits, and the next item $a_k$ alone; return the more valuable of these two sets. Guarantee: factor $1/2$, since the two values together are at least OPT.
```

```card id=qc-alg7-knap-ptas
How does ModGreedyKnapsack reach a $(1-\epsilon)$ guarantee for maximum-value knapsack?
---
Enumerate all subsets of at most $k=\lceil1/\epsilon\rceil-1$ items that fit, greedily fill each with the remaining items in ratio order, and return the best combined set. If the optimum has more than $k$ items, its $k$ most valuable items are among the branches, and the greedy part loses at most $OPT/(k+1)$. Time $O(n^{1/\epsilon}\cdot\mathrm{poly}(\log V,\log S))$, a PTAS.
```

```card id=qc-alg7-knap-dp
What does the pseudo-polynomial knapsack table compute, and what is its running time?
---
$X(i,v)$ is the smallest total size of a subset of the first $i$ items with value exactly $v$, or $\infty$. Each entry takes the smaller of skipping item $i$ and adding it. The answer is the largest $v$ with $X(n,v)\le S$. Time $O(nV)$, where $V$ is the total value.
```

```card id=qc-alg7-knap-rescale
How does Rescale-DP turn the value DP into an FPTAS?
---
Scale each value to $\lfloor v(a_i)\,n/(\epsilon\,v_{\max})\rfloor$, keep sizes, and run the DP on the scaled values. The slides bound the result by $OPT-\epsilon v_{\max}\ge(1-\epsilon)OPT$. Time $O(n^3/\epsilon)$.
```

```card id=qc-alg7-fptas-pseudo
What does an FPTAS for an integer-valued problem imply about pseudo-polynomial algorithms?
---
If OPT is bounded by a polynomial in the unary input length, choose $\epsilon$ as one over that polynomial. The error is then below one unit, so the integer answer is optimal. Hence a strongly NP-hard problem with this bound has no FPTAS unless P = NP.
```

```card id=qc-alg7-maxsat-trivial
What is MaxSAT, and what is the simple 0.5-approximation?
---
Given a CNF formula, find an assignment satisfying the most clauses. Take any assignment; if it satisfies fewer than half the clauses, flip all variables. An assignment and its complement together satisfy every clause, so the better one satisfies at least half.
```

```card id=qc-alg7-maxsat-known
Best known ratios and hardness thresholds for MaxSAT, Max3SAT and Max2SAT in the lecture?
---
MaxSAT: best known 0.7846 (Asano-Williamson), no ratio above 0.875 unless P = NP. Max3SAT: best known 7/8 (Karloff-Zwick), matching hardness. Max2SAT: best known 0.940 (Lewin, Livnat, Zwick), hardness 21/22 unless P = NP.
```

```card id=qc-alg7-maxsat-ilp
Write the ILP for MaxSAT used in the lecture and its LP relaxation.
---
Binary $y_i$ per variable, binary $z_j$ per clause. Maximise $\sum_j z_j$ subject to $z_j\le\sum_{x_i\in C_j}y_i+\sum_{\neg x_i\in C_j}(1-y_i)$. Relax all variables to $[0,1]$ and solve the LP.
```

```card id=qc-alg7-maxsat-rounding
How does randomised rounding turn the MaxSAT LP solution into an assignment, and what does it guarantee?
---
Set each $x_i=1$ independently with probability $\hat y_i$. The expected number of satisfied clauses is at least $(1-1/e)\,OPT$, about 0.6 OPT.
```

```card id=qc-alg7-maxsat-clause
Lower bound on the probability that a $k$-literal clause is satisfied by the rounding?
---
$\bigl(1-(1-1/k)^k\bigr)\hat z_j$. The proof uses AM-GM on the falsification probability and a linear lower bound from concavity.
```

```card id=qc-alg7-maxsat-best-two
What is the best-of-two MaxSAT algorithm and its guarantee?
---
With probability $1/2$ output a uniformly random assignment, otherwise the rounded LP assignment. Random assignments favour long clauses and rounding favours short ones, so the case analysis gives expected $\tfrac34 OPT$. The randomness can be removed by conditional expectations.
```
