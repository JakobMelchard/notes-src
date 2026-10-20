# A04 Divide and conquer: merge sort, inversions, closest pair, Karatsuba, Strassen

Divide and conquer splits an input into parts of a constant fraction of the size, solves the parts recursively, and combines the answers. The recursion has depth $O(\log n)$, so if combining costs little the total is a small multiple of the combine cost (A01, master theorem). The paradigm gives $O(n \log n)$ sorting, an $O(n \log n)$ count of inversions that looks like it needs $\Theta(n^2)$, an $O(n \log n)$ closest pair in the plane, and, more surprisingly, sub-cubic matrix multiplication and sub-quadratic integer multiplication by finding a clever way to save one recursive call out of four (Karatsuba) or eight (Strassen). Reference: KT chapter 5 (5.1 merge sort and recurrences, 5.3 inversions, 5.4 closest pair, 5.5 integer multiplication, 5.6 FFT); Strassen is not in KT, see CLRS 4.2. References: Kleinberg & Tardos ch. 5 [S25]; 192.219 additionally names merge sort [S5]. Exam 1 material. The 2026W chapter 4 covers merge sort, the restricted master theorem, counting inversions and closest pair only; Karatsuba, Strassen and the FFT below are background, not lecture material.

## Definitions

**Divide and conquer.** A recursive algorithm of the form: if $n \le n_0$ solve directly; else split into $a$ subproblems of size $\approx n/b$, recurse, combine in $f(n)$ time. Running time $T(n) = a T(n/b) + f(n)$ (A01).

**Inversion.** For a sequence $a_1, \ldots, a_n$ of distinct numbers, a pair $i < j$ with $a_i > a_j$. The count ranges from 0 (sorted) to $\binom{n}{2}$ (reverse sorted) and measures how far the sequence is from sorted (used in rank comparison, "collaborative filtering" in KT).

**Closest pair.** Points $p_1, \ldots, p_n \in \mathbb{R}^2$; find $i \ne j$ minimising the Euclidean distance $d(p_i, p_j)$.

**Integer multiplication.** Two $n$-bit integers $x, y$; schoolbook costs $\Theta(n^2)$ bit operations. Here the input size is $n$ bits, and $O(1)$ operations on single bits (or single digits) are the elementary steps, not word-RAM multiplication.

**Matrix multiplication.** $C = AB$ for $n \times n$ matrices; the definition $c_{ij} = \sum_k a_{ik} b_{kj}$ costs $\Theta(n^3)$ arithmetic operations. Exponent $\omega$: the infimum of $c$ such that $n \times n$ multiplication is $O(n^{c})$; $2 \le \omega < 2.372$ (current bound), Strassen gives $\omega \le \log_2 7$.

## Results

### Merge sort (KT 5.1)

Split the array in halves, sort each recursively, *merge* the two sorted halves by repeatedly taking the smaller front element. Merge is $O(n)$ (each comparison consumes one element). $T(n) = 2T(n/2) + cn$, $T(1) = c$, so $T(n) = O(n \log n)$ (A01, case 2). Merge sort is optimal among comparison sorts: any comparison-based sort needs $\lceil \log_2 n! \rceil = \Omega(n \log n)$ comparisons in the worst case (decision tree with $n!$ leaves has depth $\ge \log_2 n!$).

### Counting inversions: merge-and-count (KT 5.3)

Split into left half $L$ and right half $R$. Inversions are of three kinds: both indices in $L$, both in $R$, or $i \in L, j \in R$ ("split inversions"). The first two are counted recursively. For split inversions, the trick is to have the recursive calls return their halves *sorted*, then count while merging:

```
merge-and-count(L, R):   # both sorted
  i=j=0, count=0, out=[]
  while i<|L| and j<|R|:
    if L[i] <= R[j]: append L[i]; i++
    else: append R[j]; count += |L| - i; j++     # R[j] is smaller than all remaining L[i..]
  append the rest; return count, out
```

