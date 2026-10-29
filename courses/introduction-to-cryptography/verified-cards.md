# introduction-to-cryptography (verified cards)

Cards kept after the 2026W check against the downloaded lecture material. Unverified cards and prose were removed.

## 01-historical-ciphers

```card id=crypto-l1-syntax
Syntax of a private-key encryption scheme, and which algorithms may be randomised?
---
$(\mathsf{Gen},\mathsf{Enc},\mathsf{Dec})$ with $\mathsf{Dec}_k(\mathsf{Enc}_k(m))=m$ (correctness). Gen and Enc may be randomised; only Dec must be deterministic.
```

```card id=crypto-l1-pattern
What three steps does every primitive in the course follow, in order?
---
Syntax (the algorithms and which of them are randomised), then correctness (the scheme does its job), then security (what an adversary cannot learn or do).
```

```card id=crypto-l1-quadrants
How do the course topics split by secrecy versus integrity and private versus public key?
---
Private-key encryption (secrecy) and MACs (integrity) form the top half; public-key encryption and digital signatures the bottom. Block ciphers serve the private-key half, number theory the public-key half, and hash functions both.
```

```card id=crypto-l1-enc-deterministic
True or false: encryption must be deterministic so ciphertexts decrypt uniquely.
---
False. Only Dec must be deterministic. CPA-secure schemes need randomised Enc.
```

```card id=crypto-l1-kerckhoffs
Kerckhoffs' principle, and why?
---
Enc and Dec are public; the secrecy of $m$ given $c$ rests only on the key. Why: keys are easier to keep secret (algorithms leak, e.g. by reverse engineering); a key is easier to replace than a scheme; and public schemes can be standardised (compatibility, public scrutiny).
```

```card id=crypto-l1-scytale
Scytale: what kind of cipher, what is the key, how is it broken?
---
Transposition: letters are moved, not replaced. Key = rod circumference = number of rows of the grid; at most $n$ candidates (divisors of the length): try them all. Letter frequencies stay exactly English.
```

```card id=crypto-l1-transposition-vs-substitution
Ciphertext letter frequencies match English letter by letter. Transposition or substitution?
---
Transposition. A substitution relabels letters, so the profile is permuted (only its sorted version matches).
```

```card id=crypto-l1-shift
Shift cipher: key space and two attacks.
---
$k\in\mathbb Z_{26}$, $c_i=m_i+k \bmod 26$ (Caesar: $k=3$). Brute force over 26 keys, or frequency analysis: pick $k$ maximising $\sum_i f_i\,p_{i+k}$.
```

```card id=crypto-l1-substitution
Monoalphabetic substitution: key space size, and why is it still broken?
---
$26!\approx 2^{88}$ permutations. It preserves letter frequencies (only relabelled): match ciphertext ranks to E, T, A, O, ..., then digrams TH, HE, IN.
```

```card id=crypto-l1-key-space
Sufficient key space principle: necessary or sufficient?
---
Necessary, not sufficient. Enumerable key space is fatal, but a huge one (substitution, $2^{88}$) still falls if the cipher leaks a statistic.
```

```card id=crypto-l1-vigenere
Vigenère: definition, and how to break it with known period $\ell$.
---
Polyalphabetic: position $i$ shifted by key letter $i \bmod \ell$. Split the ciphertext into $\ell$ residue classes; each is a shift cipher, break each by frequency analysis.
```

```card id=crypto-l1-poly-immune
True or false: unlike monoalphabetic ciphers, polyalphabetic ciphers are immune to frequency analysis.
---
False. Each residue class mod the period is monoalphabetic and falls to frequency analysis.
```

```card id=crypto-l1-vernam
Vernam cipher: key requirements and security.
---
Key uniform, as long as the message, used once; $c_i=m_i+k_i$ (mod 26, or XOR). Perfectly secret (one-time pad, lecture 2).
```

```card id=crypto-l1-key-reuse
Why does reusing a Vernam key break it?
---
$c_1-c_2=m_1-m_2$: the key cancels, leaving the difference of two plaintexts, which statistics recover.
```

```card id=crypto-l1-enc-randomised
Why should Enc be randomised (2026W lecture 1)?
---
One key is used for many messages. Deterministic Enc maps equal messages to equal ciphertexts, so the adversary learns that two messages are equal: information the ciphertext must not leak.
```

