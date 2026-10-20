# B01 Models of computation and NP-completeness

Complexity theory classifies problems by the resources (time, space, randomness, qubits) needed to solve them on a fixed machine model, as a function of input length. This note fixes the model (Turing machines), the two basic classes P and NP, and the tool that makes NP a coherent class: polynomial-time reductions and NP-completeness (Cook-Levin). Everything later (B02 to B06) is built on the same skeleton: a resource bound, a class, a complete problem. References: Papadimitriou ch. 2, 7, 8-9 [S26]; Kleinberg & Tardos ch. 8 [S25]; Pichler's own recapitulation deck, which fixes the vocabulary this note uses (decision vs function vs optimization vs enumeration vs counting problems, Cook vs Karp reductions, C-hard and C-complete) [S14]. The random-access machine and "problem reductions" bullets come from 192.042, whose content moved into this course in 2026W [S4]. Cook-Levin [S56]. Exam 1 material.

## Definitions

**Turing machine (TM).** A 7-tuple $M = (Q, \Sigma, \Gamma, \delta, q_0, q_{acc}, q_{rej})$: finite state set $Q$, input alphabet $\Sigma$, tape alphabet $\Gamma \supset \Sigma \cup \{\sqcup\}$, transition function $\delta : Q \times \Gamma \to Q \times \Gamma \times \{L, R\}$, start state $q_0$, halting states $q_{acc} \ne q_{rej}$. A **configuration** is $(q, w, i)$: state, tape contents, head position; written as the string $w_1 \dots w_{i-1}\, q\, w_i \dots w_m$. $\delta$ defines the one-step relation $C \vdash C'$. $M$ accepts $x$ if $q_0 x \vdash^* C$ with $C$ in state $q_{acc}$. The language $L(M) = \{x : M \text{ accepts } x\}$. A decision problem is a language $L \subseteq \Sigma^*$. The 2026W lectures use Papadimitriou's equivalent quadruple form instead (section 2026W lectures below).

**Time and space.** $\mathrm{time}_M(x)$ = number of steps until halting; $T_M(n) = \max_{|x| = n} \mathrm{time}_M(x)$. $\mathrm{space}_M(x)$ = number of distinct cells visited; $S_M(n)$ analogously. Always as functions of the input length $n = |x|$, never of the numeric value of the input (a number $N$ has length $\log N$).

**Multi-tape TMs.** $k$ tapes with independent heads, $\delta : Q \times \Gamma^k \to Q \times \Gamma^k \times \{L,R,S\}^k$. Simulation overhead (Papadimitriou Thm 2.1): a $k$-tape TM running in time $T(n)$ is simulated by a 1-tape TM in time $O(T(n)^2)$ (store all $k$ tapes interleaved, mark the head positions; each simulated step sweeps the used part of the tape, length $O(T(n))$). Space is preserved up to a constant. Consequence: "polynomial time" does not depend on the number of tapes. Similarly a random-access machine is simulated with polynomial overhead. This model-independence is the (classical) **Church-Turing thesis** in its "extended" form: every reasonable model is polynomially equivalent to a TM. B06 discusses why quantum computers challenge exactly the "extended" part.

**Random access machine (RAM).** A machine with an unbounded array of integer registers $r_0, r_1, \dots$, indirect addressing, and the instructions load/store/add/subtract/jump-if-zero. Under the **logarithmic cost** measure (a step on operands of $b$ bits costs $b$) a RAM and a Turing machine simulate each other with polynomial overhead, so P, NP, PSPACE and everything built on them are model-independent; under the *unit* cost measure they are not (repeated squaring builds $2^{2^n}$ in $n$ steps), which is why complexity theory uses logarithmic cost or Turing machines. The RAM is the reason "$O(n\log n)$ sorting" in the A-series and "polynomial time" here mean the same thing. The model is named explicitly on the syllabus of 192.042, "Formal models of computation (Turing machines, random access machines, ...)", whose content merged into this course in 2026W [S4], and Pichler's recapitulation deck lists "Turing machines as a reasonable model of computation" [S14].

**Nondeterministic TM (NTM).** $\delta : Q \times \Gamma \to \mathcal P(Q \times \Gamma \times \{L,R\})$; the computation on $x$ is a tree of configurations. $N$ accepts $x$ iff **some** branch reaches $q_{acc}$. Time = depth of the tree. An NTM is not a physical device; it is a definition that turns out to capture "problems with short checkable solutions".