When $R[j]$ is emitted before the remaining $|L| - i$ elements of $L$, each of those forms an inversion with $R[j]$ (they are in $L$, i.e. earlier in the original order, and larger). Sorting the halves does not change the split-inversion count because that count only depends on which elements are in $L$ versus $R$. Total $T(n) = 2T(n/2) + O(n) = O(n \log n)$, versus the naive $\Theta(n^2)$.

### Closest pair of points (KT 5.4)

Sort the points once by $x$ (list $P_x$) and once by $y$ (list $P_y$). Recursive step on a set $P$ with $|P| \ge 4$ (brute force below that):

1. Split by the vertical line $x = x^*$ through the median $x$-coordinate into $Q$ (left half) and $R$ (right half); build $Q_x, Q_y, R_x, R_y$ in $O(n)$ by filtering.
2. Recurse: $\delta = \min(\delta_Q, \delta_R)$, the smallest distance found within a half.
3. Combine: only pairs $(q, r)$ with $q \in Q, r \in R$ and $d(q, r) < \delta$ can beat $\delta$. Let $S$ be the points with $|x - x^*| < \delta$ (the *strip*), listed by $y$ (filter $P_y$, $O(n)$). Scan $S_y$; for each point compare it with the next **11** points in $y$-order (the 2026W lecture's constant; a finer count gets it down to 7, and KT uses 15).

**Strip lemma.** If $s_i, s_j$ are the strip points with the $i$-th and $j$-th smallest $y$-coordinate and $|i - j| \ge 12$, then $d(s_i, s_j) \ge \delta$. So a closer pair sits within 11 positions in $S_y$.

*Proof.* Cover the strip with boxes of side $\delta/2$: four columns (two on each side of $x^*$, strip width $2\delta$) and rows of height $\delta/2$. Each box lies entirely in $Q$ or entirely in $R$, so two points in one box would be at distance $\le \delta/\sqrt{2} < \delta$, contradicting $\delta$ being the minimum within a half; hence **each box holds at most one point**. The row of $s_i$ and the two rows above it have 12 boxes, so at most 12 points including $s_i$; a point 12 or more positions later lies at least three rows higher, with two full rows in between, so its $y$-distance alone is $\ge 2 \cdot \delta/2 = \delta$. $\square$ The exact constant does not matter; what matters is $O(1)$ comparisons per strip point.

**Correctness.** Any pair closer than $\delta$ is in the strip and is detected by the scan; pairs within a half are found recursively. **Time:** $T(n) = 2T(n/2) + O(n) = O(n \log n)$; the initial sort is another $O(n \log n)$. Re-sorting the strip at each level would give $O(n \log^2 n)$; carrying the $y$-sorted lists down avoids it.

### Karatsuba integer multiplication (KT 5.5)

Write $x = x_1 2^{n/2} + x_0$, $y = y_1 2^{n/2} + y_0$ with $n/2$-bit halves. Then
$$xy = x_1 y_1 2^{n} + (x_1 y_0 + x_0 y_1) 2^{n/2} + x_0 y_0.$$
Four half-size products give $T(n) = 4T(n/2) + O(n) = O(n^2)$: nothing gained. Karatsuba: compute $p = (x_1 + x_0)(y_1 + y_0) = x_1 y_1 + x_1 y_0 + x_0 y_1 + x_0 y_0$, so the middle coefficient is $p - x_1 y_1 - x_0 y_0$. **Three** products: $x_1 y_1$, $x_0 y_0$, $p$ (on $n/2 + 1$ bits), plus $O(n)$ additions and shifts. $T(n) = 3T(n/2) + O(n) = O(n^{\log_2 3}) = O(n^{1.585})$ (A01, case 1). Toom–Cook generalises to $k$ pieces with $2k-1$ products; the FFT-based Schönhage–Strassen and Harvey–van der Hoeven achieve $O(n \log n)$.

### Strassen's matrix multiplication

Split $A, B$ into $n/2 \times n/2$ blocks. Block multiplication $C_{11} = A_{11}B_{11} + A_{12}B_{21}$ etc. needs 8 block products: $T(n) = 8T(n/2) + O(n^2) = \Theta(n^3)$ (exponent $\log_2 8 = 3$; case 1). Strassen found 7 products that suffice:
$$\begin{aligned}
M_1 &= (A_{11} + A_{22})(B_{11} + B_{22}), & M_2 &= (A_{21} + A_{22}) B_{11}, & M_3 &= A_{11}(B_{12} - B_{22}),\\
M_4 &= A_{22}(B_{21} - B_{11}), & M_5 &= (A_{11} + A_{12}) B_{22}, & M_6 &= (A_{21} - A_{11})(B_{11} + B_{12}),\\
M_7 &= (A_{12} - A_{22})(B_{21} + B_{22}),
\end{aligned}$$
$$C_{11} = M_1 + M_4 - M_5 + M_7,\quad C_{12} = M_3 + M_5,\quad C_{21} = M_2 + M_4,\quad C_{22} = M_1 - M_2 + M_3 + M_6.$$
(Check $C_{12}$: $M_3 + M_5 = A_{11}B_{12} - A_{11}B_{22} + A_{11}B_{22} + A_{12}B_{22} = A_{11}B_{12} + A_{12}B_{22}$.) 18 block additions cost $O(n^2)$, so $T(n) = 7T(n/2) + O(n^2)$, $\log_2 7 \approx 2.807 > 2$, case 1: $\Theta(n^{\log_2 7}) = O(n^{2.81})$. The idea generalises: any way to multiply $k \times k$ matrices with $r$ scalar products gives $\omega \le \log_k r$; the current record $\omega < 2.372$ uses the laser method on tensor powers and is impractical. Strassen itself beats the naive method for $n$ in the hundreds, with worse numerical stability.

### FFT (KT 5.6, mention only)

Polynomial multiplication (convolution) of two degree-$n$ polynomials in $O(n \log n)$: evaluate both at the $2n$ complex roots of unity by a $2T(n/2) + O(n)$ recursion (split into even and odd coefficients), multiply pointwise, interpolate by the inverse transform (same recursion with conjugate roots). The quantum Fourier transform (C-part) is the same linear map on amplitudes, implemented with $O(\log^2 N)$ gates instead of $O(N \log N)$ operations.

## Worked example

**Counting inversions** in $a = [1, 5, 4, 8, 10, 2, 6, 9, 3, 7]$.

Split: $L = [1, 5, 4, 8, 10]$, $R = [2, 6, 9, 3, 7]$.

Left: $[1,5,4,8,10] \to [1,5] \mid [4,8,10]$. $[1,5]$: 0 inversions, sorted. $[4,8,10] \to [4] \mid [8,10]$, $[8,10]$: 0; merging $[4]$ and $[8,10]$: 0. Merge $[1,5]$ with $[4,8,10]$: emit 1; emit 4 (from $R$, remaining in $L$: $\{5\}$, count 1); emit 5, 8, 10. Left total: 1 (the pair (5,4)). Sorted $[1,4,5,8,10]$.

Right: $[2,6,9,3,7] \to [2,6] \mid [9,3,7]$. $[2,6]$: 0. $[9,3,7] \to [9] \mid [3,7]$; $[3,7]$: 0; merge $[9]$ with $[3,7]$: emit 3 (count $+1$), emit 7 (count $+1$), emit 9: 2. Merge $[2,6]$ with $[3,7,9]$: emit 2; emit 3 (remaining $L = \{6\}$, $+1$); emit 6, 7, 9. Right total $0 + 2 + 1 = 3$ (pairs (9,3), (9,7), (6,3)). Sorted $[2,3,6,7,9]$.

Split inversions, merging $[1,4,5,8,10]$ and $[2,3,6,7,9]$: emit 1; emit 2 (remaining $L$: 4,5,8,10: $+4$); emit 3 ($+4$); emit 4, 5; emit 6 (remaining 8,10: $+2$); emit 7 ($+2$); emit 8; emit 9 (remaining 10: $+1$); emit 10. Split count $4+4+2+2+1 = 13$.

Total $1 + 3 + 13 = 17$. Check by brute force: pairs $(5,4),(5,2),(5,3),(4,2),(4,3),(8,2),(8,6),(8,3),(8,7),(10,2),(10,6),(10,9),(10,3),(10,7),(6,3),(9,3),(9,7)$: 17.

**Karatsuba** on $x = 1234$, $y = 5678$ (base 10, two-digit halves): $x_1 = 12, x_0 = 34, y_1 = 56, y_0 = 78$. $x_1y_1 = 672$, $x_0 y_0 = 2652$, $p = 46 \cdot 134 = 6164$, middle $= 6164 - 672 - 2652 = 2840$. Result $672 \cdot 10^4 + 2840 \cdot 10^2 + 2652 = 6720000 + 284000 + 2652 = 7006652$. Check: $1234 \cdot 5678 = 7006652$.

**Closest pair strip.** Suppose the recursive calls returned $\delta_Q = \delta_R = 2$ around the median line $x^* = 5$, so $\delta = 2$ and the strip is $3 < x < 7$. Strip points sorted by $y$: $p_1 = (4.2, 0)$, $p_2 = (5.9, 0.6)$, $p_3 = (3.4, 1.9)$, $p_4 = (6.4, 2.7)$, $p_5 = (4.6, 4.0)$. (Consistent with $\delta = 2$: within $Q = \{p_1, p_3, p_5\}$ the closest are $p_1, p_3$ at $\sqrt{0.64 + 3.61} \approx 2.06$; within $R = \{p_2, p_4\}$ the distance is $\sqrt{0.25 + 4.41} \approx 2.16$.) Scan: $p_1$ vs $p_2$: $\sqrt{2.89 + 0.36} \approx 1.80 < 2$, new best, and it straddles $x^*$ ($4.2 < 5 < 5.9$) as any improvement must; $p_1$ vs $p_3$: $2.06$; $p_1$ vs $p_4$: $y$-difference $2.7 \ge \delta$, stop scanning successors of $p_1$. $p_2$ vs $p_3$: $\sqrt{6.25 + 1.69} \approx 2.82$; $p_2$ vs $p_4$: $2.16$; $p_2$ vs $p_5$: $y$-difference $3.4$, stop. $p_3$ vs $p_4$: $\approx 3.10$; $p_3$ vs $p_5$: $y$-difference $2.1$, stop. $p_4$ vs $p_5$: $\sqrt{3.24 + 1.69} \approx 2.22$. Result: closest pair $(p_1, p_2)$ at $\approx 1.80$. No point was compared with more than 3 successors, well within the bound of 11.

## Pitfalls

- The recursion must shrink the size by a *constant factor*; $T(n) = 2T(n-1) + 1$ is exponential (A01).
- Counting inversions requires the recursive calls to *return sorted output*; counting split inversions on unsorted halves is $\Theta(n^2)$ again.
- In merge-and-count, add $|L| - i$ when taking from $R$, not $|R| - j$ when taking from $L$; with equal elements decide the convention (strict $>$) and use `<=` in the merge.
- Closest pair: the strip is $|x - x^*| < \delta$ around the *median*, width $2\delta$; the per-point constant (11 in the lecture, 7 or 15 elsewhere) comes from a packing argument that needs $\delta$ to be the minimum *within each half*, which is why squares of side $\delta/2$ hold at most one point.
- Sorting the strip by $y$ at every level costs $O(n \log n)$ per level, $O(n \log^2 n)$ total; pre-sorted $P_y$ filtered top-down fixes it.
- Karatsuba's $p$ has $n/2 + 1$ bits; this does not change the recurrence's asymptotics but does matter in an implementation (carry).
- Strassen's exponent is $\log_2 7$, from $a = 7$, $b = 2$; the $O(n^2)$ additions are lower order, they do not make it $n^2 \log n$.
- Divide and conquer is a *time* trade-off, not a space one: merge sort needs $O(n)$ extra space; recursion depth $O(\log n)$ on the stack.

## Exam-style questions

1. **Why does merge sort run in $O(n \log n)$ while insertion sort runs in $O(n^2)$? Give the recurrence.** $T(n) = 2T(n/2) + O(n)$: $\log_2 n$ levels each doing $O(n)$ merge work. Insertion sort does $O(i)$ work to insert the $i$-th element, $\sum i = \Theta(n^2)$.

2. **Describe an $O(n \log n)$ algorithm for counting inversions and justify the count of split inversions.** Merge-and-count. When $R[j]$ is emitted, the $|L| - i$ remaining left elements are all larger than $R[j]$ and precede it in the input: exactly those are the split inversions involving $R[j]$; every split inversion is counted exactly once, at the moment its right element is emitted.

3. **In the closest-pair algorithm, why does it suffice to compare each strip point with a constant number of others?** Boxes of side $\delta/2$ within one half contain at most one point each (two points in one box would be at distance $\le \delta/\sqrt{2} < \delta$). The strip is four boxes wide, so the row of $s$ and the two rows above hold at most 12 points including $s$. A point 12 or more positions later in $y$-order lies at least three rows higher, with two full rows of height $\delta/2$ in between, so at distance $\ge \delta$; hence comparing with the next 11 points suffices.

4. **Karatsuba uses three multiplications instead of four. What if someone found a way to use two? What if the split was into three parts with five products?** Two: $T(n) = 2T(n/2) + O(n) = O(n \log n)$. Three parts, five products: $T(n) = 5T(n/3) + O(n) = O(n^{\log_3 5}) = O(n^{1.465})$, better than Karatsuba's $1.585$; that is Toom-3 (actually Toom-3 uses 5 products, so this is real).

5. **Strassen: state the recurrence and solve it. Why is $T(n) = 7T(n/2) + O(n^2)$ not $\Theta(n^2 \log n)$?** $\log_2 7 \approx 2.807 > 2$, so $n^2 = O(n^{2.807 - \epsilon})$ with $\epsilon = 0.8$: case 1, leaves dominate, $\Theta(n^{\log_2 7})$. Case 2 (the $\log$ factor) would need $f(n) = \Theta(n^{\log_2 7})$.

## Code

`src/py/algorithmics/divide_conquer.py`: `merge_sort(a)`, `count_inversions(a)` (returns count and sorted list), `closest_pair(points)` (with the strip scan; tests compare to brute force), `strassen(A, B)` (numpy blocks, padded to a power of two), `master_theorem(a, b, d)`.

## 2026W lecture: chapter 4, divide and conquer (14.10) [S62]

Chen's chapter-4 deck follows Kleinberg and Tardos chapter 5 [S25] but stops
after closest pair. In our words:

- **Paradigm.** Split into parts, solve them recursively, combine. The common
  case halves the input and combines in linear time; named uses are binary
  search, exponentiation and sorting.
- **Merge sort** with the recurrence
  $T(n)\le T(\lceil n/2\rceil)+T(\lfloor n/2\rfloor)+O(n)$, solved to
  $O(n\log n)$ twice: by the recursion tree ($\log_2 n$ levels of $cn$ each)
  and by induction on $k$ for $n=2^k$.
- **Restricted master theorem.** For $T(n)=x\,T(\lceil n/y\rceil)+O(n^d)$ with
  constants $x>0$, $y>1$, $d\ge 0$: $O(n^d)$ if $d>\log_y x$,
  $O(n^d\log n)$ if $d=\log_y x$, $O(n^{\log_y x})$ if $d<\log_y x$. Extension:
  if the combine cost is $O(n^{\log_y x}\log^k n)$ with $k>-1$, then
  $T(n)=O(n^{\log_y x}\log^{k+1} n)$.
- **Counting inversions** of a permutation, motivated by comparing two
  rankings. The number of inversions is a distance between rankings (non-negative,
  symmetric, triangle inequality); finding one ranking with least total distance
  to a set of rankings (Kemeny) is NP-hard. Sort-and-count in $O(n\log n)$.
- **Closest pair in the plane.** 1D: sort and compare neighbours. Quadrants
  fail (no even split). Split by a vertical line, recurse, then check the
  $2\delta$ strip in $y$-order against the next 11 points. Sorting the strip
  anew in each call gives $T(n)\le 2T(n/2)+O(n\log n)=O(n\log^2 n)$; passing
  down lists sorted by $x$ and by $y$ gives $O(n\log n)$.

## Cards

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