```card id=crypto-l1-gen-randomised
Why must Gen be randomised?
---
The scheme is public (Kerckhoffs). A deterministic Gen gives every user the same key.
```

```card id=crypto-l1-secret-designs
Two deployed secret ciphers that were reverse engineered and broken, and the lesson.
---
GSM's A5/1 and Mifare's Crypto-1 (transit cards). Use standardised, publicly analysed schemes; never your own.
```

```card id=crypto-l1-key-scale
Exhaustive key search: 48, 56, 128/256-bit keys?
---
48 bits (Mifare Crypto-1): a phone. 56 bits (DES): a laptop. 128 or 256 bits (AES): out of reach.
```

```card id=crypto-l1-rot13
Why is ROT13 its own inverse?
---
It shifts by 13 and $13 \equiv -13 \pmod{26}$, so encrypting twice shifts by 26, the identity.
```

```card id=crypto-l1-length
Does the one-time pad hide the message length?
---
No: the ciphertext is as long as the message. No scheme can hide the length of arbitrarily long messages.
```

```card id=crypto-l1-correctness-vs-security
Is the identity scheme ($\mathsf{Enc}_k(m)=m$, $\mathsf{Dec}_k(c)=c$) correct? How do you check correctness?
---
Yes, correct (and completely insecure): correctness and security are separate properties. Check by plugging Enc into Dec: $\mathsf{Dec}_k(\mathsf{Enc}_k(m))=m$ must hold for every key and every message. One key or message where it fails is a counterexample, e.g. Dec adding $k$ instead of subtracting works only for $k\in\{0,13\}$.
```

```card id=crypto-l1-scytale-spec
Specify the Scytale over $\Sigma^n$: Gen, Enc and Dec.
---
Gen: pick the rod size $k\in\{1,\dots,n\}$ uniformly. Enc: write the message row by row into $k$ rows (padding the last row), read it out column by column. Dec: write the ciphertext column by column into the same grid, read it row by row and drop the padding.
```

```card id=crypto-l1-scytale-transpose
Why is the Scytale a transposition cipher?
---
Write the message row by row into a grid with $k$ rows, read it out column by column: the ciphertext is the transpose of the matrix. Letters only change positions. Other transpositions (rail fence, routes, keyed columnar) use other rearrangements but all keep the letter multiset.
```

```card id=crypto-l1-transposition-never-secret
Can a transposition cipher be perfectly secret, even with a random permutation per message?
---
No. Every transposition keeps the multiset of letters, so messages with different letters are always distinguishable. More possible permutations only make brute force harder.
```

```card id=crypto-l1-vigenere-vs-vernam
How are Vigenère and Vernam related, and which way does security transfer?
---
Vigenère with period $\ell$ is Vernam with the restriction $k_{i+\ell}=k_i$. Every Vigenère key is a Vernam key, so whatever Vigenère cannot distinguish, Vernam cannot either. Not the other way: the repetition is what attacks exploit.
```

```card id=crypto-l1-vernam-any-to-any
Why does a Vernam ciphertext say nothing about the message?
---
For any message $m$ and any ciphertext $c$ of the same length, the key $k_i=c_i-m_i \bmod 26$ maps $m$ to $c$, and exactly one key does. With a uniform key every message is equally likely for every ciphertext.
```

```card id=crypto-l1-permutation-25
Chosen plaintext against a substitution with $\ell$ permutations of a 26-letter alphabet: how many letters reveal the key?
---
$25\ell$. Each permutation is a bijection, so 25 known images fix the 26th. Place each of 25 letters once in every residue class mod $\ell$ (for $\ell=2$: AABB...YY, 50 letters).
```

```card id=crypto-l1-lesson
Lecture 1's lesson in one line.
---
A cipher is broken when it leaks a statistic, whatever its key space; so from lecture 2 on: definitions and proofs, not designs that look hard.
```

## 02-perfect-secrecy

```card id=crypto-l2-provable-security
The three ingredients of provable security.
---
A definition (adversary's goal and threat model), an assumption (a precisely stated problem believed hard), and a proof that the scheme meets the definition under the assumption.
```

```card id=crypto-l2-reduction-direction
A security proof by reduction: what is turned into what?
---
Any adversary breaking the scheme is turned into an efficient algorithm for the hard problem (e.g. factoring). So breaking the scheme is at least as hard as the problem.
```