**Classes.**
$$\mathrm{DTIME}(f) = \{L : \exists \text{ TM deciding } L \text{ in time } O(f(n))\}, \quad \mathrm{NTIME}(f), \ \mathrm{SPACE}(f), \ \mathrm{NSPACE}(f) \text{ analogously.}$$
$$\mathrm P = \bigcup_k \mathrm{DTIME}(n^k), \qquad \mathrm{NP} = \bigcup_k \mathrm{NTIME}(n^k), \qquad \mathrm{coNP} = \{\bar L : L \in \mathrm{NP}\}.$$

**NP via verifiers (second definition).** $L \in \mathrm{NP}$ iff there is a polynomial $p$ and a polynomial-time TM $V$ (the verifier) with
$$x \in L \iff \exists\, c \in \{0,1\}^{p(|x|)} : V(x, c) = 1.$$
$c$ is a **certificate** (witness). Example: SAT, certificate = satisfying assignment; HAMILTONIAN CYCLE, certificate = the cycle. coNP: $x \in L \iff \forall c: V(x,c) = 1$ (e.g. UNSAT, TAUTOLOGY). The 2026W lectures phrase the same definition as a certificate relation $R$ that is polynomially balanced ($(x, c) \in R$ implies $|c| \le |x|^k$) and decidable in polynomial time (section 2026W lectures below).

**Karp reduction.** $A \le_p B$ if there is a polynomial-time computable $f$ with $x \in A \iff f(x) \in B$ for all $x$. (Many-one: one query to $B$, answer taken as is. Cook/Turing reductions allow many adaptive queries; in this course reductions are Karp unless stated.)

**NP-hard / NP-complete.** $B$ is NP-hard if $A \le_p B$ for every $A \in \mathrm{NP}$; NP-complete if additionally $B \in \mathrm{NP}$.

## Results

**Theorem (NTM = verifier).** The two definitions of NP coincide.
*Proof.* ($\Leftarrow$) Given $V, p$: the NTM guesses $c$ bit by bit ($p(n)$ nondeterministic steps), then runs $V(x,c)$ deterministically. Some branch accepts iff some $c$ works. ($\Rightarrow$) Given an NTM $N$ with time $q(n)$: WLOG every step has at most 2 choices (a $d$-way choice is $\lceil \log d \rceil$ binary choices, constant blow-up). The certificate is the sequence of choices $c \in \{0,1\}^{q(n)}$; the verifier simulates $N$ on $x$ following $c$ and accepts iff that branch accepts. Polynomial time in $|x|$. $\square$

**Lemma (reductions compose).** $A \le_p B$ and $B \le_p C$ imply $A \le_p C$: $g \circ f$ runs in time $\mathrm{poly}(|f(x)|) \le \mathrm{poly}(\mathrm{poly}(|x|))$ because a polynomial-time function has polynomially bounded output. Also: $A \le_p B$ and $B \in \mathrm P$ imply $A \in \mathrm P$; contrapositive: $A \le_p B$ and $A$ NP-hard imply $B$ NP-hard. This is **how one proves hardness: reduce a known hard problem TO the new problem.**

**Theorem (Cook-Levin).** SAT is NP-complete.
*Proof sketch.* Membership: certificate = assignment, check in linear time. Hardness: let $L \in \mathrm{NP}$ via NTM $N$ with time $p(n)$ (single tape WLOG). A branch of the computation on $x$ is a **tableau**: $p(n) + 1$ rows (configurations), each a string of length $p(n) + 2$ over $\Gamma \cup (Q \times \Gamma)$ (the head position is marked by a composite symbol "state $q$ reading $a$"). Boolean variables $y_{i,j,s}$ = "cell $(i,j)$ contains symbol $s$", $O(p(n)^2 \cdot |\Gamma \cup Q\times\Gamma|)$ of them. Clauses: (i) each cell holds exactly one symbol; (ii) row 0 is the start configuration $q_0 x \sqcup \dots$; (iii) some row contains $q_{acc}$; (iv) **locality**: row $i+1$ follows from row $i$ by one step of $\delta$. Because a TM step changes only the cells around the head, correctness of a step is checked by $2 \times 3$ **windows**: for every $(i, j)$ the six symbols $y_{i,j-1..j+1}, y_{i+1,j-1..j+1}$ must be one of a constant number of legal patterns (determined by $\delta$; the nondeterminism is simply that several patterns are legal). Each window constraint is a constant-size CNF; total formula size $O(p(n)^2)$, computable in polynomial time. Satisfying assignments correspond exactly to accepting branches. $\square$

