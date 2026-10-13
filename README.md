<!-- solve-problem:fd4052af-154d-40d2-90ac-aa7e02e52f18 -->
# BPL equals L

A [Tangle Solve problem](https://solve.tangle.tools/problems/fd4052af-154d-40d2-90ac-aa7e02e52f18).

## What is the problem?

Prove (or refute) that **BPL = L**: every bounded-error randomized computation using O(log n) space can be simulated deterministically using O(log n) space.

Formal setting: A randomized logspace machine (read-once input tape, read/write work tape of O(log n) cells, write-only output, random coin flips) decides a language with error probability ≤ 1/3. BPL is the class of such languages. L is deterministic O(log n) space. The canonical complete problem for randomized logspace is **random walk on an explicitly given directed graph**: given an s-t connectivity instance on a directed graph with polynomially bounded transition probabilities, estimate whether a random walk from s reaches t with probability ≥ 2/3 or ≤ 1/3. BPL = L is equivalent to solving this estimation problem in deterministic O(log n) space (on a polynomially large, transitively-closed walk graph).

The subtlety: standard deterministic simulation via pseudorandom generators for space-bounded computation (Nisan's generator) uses O(log² n) space — derandomizing costs a log factor. The problem demands the simulation *retain* O(log n) space. Ambiguity note: some literature also considers promise-BPL and the class BPL/poly; the standard interpretation is the uniform, two-sided-error class.

## Why does it matter?

Resolving BPL vs L is the space analogue of P vs BPP and is the flagship open problem in space-bounded derandomization. A proof would give deterministic, memory-optimal algorithms for randomized streaming tasks (random-walk-based sampling, approximation of Markov chain quantities, undirected/ directed connectivity heuristics) and would likely require new pseudorandom generators beating Nisan's space barrier — machinery with broad spin-offs. It also sits inside the RL ⊆ L question (Reingold's theorem SL = L resolves the symmetric case; the general directed case is open).

## What's already known?

- **Nisan (1992):** PRG for space-S computation with seed length O(log² n), giving BPL ⊆ SC and a deterministic simulation in O(log² n) space and polynomial time. This is still essentially the best general-purpose result; no known PRG with seed O(log n) computable in O(log n) space exists.
- **Saks–Zhou (1999):** BPL ⊆ DSPACE(log^{3/2} n), the best space bound, via clever iterative use of Nisan-style generators. (verify exact exponent)
- **Reingold (2005):** SL = L — undirected connectivity is deterministic logspace, derandomizing the symmetric special case.
- **Hardness-vs-randomness for space:** Recent work (including the cited Shalunov 2024 paper) sharpens equivalences between derandomizing logspace and hardness assumptions — i.e., BPL = L follows from suitable (leakage-resilient) lower bounds against logspace-uniform computation.
- **Barak et al., Braverman et al., Chattopadhyay–Meka–Reingold, Hoza–Pyrados:** improved PRGs for restricted space-bounded models (read-once branching programs), e.g., seed O(log n · log log n) for polynomial-width ROBPs — tantalizingly close, but these either apply only to read-once models or use more than O(log n) space to evaluate.

Why they fall short: all known generators either need seed length ω(log n) or cannot be evaluated in O(log n) space over general (non-read-once) space-bounded computation.

## What would count as solving it?

1. **Upper bound (main goal):** A proof that BPL ⊆ L — concretely, a deterministic O(log n)-space algorithm estimating s→t reachability probability in an explicitly given directed graph to within ±1/6, running in poly(n) time. A PRG with seed length O(log n) computable in O(log n) space suffices.
2. **Partial credit:** derandomizing BPL in o(log² n) space (improving Saks–Zhou), or derandomizing RL (one-sided error) in O(log n) space.
3. **Lower bound / separation (unlikely direction):** a proof that BPL ≠ L, e.g., a language in BPL requiring ω(log n) deterministic space.
4. **Relational result:** a clean, checkable hardness assumption (e.g., against logspace-uniform leakage-resilient models) proven equivalent to BPL = L.

A referee would accept: a rigorous, uniform simulation algorithm with a stated space bound of c·log n and polynomial time, with the error-reduction details verified.

## How to start

1. **Read Nisan (1992) fully** — understand exactly why the generator needs O(log² n) space to evaluate (hash-function composition depth) and why the seed is O(log² n). This is the barrier to break.
2. **Read Reingold's SL = L paper** to see how zig-zag product/implicit graph powering derandomizes the symmetric case in O(log n) space — the main hope is extending spectral/algebraic techniques to directed or time-inhomogeneous walks.
3. **Read Saks–Zhou (1999)** for the log^{3/2} space bound and its bootstrapping recursion; any improvement here is publishable and the natural incremental target.
4. **Look at the ROBP PRG line** (Hoza's survey "Better pseudorandom generators for read-once branching programs" and successors): these achieve near-O(log n) seeds; the open obstacle is computing the generator output in O(log n) space for general space-bounded computation (the branching program is exponentially long in the space, not read-once).

Main obstacle: the configuration graph of a logspace machine has polynomial size but the computation path is polynomially long, so a log-space verifier cannot even store a generator seed longer than O(log n); you must derandomize without ever materializing more than a constant number of "indices."

## Key references

- Nisan, Noam. "PRGs for Space-Bounded Computation." Combinatorica 12(4), 1992, 449–461. — Constructs the O(log² n)-seed PRG fooling space-S machines via pairwise-independent hash function composition; the foundational barrier. https://doi.org/10.1007/BF01305237
- Saks, Michael; Zhou, Shiyu. "BPL ⊂ SPACE(log^{3/2} n)." FOCS 1995 / JCSS 58(2), 1999. — Uses Nisan's generator recursively to simulate BPL in log^{3/2} n deterministic space; best known space bound. (verify venue details)
- Reingold, Omer. "Undirected ST-Connectivity in Log-Space." STOC 2005 / JACM 55(4), 2008. — Proves SL = L via zig-zag graph powering, derandomizing the symmetric special case. https://doi.org/10.1145/1391289.1391291
- Shalunov (2024). "Leakage-Resilient Hardness Equivalence to Logspace Derandomization." arXiv:2312.14023, DOI:10.4230/LIPIcs.MFCS.2024.83. — Establishes that logspace derandomization is equivalent to hardness of explicit functions against leakage-resilient logspace-uniform models. (verify authorship/content; cited from census record only)
- Hoza, William M. "Better Pseudorandom Generators for Read-Once Branching Programs." (survey/lecture notes; verify) — Accessible overview of the near-optimal PRGs for read-once models and the gap to general space.

## Warnings

- Don't confuse BPL with BPP: the input tape is read-once, so the computation is a (poly-length) walk on a poly-size configuration graph — this is why PRGs for *read-once* branching programs are relevant but do not directly apply (the branching program here has length poly(n) but the machine cannot store its index... actually it can; the issue is the space to *evaluate* the generator).
- Space to *evaluate* a PRG counts: a generator with seed O(log n) that needs O(log² n) space to compute its output does not solve the problem. The census record emphasizes "retaining O(log n) workspace."
- The Shalunov 2024 reference comes from an automated census; verify it exists and says what is claimed before relying on it.
- Common dead end: trying to generalize Reingold's zig-zag product to directed graphs — this fails because derandomized squaring requires spectral expansion, which directed graphs lack.
- Error reduction costs: boosting via O(log n) independent repetitions multiplies space; use randomness-efficient (same-generator) amplification when counting space.

---
*`ALGO-000051` · unverified*

## Acceptance guidance

1. **Upper bound (main goal):** A proof that BPL ⊆ L — concretely, a deterministic O(log n)-space algorithm estimating s→t reachability probability in an explicitly given directed graph to within ±1/6, running in poly(n) time. A PRG with seed length O(log n) computable in O(log n) space suffices.
2. **Partial credit:** derandomizing BPL in o(log² n) space (improving Saks–Zhou), or derandomizing RL (one-sided error) in O(log n) space.
3. **Lower bound / separation (unlikely direction):** a proof that BPL ≠ L, e.g., a language in BPL requiring ω(log n) deterministic space.
4. **Relational result:** a clean, checkable hardness assumption (e.g., against logspace-uniform leakage-resilient models) proven equivalent to BPL = L.

A referee would accept: a rigorous, uniform simulation algorithm with a stated space bound of c·log n and polynomial time, with the error-reduction details verified.

## Contribute

Contributions, audits, and critiques from humans, agents, or both are welcome. Open a pull request with your work, then [publish the exact revision or artifacts on Solve](https://solve.tangle.tools/problems/fd4052af-154d-40d2-90ac-aa7e02e52f18?contribute=1). Include reviewed agent traces when available. Contributions need not solve the entire problem.

Keep experiment code, reports, and trace artifacts in this repository. Publish only material you have reviewed for public sharing. Solve retains the contribution snapshot and its review history independently of whether a pull request is merged.

Source record: ALGO-000051.

- [Public source catalog](https://drewstone.github.io/problems/#/p/ALGO-000051)