```card id=crypto-l2-why-assumptions
Why does practical cryptography need hardness assumptions?
---
Security of any interesting scheme implies P $\ne$ NP, which is unproven. Only information-theoretic schemes like the one-time pad avoid assumptions, at a high price.
```

```card id=crypto-l2-average-case
Why is NP-hardness the wrong kind of hardness for cryptography?
---
NP-hardness is worst case. A randomly generated key must give a hard instance, so cryptography needs average-case hardness. (Factoring is not even known to be NP-complete.)
```

```card id=crypto-l2-proof-failure
Two ways a provably secure scheme can still fail.
---
The assumption is false (e.g. quantum computers factor), or the definition misses a real requirement (e.g. it allows one ciphertext per key and the key is reused). The second is more common.
```

```card id=crypto-l2-bad-goals
Why are "find the key" and "recover the plaintext" bad security goals?
---
$\mathsf{Enc}_k(m)=m$ hides the key; encrypting half the message and sending the rest in clear hides the full plaintext. The goal must be: no information about the plaintext.
```

```card id=crypto-l2-adversary-powers
Adversary powers in increasing order (lecture 2).
---
One ciphertext; many ciphertexts; known plaintext-ciphertext pairs; chosen plaintexts (encryption queries); chosen ciphertexts (decryption queries).
```

```card id=crypto-l2-perfect-secrecy
Def. 2.3: perfect secrecy.
---
For every distribution over $\mathcal M$, every $m$ and every $c$ with $\Pr[C=c]>0$: $\Pr[M=m\mid C=c]=\Pr[M=m]$.
```

```card id=crypto-l2-example-shift
Example 2.1: one letter, shift cipher, $\Pr[M=B]=0.7$, $\Pr[M=D]=0.3$. What is $\Pr[C=F]$?
---
$0.7\cdot\frac1{26}+0.3\cdot\frac1{26}=\frac1{26}$ (B with key 4, D with key 2, $K$ and $M$ independent). The same for any message distribution and any letter.
```

```card id=crypto-l2-otp-proof
Proof skeleton of Thm 2.10 (one-time pad perfectly secret).
---
(*) $\Pr[K=k]=2^{-\ell}$. (**) $\Pr[C=c\mid M=m]=\Pr[K=m\oplus c]=2^{-\ell}$ by independence. (***) total probability: $\Pr[C=c]=2^{-\ell}$. Bayes: $\Pr[M=m\mid C=c]=\Pr[M=m]$.
```

```card id=crypto-l2-independence
Which assumption about $K$ and $M$ does the one-time pad proof need, and where?
---
$K$ and $M$ independent: it lets $\Pr[K=m\oplus c\mid M=m]$ become $\Pr[K=m\oplus c]$.
```

```card id=crypto-l2-key-reuse
One-time pad key used twice: what leaks?
---
$c\oplus c'=m\oplus m'$, the key cancels. With small or structured message spaces (B/S and Y/N) this reveals both messages.
```

```card id=crypto-l2-otp-drawbacks
Practical drawbacks of the one-time pad.
---
Key as long as the message and usable once: needs lots of true randomness, secure distribution of the key material in advance, and its destruction afterwards.
```

```card id=crypto-l2-thm-2-11
Thm 2.11.
---
Every perfectly secret scheme has $|\mathcal K|\ge|\mathcal M|$.
```

```card id=crypto-l2-shannon
Thm 2.12 (Shannon), both conditions.
---
For $|\mathcal M|=|\mathcal K|=|\mathcal C|$: perfectly secret iff every key has probability $1/|\mathcal K|$ AND for every $m,c$ there is a unique $k$ with $\mathsf{Enc}_k(m)=c$.
```

```card id=crypto-l2-modularity
Why do precise security definitions matter beyond one scheme?
---
Each layer can assume the one below as a black box: block cipher, then symmetric encryption, then authenticated encryption, then TLS. A designer relies on the definition of each layer, not on its internals.
```

```card id=crypto-l2-tradeoff
Why pick a weaker security notion or a weaker assumption on purpose?
---
Stronger notions and stronger assumptions usually cost efficiency. If the weaker notion suffices for the setting, a more efficient scheme can be used.
```

```card id=crypto-l2-terminal
Why is a chosen-plaintext attack a realistic model?
---
A shared terminal on an encrypted line encrypts whatever a user types. An attacker can choose plaintexts and watch the ciphertexts sent by others on the same line, so later ciphertexts must stay secure.
```