**SAT $\le_p$ 3SAT.** Replace a clause $C = (\ell_1 \vee \dots \vee \ell_k)$, $k \ge 4$, by
$$(\ell_1 \vee \ell_2 \vee y_1)(\neg y_1 \vee \ell_3 \vee y_2)(\neg y_2 \vee \ell_4 \vee y_3) \cdots (\neg y_{k-3} \vee \ell_{k-1} \vee \ell_k)$$
with fresh $y_i$ (clauses of length $< 3$: repeat a literal). *Equisatisfiability.* If some $\ell_j$ is true set $y_i = 1$ for $i < j - 1$ and $y_i = 0$ for $i \ge j-1$: every new clause contains a true literal. Conversely, if all $\ell_j$ are false the chain forces $y_1 = 1$, then $y_2 = 1$, ..., $y_{k-3} = 1$, and the last clause $(\neg y_{k-3} \vee \ell_{k-1} \vee \ell_k)$ is false. Size grows linearly. Note that the new formula is over more variables; the assignments correspond, they are not identical.

**3SAT $\le_p$ INDEPENDENT SET $\le_p$ VERTEX COVER (KT 8.2, 8.4).** For a 3-CNF with $m$ clauses build $G$: a triangle per clause with vertices labelled by the three literal occurrences, plus an edge between every pair of occurrences of $x$ and $\neg x$. Set $k = m$. *Claim:* $F$ satisfiable iff $G$ has an independent set of size $m$. ($\Rightarrow$) pick one true literal per clause: one vertex per triangle, never both $x$ and $\neg x$, so independent. ($\Leftarrow$) an independent set of size $m$ has exactly one vertex per triangle (two in one triangle are adjacent); set the chosen literals true; consistent because complementary occurrences are adjacent; every clause has a true literal. Then $S$ is independent iff $V \setminus S$ is a vertex cover (an edge is missed by $V \setminus S$ iff both endpoints lie in $S$), so IS with $k$ $\Leftrightarrow$ VC with $|V| - k = 2m$.

**VERTEX COVER $\le_p$ SUBSET SUM (KT 8.8).** Graph $G$ with $n$ vertices, edges $e_0, \dots, e_{m-1}$, budget $k$. Base-4 numbers with $m + 1$ digits:
$$a_v = 4^m + \sum_{i : v \in e_i} 4^i \ (v \in V), \qquad b_i = 4^i \ (0 \le i < m), \qquad W = k \cdot 4^m + 2 \sum_{i=0}^{m-1} 4^i .$$
Digit $i$ of any subset sum is at most $1 + 2 = 3 < 4$ (two endpoints plus $b_i$), so **there are no carries** and digits can be read off independently. If $C$ is a cover of size $k$: take $\{a_v : v \in C\}$ and $b_i$ for every edge covered exactly once; digit $m$ is $k$, every edge digit is $2$. Conversely a subset with sum $W$ has exactly $k$ vertex numbers (digit $m$) and every edge digit equal to 2, which needs at least one endpoint in the subset (the $b_i$ alone give 1): a cover of size $k$. Numbers have $O(m)$ digits, polynomial size; this is why SUBSET SUM is NP-complete although it has a pseudo-polynomial $O(nW)$ DP: $W$ here is exponential in $m$.

**Further NP-complete problems (statement).** 3SAT $\le_p$ HAMILTONIAN CYCLE (KT 8.5, one gadget per variable, a "diamond" chain visited left-to-right or right-to-left = truth value, clause nodes spliced in), 3SAT $\le_p$ 3-COLOURING (KT 8.7, a base triangle T/F/B, a variable pair $x, \neg x$ both adjacent to B, an "OR gadget" per clause). Also TSP (decision), CLIQUE, SET COVER, PARTITION, KNAPSACK (decision), INTEGER PROGRAMMING (feasibility).

**What NP-completeness means.** If any NP-complete problem is in P then P = NP (every $A \in \mathrm{NP}$ reduces to it). If P $\ne$ NP then no NP-complete problem has a polynomial algorithm, and (Ladner 1975 [S56]) there are problems in NP that are neither in P nor NP-complete (**NP-intermediate**; candidates: FACTORING, GRAPH ISOMORPHISM). Note $\mathrm P \subseteq \mathrm{NP} \cap \mathrm{coNP}$; if an NP-complete problem were in coNP then NP = coNP, also believed false. FACTORING (decision version) is in NP $\cap$ coNP, one reason it is not expected to be NP-complete and one reason Shor's algorithm (B06) does not imply NP $\subseteq$ BQP.

