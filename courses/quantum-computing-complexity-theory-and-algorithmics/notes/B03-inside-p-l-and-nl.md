# B03 Inside P: logarithmic space, L and NL

Below P the interesting resource is sublinear space: a machine that may read its input but can only remember $O(\log n)$ bits, i.e. a constant number of pointers into the input. L and NL capture "pointer-chasing" problems; directed reachability is the complete problem for NL, and the surprising theorem of Immerman and Szelepcsényi says NL is closed under complement, unlike what we believe for NP. Reachability is also what Savitch (B02) and the circuit class AC$^1$ (B04) are about, so this note connects those. References: Papadimitriou ch. 7.3 (Immerman-Szelepcsenyi, Thm 7.6), ch. 16 (logarithmic space: REACHABILITY, 2SAT, NL-completeness) [S26]; Immerman and Szelepcsenyi [S56]; "Logarithmic Space" is the second heading of Pichler's own course [S14] and "complexity classes inside P: L, NL" is the TISS bullet [S1]. Exam 1 material.

## Definitions

- **Log-space TM.** Two tapes: a read-only input tape (head moves freely, cannot write) and a read-write work tape; only work-tape cells count. $\mathrm L = \mathrm{SPACE}(\log n)$, $\mathrm{NL} = \mathrm{NSPACE}(\log n)$, $\mathrm{coNL} = \{\bar L : L \in \mathrm{NL}\}$. With $c \log n$ bits the machine holds $O(1)$ counters or vertex names in the range $[0, n^c)$. The 2026W lectures use a $k$-string machine ($k > 2$) whose first string is the read-only input and whose last string is a write-only output (the head never moves left on it); space is the total length of strings $2$ to $k-1$.
- **Configuration** of a log-space machine on $x$: (state, input head position, work tape content, work head position). Count: $|Q| \cdot (n+2) \cdot |\Gamma|^{c \log n} \cdot c \log n = O(n^{c'})$: **polynomially many** configurations. The configuration graph $G_{M,x}$ is polynomial in size and each edge can be checked in log space (compare two configurations, simulate one step).
- **Log-space reduction.** $A \le_L B$ if some log-space computable $f$ (write-only output tape, not counted) satisfies $x \in A \iff f(x) \in B$. Every log-space reduction is a polynomial-time reduction (polynomially many configurations, no repetition on a halting computation). NL-completeness is defined with $\le_L$; polynomial-time reductions would trivialise it, since $\mathrm{NL} \subseteq \mathrm P$ makes every nontrivial problem in NL complete under $\le_p$.
- **PATH (REACHABILITY).** Given a directed graph $G$ and vertices $s, t$: is there a directed path $s \to t$?
- **2SAT.** CNF with clauses of two literals. **Implication graph** $G_F$: vertices are the $2n$ literals; each clause $(a \vee b)$ contributes the edges $\neg a \to b$ and $\neg b \to a$ (both are the same implication $\neg a \Rightarrow b$).

## Results

**Lemma.** $\mathrm L \subseteq \mathrm{NL} \subseteq \mathrm P$, and $\mathrm{NL} \subseteq \mathrm{SPACE}(\log^2 n)$.
*Proof.* Given $N$ in NL and input $x$, a polynomial-time machine writes down $G_{N,x}$ (polynomially many vertices, edges by one-step simulation) and runs BFS from the start configuration to the (unique) accepting configuration. The second claim is Savitch's theorem (B02) with $s = \log n$: run REACH$(C_0, C_{acc}, O(\log n))$ on the configuration graph, generated on the fly rather than stored. $\square$

**Lemma (log-space reductions compose; $\mathrm L$ and $\mathrm{NL}$ are closed under $\le_L$).** Computing $g(f(x))$ in log space cannot store $f(x)$ (it may be polynomially long). Instead run $g$ on a *virtual* input: whenever $g$ wants bit $i$ of $f(x)$, restart $f$ from scratch and count output bits until the $i$-th one, discarding the rest. Space: that of $f$, of $g$ (on an input of polynomial length, so still $O(\log n)$), and a counter for $i$. Time is polynomial (each bit costs a full run of $f$). $\square$

**Theorem.** PATH is NL-complete.
*Membership.* Nondeterministically: `cur := s; for step in 1..n-1: guess a neighbour w of cur; cur := w; if cur = t accept`. Reject when the counter runs out. The state is (cur, step): $2 \log n$ bits. A branch accepts iff its guesses trace a path; any path can be shortened to length $\le n-1$ so if $t$ is reachable some branch accepts.
*Hardness.* Let $L \in \mathrm{NL}$ via $N$ (WLOG unique accepting configuration $C_{acc}$). Map $x \mapsto (G_{N,x}, C_0, C_{acc})$. This is log-space computable: enumerate all pairs of configurations (two counters of $O(\log n)$ bits), output the edge $(C, C')$ iff $C \vdash C'$ according to $\delta$ and the input bit under $C$'s head. $x \in L$ iff some computation path reaches $C_{acc}$ iff PATH answers yes. $\square$
By symmetry, $\overline{\mathrm{PATH}}$ (non-reachability) is coNL-complete, and 2SAT, as a language, is coNL-complete under $\le_L$; after Immerman-Szelepcsényi [S56] both are NL-complete.

**Theorem (2SAT and the implication graph).** $F$ is unsatisfiable iff some variable $x$ has $x \leadsto \neg x$ and $\neg x \leadsto x$ in $G_F$ (equivalently, $x, \neg x$ in the same strongly connected component).
*Proof.* Paths in $G_F$ are chains of valid implications, so $x \leadsto \neg x$ forces $x = 0$ and $\neg x \leadsto x$ forces $x = 1$: unsatisfiable. Conversely, if no variable is in an SCC with its negation, assign literals in reverse topological order of the SCC DAG: set a literal $\ell$ true iff $\mathrm{SCC}(\ell)$ comes after $\mathrm{SCC}(\neg \ell)$ topologically (the graph is skew-symmetric, $a \leadsto b \iff \neg b \leadsto \neg a$, so exactly one of the two comes later). Every clause $(a \vee b)$ is satisfied: if $a$ is false then $\mathrm{SCC}(\neg a)$ is after $\mathrm{SCC}(a)$, the edge $\neg a \to b$ gives $\mathrm{SCC}(b)$ at or after $\mathrm{SCC}(\neg a)$, and $\neg b \to a$ gives $\mathrm{SCC}(\neg b)$ at or before $\mathrm{SCC}(a)$, hence $b$ is true. $\square$
*2SAT $\in$ NL.* UNSAT-2SAT is in NL: guess $x$, then guess a path $x \leadsto \neg x$ and one $\neg x \leadsto x$ vertex by vertex (two PATH instances on $G_F$, whose edges are read off the clauses in log space). So 2SAT $\in$ coNL $= $ NL. Deterministically it is linear time via Kosaraju/Tarjan SCC, which uses linear space. Compare 3SAT: NP-complete (B01); HORN-SAT: P-complete (B04).

**Theorem (Immerman 1988, Szelepcsényi 1987).** $\mathrm{NL} = \mathrm{coNL}$; more generally $\mathrm{NSPACE}(s) = \mathrm{coNSPACE}(s)$ for space-constructible $s \ge \log n$.
*Proof sketch (inductive counting).* It suffices to show $\overline{\mathrm{PATH}} \in \mathrm{NL}$: an NL machine that on input $(G, s, t)$ has an accepting branch iff $t$ is **not** reachable. Let $L_d = \{v : \mathrm{dist}(s, v) \le d\}$ and $c_d = |L_d|$; $c_0 = 1$.
1. *Certifying $v \in L_d$:* guess a path of length $\le d$ from $s$ to $v$ (PATH membership routine). One-sided: a wrong guess makes the branch reject, never a false claim.
2. *Certifying $v \notin L_d$ given the true value of $c_{d-1}$:* enumerate all $u \in V$; for each $u$ guess whether $u \in L_{d-1}$ and if so certify it by a guessed path, incrementing a counter $m$; check that $u \ne v$ and $(u, v) \notin E$ for every certified $u$. At the end demand $m = c_{d-1}$; otherwise reject. If the branch survives, it has seen **all** of $L_{d-1}$ (the count matches, and certificates cannot be faked), and none of them is $v$ or a predecessor of $v$: so $v \notin L_d$. Space: $u, v, m, c_{d-1}, d$ and a path-guessing routine: $O(\log n)$.
3. *Computing $c_d$ from $c_{d-1}$:* for each $v$ decide $v \in L_d$ (by 1, with a path of length $\le d$) or $v \notin L_d$ (by 2); count the members. A branch that guesses wrong at any point rejects, so every surviving branch computes the correct $c_d$.
4. Iterate $d = 1, \dots, n-1$ keeping only $c_{d-1}$; finally certify $t \notin L_{n-1}$ by step 2 with $c_{n-1}$. Accept iff that succeeds. $\square$
Consequence: the nondeterministic space hierarchy is strict (B02), and the "polynomial hierarchy" analogue for log space collapses: $\mathrm{NL}^{\mathrm{NL}} = \mathrm{NL}$. Nothing similar is known for time: $\mathrm{NP} = \mathrm{coNP}$ is open, because counting witnesses of a polynomial-time verifier is a #P problem, whereas counting reachable configurations of a log-space machine is itself an NL task.

**Theorem (Reingold 2005, statement).** UNDIRECTED PATH is in L, hence $\mathrm{SL} = \mathrm L$. The algorithm turns the graph into an expander by repeated zig-zag products and powering, after which $O(\log n)$-length walks from $s$ suffice; enumerating all such walks needs $O(\log n)$ bits. Earlier: undirected reachability in randomised log space by a random walk of length $O(n^3)$ (Aleliunas et al. 1979). Directed reachability in L would give $\mathrm L = \mathrm{NL}$.

**L vs NL vs P.** Open. Known: $\mathrm L \subseteq \mathrm{NL} \subseteq \mathrm{NC}^2 \subseteq \mathrm P$ (B04) and $\mathrm{NL} \ne \mathrm{PSPACE}$. Complete problems locate the questions: PATH for NL, CIRCUIT VALUE for P (B04). $\mathrm L = \mathrm P$ would mean every polynomial-time problem is solvable with $O(1)$ pointers into the input; $\mathrm{NL} = \mathrm P$ would parallelise all of P (B04).

## Worked example

*Graph.* $V = \{0, \dots, 5\}$, edges $0 \to 1, 0 \to 2, 1 \to 3, 2 \to 3, 3 \to 4, 5 \to 0$; $s = 0$. Is $t = 5$ reachable?

*NL guess.* Branches of the guessing machine from 0 with counter limit 5: $0 \to 1 \to 3 \to 4$ (stuck), $0 \to 2 \to 3 \to 4$ (stuck): no branch reaches 5, so the machine rejects; every branch stored only (cur, step).

*Inductive counting.* $L_0 = \{0\}$, $c_0 = 1$. $d = 1$: predecessors reachable: $1, 2$ from $0$: $L_1 = \{0, 1, 2\}$, $c_1 = 3$. $d = 2$: $3$ has predecessor $1 \in L_1$: $c_2 = 4$. $d = 3$: $4$: $c_3 = 5$. $d = 4, 5$: no new vertices, $c_4 = c_5 = 5$. Certifying $5 \notin L_5$: enumerate $u = 0, \dots, 5$; guess and certify membership for $u = 0, 1, 2, 3, 4$ (five certificates, $m = 5 = c_4$); none equals 5 and none has an edge to 5 (the only edge into 5 does not exist; the edge $5 \to 0$ is irrelevant). Accept: 5 is provably unreachable. (`inductive_count` returns `[1, 3, 4, 5, 5, 5]`, `certify_unreachable(g, 0, 5)` is `True`.)

*2SAT.* $F = (x_1 \vee x_2)(\neg x_1 \vee x_2)(\neg x_2 \vee x_3)(\neg x_3 \vee \neg x_1)$. Implications: $\neg x_1 \to x_2$, $\neg x_2 \to x_1$; $x_1 \to x_2$, $\neg x_2 \to \neg x_1$; $x_2 \to x_3$, $\neg x_3 \to \neg x_2$; $x_3 \to \neg x_1$, $x_1 \to \neg x_3$. Chain $x_1 \to x_2 \to x_3 \to \neg x_1$: so $x_1 \leadsto \neg x_1$, forcing $x_1 = 0$. Is there a path $\neg x_1 \leadsto x_1$? From $\neg x_1$: $x_2, x_3, \neg x_1$ only. No. Hence satisfiable: $x_1 = 0$, then $x_2 = 1$ (from $\neg x_1 \to x_2$), $x_3 = 1$. The SCCs are all singletons here; `two_sat` returns `{1: False, 2: True, 3: True}`. Adding the clause $(\neg x_2 \vee \neg x_3)$ would add $x_2 \to \neg x_3$ and $x_3 \to \neg x_2$, creating $x_2 \to x_3 \to \neg x_2$ and $\neg x_2 \to x_1 \to x_2$: $x_2$ and $\neg x_2$ in one SCC, unsatisfiable.

## Pitfalls

- Log space counts only the work tape. Copying the input to the work tape is not allowed; the input head is the "free" resource. A number of magnitude $n^c$ costs $c \log n$ bits, a subset of vertices costs $n$ bits (too much).
- The NL machine for PATH does **not** store the path, only the current vertex and a counter; the counter is essential to force termination on every branch.
- Certificates in NL are one-sided: a branch can fail to certify a true fact (guess wrong) but never certify a false one. Immerman-Szelepcsényi turns this into two-sided certification via exact counts.
- NL-completeness must be under log-space (or at least $\mathrm{NC}^1$) reductions; under polynomial-time reductions every problem in $\mathrm P$ reduces to any nontrivial problem in $\mathrm P$.
- 2SAT is in NL (in fact NL-complete), 3SAT is NP-complete, HORN-SAT is P-complete: the syntactic restriction matters.
- Savitch's algorithm for NL uses $O(\log^2 n)$ space and quasi-polynomial time; the polynomial-time algorithm (BFS on the configuration graph) uses polynomial space. No algorithm is known with both $O(\log n)$ space and polynomial time for directed reachability, which is exactly the L vs NL question.
- Reingold is about **undirected** graphs. Directed reachability in L is open.

## Exam-style questions

1. *Define L and NL and show $\mathrm{NL} \subseteq \mathrm P$.* Read-only input, $O(\log n)$ work tape; polynomially many configurations; BFS on the configuration graph.
2. *Prove PATH is NL-complete.* Membership by guessing a path with a step counter; hardness via $x \mapsto (G_{N,x}, C_0, C_{acc})$ computed in log space.
3. *Give an NL algorithm for the complement of 2SAT and explain why this puts 2SAT in NL.* Guess $x$ and paths $x \leadsto \neg x \leadsto x$ in the implication graph; correctness from the implication-graph theorem; $\mathrm{coNL} = \mathrm{NL}$ (Immerman-Szelepcsényi).
4. *Explain the inductive counting step: given $c_{d-1}$, how does an NL machine certify $v \notin L_d$?* Enumerate all $u$, certify $c_{d-1}$ of them in $L_{d-1}$ by guessed paths, check none is $v$ or a predecessor; reject if the count is not met.
5. *Why can log-space reductions be composed although $f(x)$ cannot be stored?* Recompute $f(x)$ bit by bit on demand; polynomial time overhead, still $O(\log n)$ space.
6. *Which of the following are known: $\mathrm L = \mathrm{NL}$, $\mathrm{NL} = \mathrm{coNL}$, $\mathrm{NL} = \mathrm P$, $\mathrm{NL} \ne \mathrm{PSPACE}$?* Only $\mathrm{NL} = \mathrm{coNL}$ (Immerman-Szelepcsényi) and $\mathrm{NL} \ne \mathrm{PSPACE}$ (Savitch plus space hierarchy).

## 2026W lectures: chapter 1 (28.10) and chapter 3 (30.10) [S62]

The decks treat L and NL briefly. Not in them: Immerman-Szelepcsényi, Reingold, the 2SAT implication graph (2-SAT is only called tractable via reachability), HORN-SAT. Composition of log-space reductions is named as beyond the scope of the lecture. In our words:

- **L.** A log-space program keeps a constant number of pointers, counters and Boolean flags. Its running time is not bounded by definition, but with logarithmic work space (and a logarithmic counter for the input head) there are only polynomially many configurations, so it can be taken polynomial.
- **Example in L: FIND-NODE** (does a labelled tree contain label $n$). Traverse the tree with two node pointers (current, next) and one direction bit: going down, move to the first child or turn upward at a leaf; going up, move to the next sibling and turn downward, else to the parent; stop on return to the root.
- **NL and REACHABILITY.** REACHABILITY is NL-complete under log-space reductions. Membership: keep the current vertex in binary, guess the next vertex, check the edge, stop with yes at the target, and count steps so no path of length $n$ or more is built. Hardness idea: an NL machine's configurations and transitions form the configuration graph, and acceptance is a path from the start to an accepting configuration. Since REACHABILITY is in P, NL $\subseteq$ P follows.
- **Why log-space reductions.** For P and classes below it, polynomial-time reductions are too strong to compare problems.

## Code

`src/py/complexity/reachability.py`: `nl_reachable` (guess-the-next-vertex machine, state = current vertex + counter, all branches explored), `savitch_reach` / `reachable_savitch` (B02), `implication_graph`, `kosaraju_scc`, `two_sat` (SCC criterion, returns an assignment), `two_sat_brute`, `inductive_count` (returns $c_{n-1}$, optionally the whole sequence $c_0, \dots, c_{n-1}$), `certify_unreachable`. Tests compare with `networkx.has_path`, `networkx.strongly_connected_components` and brute-force 2SAT.

## Cards

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

```card id=qc-cx3-2sat
When is a 2SAT formula unsatisfiable, in terms of the implication graph?
---
Iff some $x$ has $x\leadsto\neg x$ and $\neg x\leadsto x$ (same SCC).
Clause $(a\vee b)$ gives edges $\neg a\to b$ and $\neg b\to a$.
```

```card id=qc-cx3-sat-variants
Classify 2SAT, 3SAT and HORN-SAT.
---
2SAT: NL-complete. 3SAT: NP-complete. HORN-SAT: P-complete.
```

```card id=qc-cx3-immerman
State Immerman-Szelepcsényi and its proof technique.
---
$\mathrm{NL}=\mathrm{coNL}$ (generally $\mathrm{NSPACE}(s)=\mathrm{coNSPACE}(s)$, $s\ge\log n$).
Inductive counting: compute $c_d=|L_d|$ from $c_{d-1}$, then certify $t\notin L_{n-1}$.
```

```card id=qc-cx3-certify-not
Given $c_{d-1}$, how does an NL machine certify $v\notin L_d$?
---
Enumerate all $u$, certify $c_{d-1}$ of them in $L_{d-1}$ by guessed paths, check none is $v$ or a predecessor of $v$.
Reject if the count is not met.
```

```card id=qc-cx3-reingold
True or false: Reingold's theorem puts directed reachability in L.
---
False. Reingold: UNDIRECTED PATH is in L (SL = L).
Directed reachability in L would give L = NL, which is open.
```

```card id=qc-cx3-known
Which are known: L = NL, NL = coNL, NL = P, NL $\ne$ PSPACE?
---
Only NL = coNL (Immerman-Szelepcsényi) and NL $\ne$ PSPACE (Savitch plus space hierarchy).
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