```card id=crypto-l2-refute
How do you refute perfect secrecy of a scheme?
---
Fix a message distribution giving two messages positive probability, then find a ciphertext $c$ and message $m$ with $\Pr[C=c]>0$ but $\Pr[M=m\mid C=c]\neq\Pr[M=m]$. Example: $\Pr[M=m\mid C=c]=0$ when $c$ cannot be an encryption of $m$. One such pair suffices.
```

```card id=crypto-l2-zero-key
Is the one-time pad over $\{0,1\}^\ell$ without the all-zero key perfectly secret?
---
No. Take $m=0\ldots0$ with $\Pr[M=m]>0$: the ciphertext $0\ldots0$ needs key $0\ldots0$, which is excluded, so $\Pr[M=m\mid C=c]=0\neq\Pr[M=m]$. Also $2^\ell-1<2^\ell$ keys violates Thm 2.11.
```

```card id=crypto-l2-group
Which property of XOR makes the one-time pad work, and how does it generalise?
---
$\{0,1\}^\ell$ with XOR is a group: associative, identity $0\ldots0$, every element its own inverse. For any finite group, a uniform key $k$, $c=m+k$ and $m=c-k$ gives a perfectly secret scheme.
```

```card id=crypto-l2-not-uniform
Does perfect secrecy mean every message is equally likely given the ciphertext?
---
No. It means the posterior equals the prior, $\Pr[M=m\mid C=c]=\Pr[M=m]$, which may be far from uniform. Example: with $\Pr[M=\text{buy}]=0.7$, seeing any ciphertext still leaves buy at 0.7.
```

```card id=crypto-l2-bayes-equiv
Equivalent form of perfect secrecy: what must hold for two messages?
---
For all $m,m'$ with positive probability and every $c$: $\Pr[C=c\mid M=m]=\Pr[C=c\mid M=m']$, both equal to $\Pr[C=c]$. Proof: Bayes on each term, then the definition; $\Pr[C=c]=0$ is trivial.
```

## 03-block-ciphers

```card id=crypto-l3-generic-attacks
Two generic attacks on any block cipher, and the size requirement each forces.
---
Exhaustive key search: keys of at least 128 bits. Dictionary (codebook) attacks and statistics: blocks of at least 64 bits (AES: 128).
```

```card id=crypto-l3-ideal-cipher
What is the ideal cipher, and why is it not used?
---
Each key selects an independent uniformly random permutation of $\{0,1\}^\ell$. Describing one such permutation of 64-bit blocks takes about $2^{70}$ bits of key.
```

```card id=crypto-l3-ideal-vs-otp
Why is the ideal cipher not just a one-time pad?
---
One plaintext-ciphertext pair reveals the whole one-time-pad key ($k=m\oplus c$); for a random permutation one pair says almost nothing about the others.
```

```card id=crypto-l3-confusion-diffusion
Confusion and diffusion: meaning and which layer gives which.
---
Confusion: the plaintext/key-ciphertext relation is obscured, from the non-linear S-boxes. Diffusion: each output bit depends on many input bits, from the permutation/mixing layer.
```

```card id=crypto-l3-avalanche
Avalanche criterion.
---
Flipping one input bit should change at least half of the output bits.
```

```card id=crypto-l3-spn-key
In an SPN, why a fixed public S-box with the key XORed in, rather than secret random S-boxes?
---
Random S-boxes as the key are far too large (about 32 KB for 16 rounds of eight 8-bit boxes). $y_i=S(x_i\oplus k_i)$ keeps confusion with short round keys from one master key.
```

```card id=crypto-l3-feistel
Feistel round and its inverse.
---
$L_i=R_{i-1}$, $R_i=L_{i-1}\oplus f_{k_i}(R_{i-1})$. Inverse: $R_{i-1}=L_i$, $L_{i-1}=R_i\oplus f_{k_i}(L_i)$. $f$ need not be invertible.
```

```card id=crypto-l3-des-params
DES: block, key, rounds, structure.
---
64-bit block, 56-bit key (IBM's Lucifer had 128), 16 Feistel rounds, 1977.
```

```card id=crypto-l3-des-round
The DES round function $f$, in four steps.
---
Expand 32 to 48 bits; XOR the 48-bit round key; eight S-boxes 6 to 4 bits; permute the 32 bits. Not invertible, which Feistel allows.
```