**Recipe for an exam-style NP-completeness proof.** (1) Membership: name the certificate, bound its length by a polynomial, give the polynomial-time check. (2) Hardness: choose a known NP-complete problem $A$, give $f$ from instances of $A$ to instances of the new problem $B$ (direction: **known-hard $\to$ new**), argue $f$ is polynomial-time, prove both directions of $x \in A \iff f(x) \in B$.

## Worked example

$F = (x_1 \vee x_2 \vee x_3)(\neg x_1 \vee \neg x_2 \vee x_3)(\neg x_3 \vee x_1 \vee x_2)$, DIMACS `[[1,2,3],[-1,-2,3],[-3,1,2]]`, $m = 3$.

*Graph.* Vertices $(i, j)$ = occurrence $j$ in clause $i$: $(0,0){=}x_1, (0,1){=}x_2, (0,2){=}x_3;\ (1,0){=}\neg x_1, (1,1){=}\neg x_2, (1,2){=}x_3;\ (2,0){=}\neg x_3, (2,1){=}x_1, (2,2){=}x_2$. Edges: 9 triangle edges plus complementary pairs $(0,0)(1,0)$, $(0,1)(1,1)$, $(0,2)(2,0)$, $(1,2)(2,0)$, $(2,1)(1,0)$, $(2,2)(1,1)$: 15 edges, $k_{IS} = 3$, $k_{VC} = 6$.

*Assignment to independent set.* $x_1 = 1, x_2 = 0, x_3 = 1$ satisfies $F$. True occurrences per clause: clause 0: $(0,0)$; clause 1: $(1,1)$ or $(1,2)$; clause 2: $(2,1)$. Choose $S = \{(0,0), (1,2), (2,1)\}$. Check: no triangle edge (one per clause), $(0,0)$'s only cross edge goes to $(1,0) \notin S$, $(1,2)$'s to $(2,0) \notin S$, $(2,1)$'s to $(1,0) \notin S$. Independent, size 3. Cover $= V \setminus S$, size 6; a cover of size 5 is impossible because three vertex-disjoint triangles need two vertices each (test `test_worked_example_from_note`).

*Subset sum on a toy graph* (the full instance has 24 numbers of 16 base-4 digits, done by `sat.py`): triangle $a, b, c$ with $e_0 = ab, e_1 = bc, e_2 = ac$, $k = 2$, $m = 3$:
$$a_a = 64 + 4^0 + 4^2 = 81, \quad a_b = 64 + 4^0 + 4^1 = 69, \quad a_c = 64 + 4^1 + 4^2 = 84, \quad b_0, b_1, b_2 = 1, 4, 16, \quad W = 2 \cdot 64 + 2 \cdot 21 = 170 = (2222)_4.$$
Cover $\{a, b\}$: $81 + 69 = 150 = (2112)_4$: digit $e_0$ is already 2 (both endpoints), digits $e_1, e_2$ are 1; add $b_1 + b_2 = 20$: $170$. Any subset hitting 170 must contain exactly two vertex numbers (digit 3 equals 2) and every edge digit reaches 2 only if a chosen vertex contributes, i.e. every edge is covered.

## Pitfalls

- **Direction of the reduction.** To show $B$ hard you need $A \le_p B$ with $A$ known hard: transform $A$-instances into $B$-instances. Reducing $B$ to SAT shows only $B \in \mathrm{NP}$ (uselessly, since everything in NP reduces to SAT).
- NP is "nondeterministic polynomial", not "non-polynomial". P $\subseteq$ NP; exponential brute force solves every NP problem, that is not the point.
- The certificate must have polynomial length **and** be checkable in polynomial time. "Guess a factorisation, check by multiplying" is fine; "guess whether a formula is a tautology" is not (no short certificate known for UNSAT: that is the NP vs coNP question).
- NP-hard is not NP-complete: HALTING is NP-hard but not in NP. TQBF is NP-hard, believed not in NP (B02).
- Input length. SUBSET SUM with target $W$ written in unary is in P (DP); in binary it is NP-complete. PRIMES with $N$ in binary has input length $\log N$; trial division is exponential in the input length.
- Equisatisfiable is not equivalent: SAT $\to$ 3SAT adds variables; the reduction preserves the yes/no answer, not the set of models.
- Karp vs Turing reductions: "solve $A$ by calling a $B$-oracle many times" is a Turing reduction. It suffices for "if $B \in \mathrm P$ then $A \in \mathrm P$" but NP-completeness is defined with Karp reductions.

