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