```card id=crypto-l3-des-attacks
Differential vs linear cryptanalysis of DES, and the best practical attack.
---
Differential (Biham-Shamir 1990): $2^{47}$ chosen plaintexts. Linear (Matsui 1993): $2^{43}$ known pairs. Best in practice: exhaustive key search (1999 Deep Crack: 22 hours).
```

```card id=crypto-l3-mitm
Double encryption with two 56-bit keys: cost of meet-in-the-middle?
---
Tabulate $E_{k_1}(x)$ for all $k_1$, compare with $E^{-1}_{k_2}(y)$: $2^{56}$ memory and $2^{57}$ evaluations, not $2^{112}$.
```

```card id=crypto-l3-3des
Two-key Triple DES: formula, security, why DES$^{-1}$ in the middle?
---
$\mathsf{DES}_{k_1}(\mathsf{DES}^{-1}_{k_2}(\mathsf{DES}_{k_1}(x)))$, 112 key bits, 112-bit security (NIST: 80). With $k_1=k_2$ it collapses to single DES (backward compatibility). Discontinued for new implementations; kept for decryption of legacy data.
```

```card id=crypto-l3-aes-round
AES: key sizes, rounds, round steps and their roles.
---
128/192/256-bit keys, 10/12/14 rounds, 128-bit block. SubBytes (confusion), ShiftRows and MixColumns (diffusion), AddRoundKey; a key is also added before round 1.
```

```card id=crypto-l3-aes-sbox
How is the AES S-box defined, and how is it looked up?
---
Inverse in $\mathrm{GF}(2^8)=\mathbb F_2[X]/(X^8+X^4+X^3+X+1)$, then an affine map. Table lookup: high nibble = row, low nibble = column, e.g. $S(\{42\})=\{2c\}$.
```

```card id=crypto-l3-not-encryption
Is a block cipher an encryption scheme?
---
No. It is a deterministic permutation of one block; encryption needs a mode of operation on top, and a bad mode breaks everything.
```

```card id=crypto-l3-nsa-sboxes
What was questioned about the DES design, and what came out later?
---
The standard was reshaped during standardisation: the key shrank from 128 to 56 bits, and the S-boxes were chosen without published criteria. Later differential cryptanalysis showed the S-boxes resist that attack, which suggests the designers knew it in the 1970s.
```

```card id=crypto-l3-des-vs-aes
Name three differences between DES and AES.
---
DES: 64-bit block, 56-bit key, Feistel network, eight different S-boxes, secret design choices, optimised for hardware. AES: 128-bit block, 128/192/256-bit keys, substitution-permutation network with one S-box, open competition, optimised for byte-oriented software.
```

```card id=crypto-l3-aes-margin
What is a security margin, and where does AES stand?
---
The number of rounds beyond the best known attack. AES-128 has 10 rounds; the best attacks reach about 5 to 6 rounds. The best full-round attack (biclique, $2^{126.2}$ calls) is barely below the $2^{128}$ generic search. The related-key attack ($2^{99.5}$) is in a hypothetical model.
```

```card id=crypto-l3-feistel-hw
Why is the Feistel structure convenient for hardware?
---
Decryption uses the same components in reverse order with the XOR rewired, so one circuit serves both directions. The round function $f$ need not be invertible.
```

## lecture 4, 2026W

```card id=crypto-l4-stream-enc
How does a stream cipher encrypt and decrypt, compared with the one-time pad?
---
A keystream $s_1,s_2,\dots$ is generated from a short seed and XORed bit by bit: $c_i=m_i\oplus s_i$, and decryption is $m_i=c_i\oplus s_i$. It is the one-time pad with a pseudorandom pad in place of a truly random key.
```

```card id=crypto-l4-lfsr
Why is a linear-feedback shift register a poor keystream generator on its own?
---
It is very efficient in hardware, but its output is linear in its state, and linearity gives attackers equations to solve.
```

```card id=crypto-l4-keystream-sources
Name the keystream sources lecture 4 lists besides LFSRs.
---
Combinations of LFSRs (A5/1 for GSM), RC4 (used in WEP), and block cipher modes of operation, which are covered later in the course.
```

```card id=crypto-l4-perfect-cost
What two limits does perfect secrecy impose, and why are they impractical?
---
The key must be as long as the message and used only once (Shannon). Such keys are not practical, so the course moves to computational notions.
```

```card id=crypto-l4-two-relaxations
Why must the two relaxations of perfect security be made together?
---
Restrict the adversary to probabilistic polynomial time and allow a negligible success probability. Restricting only the resources does not get past Shannon's limits, and allowing only a small error does not either.
```