## Exam-style questions

1. *State the two definitions of NP and prove they agree.* See the theorem above: guess-and-verify NTM, and choice sequence as certificate; both directions need only that the polynomial bound on time bounds the certificate length.
2. *Show 3SAT $\le_p$ INDEPENDENT SET and give the instance for $(x \vee y \vee z)(\neg x \vee \neg y \vee z)$.* Two triangles, cross edges $x{-}\neg x$ and $y{-}\neg y$, $k = 2$. Both directions of the equivalence as in Results.
3. *Prove that VERTEX COVER is NP-complete.* Membership: certificate is the set $C$, $|C| \le k$, check every edge has an endpoint in $C$, $O(m)$. Hardness: INDEPENDENT SET $\le_p$ VERTEX COVER via $(G, k) \mapsto (G, n - k)$, complement argument; IS is hard by the triangle reduction from 3SAT.
4. *Why do the base-4 digits in VC $\le_p$ SUBSET SUM never carry, and why does the reduction not contradict the $O(nW)$ DP for SUBSET SUM?* Each digit sums to at most $1 + 1 + 1 = 3$. $W \approx k \cdot 4^m$ is exponential in the instance size $m$, so $O(nW)$ is exponential; pseudo-polynomial, not polynomial.
5. *If someone found a polynomial algorithm for 3-COLOURING, what follows for FACTORING?* 3-COLOURING is NP-complete, so P = NP; FACTORING (decision version) is in NP, hence in P. Conversely a polynomial FACTORING algorithm (e.g. Shor on a quantum computer) says nothing about P vs NP, since FACTORING is not known to be NP-hard.
6. *Sketch why the Cook-Levin formula has polynomial size.* Tableau of $p(n) \times p(n)$ cells, each with $O(1)$ variables; window constraints are $O(1)$ clauses per cell; total $O(p(n)^2)$.

## 2026W lectures: introduction (12.10), chapter 1 (28.10), chapter 2 (30.10) [S62]

Pichler's complexity block opens with an introduction to computability and complexity, then chapter 1 (Turing machines and complexity classes) and chapter 2 (propositional logic and NP). In our words, with what differs from the sections above:

- **Model of computation in the introduction.** Not a RAM but a small imperative language (typed variables, assignment, if, while, return). It is as strong as any real language because it can host an interpreter, so "no program in it" means "no algorithm at all". Problem types: decision, function, optimisation, enumeration, counting, search. A problem needs infinitely many instances, otherwise a lookup table solves it.
- **Halting is undecidable** by diagonalisation: from a supposed decider build a program that loops exactly when its input program halts on its own code, and run it on itself. Further undecidable problems follow by computable reductions from Halting (example: does a program map input $I_1$ to output $I_2$; reduce by a program that first runs $\Pi$ on its input and then returns the input unchanged).
- **Reductions.** Turing reductions call a subroutine for $B$ arbitrarily often (polynomial time: Cook); many-one reductions map each instance to one instance of $B$ and take the answer as is (polynomial time: Karp). Complexity work uses polynomial time or logarithmic space reductions, so that the reduction does not hide the difficulty. C-hard and C-complete are defined with many-one reductions. If one NP-complete problem is in P, then P = NP.
- **Turing machine as in the lectures.** $M = (K, \Sigma, \delta, s)$ with $\delta : K \times \Sigma \to (K \cup \{h, \text{yes}, \text{no}\}) \times \Sigma \times \{\leftarrow, \rightarrow, -\}$; the tape is infinite to the right only, starts with the left marker $\triangleright$ (never overwritten, the head bounces off it) followed by the input; a configuration is a triple (state, string left of and including the head, string right of the head). Numbers are encoded in binary.
- **Variants.** A $k$-string machine running in time $f(n)$ is simulated by one string in $O(f(n)^2)$; a two-way infinite tape is simulated in $O(f(n))$ by folding the cells $z \ge 0$ to odd and $z < 0$ to even positions. Palindromes take $\Theta(n^2)$ on one string and $\Theta(n)$ on two. For sublinear space the machine gets a read-only input string and a write-only output string, and only the strings in between count.
- **NTMs.** Transition relation instead of function. Acceptance is asymmetric: accept if some branch says yes, reject only if no branch does. $\mathrm{NTIME}(f) \subseteq \bigcup_{c>1}\mathrm{TIME}(c^{f(n)})$ by exploring the computation tree (branching bounded by a constant). The two definitions of NP agree: guess a certificate and check it, or take an accepting computation of polynomial length as the certificate. Nondeterminism reads as guess and check.
- **Complements.** co-C consists of the complements of problems in C; a many-one reduction from $A$ to $B$ is also one from co-$A$ to co-$B$. Deterministic classes equal their co-classes; NP = coNP is open and believed false.
- **Propositional logic in chapter 2.** Syntax, truth assignments, the derived connectives $\to, \leftrightarrow, \oplus$, Boolean functions and truth tables, the laws of a Boolean algebra; $\{\wedge, \vee, \neg\}$ is functionally complete. $\varphi$ is valid iff $\neg\varphi$ is unsatisfiable.
- **Cook-Levin as proved in the lectures.** Not tableau windows but atoms $\mathit{symbol}_\sigma[\tau, \pi]$, $\mathit{cursor}[\tau, \pi]$ and $\mathit{state}_s[\tau]$ for times and cells up to $N = q(|x|)$, with five groups of conjuncts: initialisation, transition rules (state, symbol and cursor at time $\tau$ imply one of the allowed successors at $\tau + 1$), uniqueness (exactly one state, cursor position and symbol per cell), inertia (cells away from the cursor keep their symbol), and acceptance (state yes at time $N$).
- **VALIDITY** is coNP-complete (complement of SAT up to negating the formula). SAT on DNF and VALIDITY on CNF are in P; VALIDITY stays coNP-complete on 3-DNF.
- **Normal forms.** Rewriting to CNF or DNF by eliminating $\to, \leftrightarrow$, pushing negations inward and distributing can blow up exponentially (CNF of $(x_1 \wedge y_1) \vee \dots \vee (x_n \wedge y_n)$). A sat-equivalent CNF is polynomial: replace $(A \wedge B) \vee C$ by $(z \vee A) \wedge (z \vee B) \wedge (\neg z \vee C)$ with fresh $z$. To 3-CNF: replace a clause $l_1 \vee l_2 \vee l_3 \vee R$ by $(z \vee l_1 \vee l_2) \wedge (\neg z \vee l_3 \vee R)$ and repeat. 2-SAT is in P (via reachability, B03).
- **NP-complete problems in the lectures.** 3-SAT, INDEPENDENT SET (reduction from 3-SAT as in Results, with both directions proved), CLIQUE ($I$ independent in $G$ iff $I$ is a clique in the complement graph), VERTEX COVER ($I$ independent iff $V \setminus I$ is a cover), $k$-COLORABILITY for fixed $k \ge 3$ (2-COLORABILITY is in P by propagating colours through a connected graph), HAMILTON PATH, HAMILTON CYCLE, TSP as a decision problem. Not in the 2026W decks: SUBSET SUM and Ladner's theorem.

## Code

`src/py/complexity/sat.py`: `brute_force_sat`, `dpll` (cross-check), `sat_to_3sat` (clause splitting), `three_sat_to_vertex_cover` (returns vertices, edges, $k_{IS}$, $k_{VC}$), `assignment_to_independent_set`, `independent_set_exists`, `vertex_cover_exists`, `vertex_cover_to_subset_sum` (base-4 construction), `subset_sum_exists` (exact pruned search). `test_sat.py` checks on 30 seeded tiny CNFs that satisfiability is preserved along SAT $\to$ 3SAT $\to$ IS/VC $\to$ SUBSET SUM and that a satisfying assignment maps to an actual cover and subset.

## Cards

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

```card id=qc-cx1-vc-subset-sum
In VC to SUBSET SUM, why are base-4 digits used, and why is there no contradiction with the $O(nW)$ DP?
---
Each digit sums to at most 3, so no carries.
$W\approx k\cdot4^m$ is exponential in the instance size; the DP is only pseudo-polynomial.
```

```card id=qc-cx1-ladner
What does Ladner's theorem say, and what are candidate examples?
---
If P $\ne$ NP there are NP problems neither in P nor NP-complete (NP-intermediate).
Candidates: FACTORING, GRAPH ISOMORPHISM.
```

```card id=qc-cx1-factoring
Why does Shor's algorithm not imply NP $\subseteq$ BQP?
---
FACTORING (decision) is in NP $\cap$ coNP and not known to be NP-hard.
An NP-complete problem in coNP would give NP = coNP.
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