```card id=crypto-l4-eav-experiment
Describe the eavesdropper indistinguishability experiment for a private-key scheme.
---
The adversary submits two messages $m_0,m_1$ of equal length. The challenger generates a key $k\leftarrow\mathsf{Gen}$, picks a uniform bit $b$, and returns $c\leftarrow\mathsf{Enc}_k(m_b)$. The adversary outputs $b'$ and wins iff $b'=b$.
```

```card id=crypto-l4-perfect-indist
What success probability does perfect indistinguishability allow the adversary in the eavesdropper experiment?
---
Exactly $\tfrac12$ for every adversary, the same as guessing the bit at random.
```

```card id=crypto-l4-equivalence
Are perfect secrecy and perfect indistinguishability equivalent?
---
Yes (lecture 4, Lemma 2.7). The direction from perfect indistinguishability to perfect secrecy is the less intuitive one and needs a reduction.
```

```card id=crypto-l4-concrete
What does a $(t,\varepsilon)$-indistinguishable scheme guarantee?
---
Every adversary running in time at most $t$ wins the eavesdropper experiment with probability at most $\tfrac12+\varepsilon$.
```

```card id=crypto-l4-concrete-limits
What is the main strength and the main weakness of the concrete $(t,\varepsilon)$ approach?
---
Strength: the numbers speak directly to real applications. Weakness: it does not say how security scales when the key grows, and it does not fix the machine model that counts time.
```

```card id=crypto-l4-asymptotic
In the asymptotic approach, what is the security parameter and what depends on it?
---
A parameter $n$ known to everyone, roughly the key length. The running times of the honest parties and the adversary, and the success probability, are all functions of $n$. The adversary is told $n$.
```

```card id=crypto-l4-ppt
Why use probabilistic polynomial time as the model for honest parties and adversaries?
---
By the strong Church-Turing thesis, any physically realisable computation can be simulated on a Turing machine with at most polynomial slowdown. Polynomial time is therefore independent of the device, and randomised algorithms are included.
```

```card id=crypto-l4-negligible
Define a negligible function.
---
$f:\mathbb N\to\mathbb R^+$ is negligible if for every positive polynomial $p$ there is an $N$ such that $f(n)<1/p(n)$ for all $n>N$. Examples: $2^{-n}$ and $2^{-\sqrt n}$.
```

```card id=crypto-l4-closure
Which closure properties of polynomials and negligible functions do security proofs use?
---
Products and compositions of polynomials are polynomial. A polynomial times a negligible function is negligible. So a polynomial number of subroutine calls keeps both the running time and the total failure probability small.
```

```card id=crypto-l4-worked-example
Lecture 4 worked example: honest cost $10^6n^2$ cycles, adversary $10^8n^4$ cycles with success $2^{-n/2}$. What changes between $n=80$ at 2 GHz and $n=160$ at 8 GHz?
---
The honest run takes 3.2 seconds in both cases. The adversary needs about three weeks at $n=80$ for success $2^{-40}$, and about 13 weeks at $n=160$ for success $2^{-80}$.
```

```card id=crypto-l4-revised-syntax
What does the revised syntax of a private-key scheme require from Gen, Enc and Dec?
---
Gen takes the security parameter in unary, $1^n$, and returns a key of length at least $n$. Enc is probabilistic polynomial time and takes messages of any length. Dec is deterministic, without loss of generality.
```

```card id=crypto-l4-comp-indist
State computational indistinguishability for an eavesdropper.
---
For every probabilistic polynomial-time adversary there is a negligible $\varepsilon(n)$ with $\Pr[\mathrm{PrivK}=1]\le\tfrac12+\varepsilon(n)$, where the challenge messages have equal length.
```

```card id=crypto-l4-equal-length
Why must the challenge messages have equal length in the computational definition?
---
Otherwise the length alone identifies the chosen message. For example, a one-block and a two-block message are told apart from the ciphertext size, so no scheme could meet the definition.
```

```card id=crypto-l4-plaintext-length
Can an encryption scheme hide the plaintext length?
---
Not in general. Even the one-time pad reveals the length. Sensitive uses, such as yes/no answers, database searches and very small message spaces, need an extra mechanism at the protocol or scheme level.
```

```card id=crypto-l4-semantic
What does semantic security promise, and how does it relate to indistinguishability?
---
The ciphertext leaks nothing to any probabilistic polynomial-time adversary except with negligible probability. It is equivalent to computational indistinguishability (lecture 4, Theorem 3.13).
```

```card id=crypto-l4-eav-only
What does the lecture 4 indistinguishability definition not yet cover?
---
Chosen-plaintext and chosen-ciphertext attacks. It is an eavesdropper-only notion, the base for the stronger notions that follow.
```

```card id=crypto-l4-partial-leak
Is recovering one letter of the plaintext enough to call a scheme broken?
---
Yes. Security means the adversary learns no information about the plaintext, and any dependency that leaks, even from an XOR, counts as a break.
```

## lecture 5, 2026W

```card id=crypto-l5-randomness
Is a single bitstring random?
---
Randomness is a property of a distribution, not of one string. Under the uniform distribution on $\{0,1\}^{16}$ every string has probability $2^{-16}$, including the all-zero string.
```

```card id=crypto-l5-all-tests
Why must cryptographic pseudorandomness pass all efficient tests?
---
The adversary's test is unknown in advance, so a distribution is pseudorandom only if every efficient test accepts it with nearly the same probability as the uniform distribution.
```

```card id=crypto-l5-pseudorandom-def
Define a pseudorandom sequence of distributions, asymptotically and concretely.
---
Asymptotically: for every probabilistic polynomial-time test $A$ there is a negligible $\varepsilon$ such that the acceptance probabilities on $D_n$ and on the uniform distribution differ by at most $\varepsilon(n)$. Concretely: tests running in time at most $t$ differ by at most $\varepsilon$.
```

```card id=crypto-l5-prg-def
Define a pseudorandom generator.
---
A deterministic polynomial-time algorithm $G$ with output length $\ell(|x|)$ for all seeds $x$, where $\ell(n)>n$ (it expands), and where the outputs $G(U_n)$ form a pseudorandom sequence.
```

```card id=crypto-l5-prg-indist
Restate PRG security as indistinguishability.
---
A uniform seed $x\in\{0,1\}^n$ expanded by $G$ and a uniform string $y\in\{0,1\}^{\ell(n)}$ are computationally indistinguishable: no probabilistic polynomial-time algorithm tells them apart with more than negligible advantage.
```

```card id=crypto-l5-prg-existence
Do pseudorandom generators exist, and what is known?
---
Unknown. Their existence would imply $\mathsf P\ne\mathsf{NP}$. They can be built from weaker assumptions, such as one-way functions, with factoring given as the example.
```

```card id=crypto-l5-reduction-shape
What is the shape of a security proof by reduction?
---
Assume the problem is hard. Suppose an efficient adversary breaks the construction. Build a reduction $A'$ that uses this adversary to solve the problem efficiently, which is a contradiction. $A'$ is probabilistic polynomial time whenever $A$ is.
```

```card id=crypto-l5-pseudo-otp
Define the pseudo one-time pad.
---
$\mathsf{Gen}(1^n)$ samples a uniform $k\in\{0,1\}^n$. For a message $m\in\{0,1\}^{\ell(n)}$, $\mathsf{Enc}_k(m)=G(k)\oplus m$, and $\mathsf{Dec}_k(c)=G(k)\oplus c$.
```

```card id=crypto-l5-pseudo-otp-thm
What does the pseudo one-time pad achieve, and under which assumption?
---
If $G$ is a pseudorandom generator, it is computationally indistinguishable against an eavesdropper. It is not perfectly secret: its key has $n$ bits while there are $2^{\ell(n)}$ messages with $\ell(n)>n$, so lecture 2 Theorem 2.11 rules out perfect secrecy.
```

```card id=crypto-l5-pseudo-otp-proof
How does the reduction prove the pseudo one-time pad secure?
---
$A'$ receives $y$, which is either $G(k)$ or uniform, and runs the eavesdropper experiment with $y$ as the pad, outputting 1 iff $A$ guesses $b$. With $G(k)$ this is the real experiment. With uniform $y$ the pad is a true one-time pad, so $A$ guesses with probability exactly $\tfrac12$. The advantage of $A$ is therefore at most the advantage of $A'$ against $G$, which is negligible.
```

```card id=crypto-l5-open-questions
What open questions does lecture 5 raise after the pseudo one-time pad theorem?
---
What if $G$ is not pseudorandom (the assumption fails), what if the definition is not strong enough, and what if the key is used more than once.
```
