<!-- solve-problem:0a856639-f090-4e56-9678-900f59096015 -->
# Polylogarithmic fully dynamic exact global minimum cut

A [Tangle Solve problem](https://solve.tangle.tools/problems/0a856639-f090-4e56-9678-900f59096015).

## What is the problem?

Maintain the exact value (and ideally a witnessing cut) of the global minimum cut of an undirected unweighted graph G = (V, E) under an intermixed sequence of edge insertions and deletions, with worst-case (or amortized) update time polylogarithmic in n = |V| (and m = |E|). More precisely: preprocess G in polynomial time, then process each update in O(polylog n) time, answering queries "what is λ(G), the size of the minimum edge cut?" correctly after every update. Alternatively, prove a conditional or unconditional lower bound showing that polylog update time is impossible (e.g., under OMv/online matrix-vector multiplication or other fine-grained assumptions, or via cell-probe bounds), thereby establishing a definitive barrier for this model.

Ambiguities to note: (a) whether update time is worst-case or amortized; (b) whether only the *value* or also a *witness cut* must be maintained; (c) whether "polylogarithmic" excludes n^{o(1)}; (d) whether weighted graphs are in scope (the census says unweighted; the weighted case is strictly harder).

## Why does it matter?

The global min-cut measures network vulnerability — the smallest set of links whose failure disconnects the system — so a polylog-update exact algorithm would enable real-time reliability monitoring in networks undergoing churn. It is also a flagship open question in dynamic graph algorithms: connectivity and approximate min-cut now have near-optimal polylog dynamic algorithms, and min-cut is the natural next target; resolving it (in either direction) would significantly clarify the dynamic algorithms landscape. A lower bound would be the first strong separation between exact cut maintenance and connectivity in the fully dynamic setting.

## What's already known?

- Static exact min-cut is solved in near-linear time (Karger's randomized ~Õ(m); deterministic near-linear algorithms by Kawarabayashi–Thorup and Li, 2021). So preprocessing is not the issue.
- Fully dynamic connectivity has deterministic polylog worst-case update time (Holm–de Lichtenberg–Thorup) and randomized near-optimal bounds — the connectivity machinery (non-local search over expander decomposition-like structures) does not directly give cuts.
- Fully dynamic (1+ε)- and (2+ε)-approximate min-cut values can be maintained in polylog time (via dynamic sketching / cut sparsifiers, e.g., Goranci–Henzinger–Saranurak; Filtser–Krauthgamer–Yung for streaming/dynamic sketches). Exactness is what resists.
- For exact dynamic min-cut, known algorithms have update times polynomial or near-linear in n (decremental/incremental and fully dynamic exact algorithms by Ćopić–Gupta–Goranci–Räcke-type approaches; Thorup's dynamic min-cut for undirected graphs has update time ~Õ(n) per operation). The recent PhD thesis of Christiansen (2025) surveys and advances dynamic graph algorithms including min-cut (verify exact contributions — the census cites it as the key work).
- No polylog lower bound specific to exact min-cut is known; conditional lower bounds from OMv rule out polynomially-smaller-than-n^{2-o(1)} for some dynamic problems but do not separate exact min-cut from polylog.

## What would count as solving it?

Either direction resolves the census item:
1. **Upper bound:** A (randomized or deterministic) data structure maintaining the exact min-cut value (and witness) under edge insertions/deletions in amortized O(polylog n) time per update, with polynomial preprocessing. Randomized with polylog worst-case time is the strongest form; amortized polylog is acceptable.
2. **Lower bound:** A proof that exact fully dynamic min-cut requires n^{Ω(1)} (or at least ω(polylog)) update time, unconditionally in the cell-probe model, or under a standard fine-grained assumption (OMv, 3SUM, triangle detection), with an explicit reduction.

## How to start

1. Read Christiansen (2025), *Algorithms for Dynamic Graphs*, chapters on dynamic connectivity and min-cut, to get the current frontier precisely (verify structure). Then Karger's tree-packing theorem: O(log n) random spanning trees contain a min-cut with high probability — this is the backbone of static near-linear algorithms and the natural dynamic handle.
2. Main obstacle: min-cuts can change combinatorially with a single edge update (a deletion can merge or shift which cut is minimum), so no "monotone/local" repair strategy like HDT's connectivity levels works. The number of distinct near-minimum cuts (Karger's bound: n^{O(1/α)} cuts within factor α) is small for constant approximation but polynomial for exact α→1.
3. Natural approaches: (a) dynamically maintain Karger's tree packing plus dynamic tree cut structures (dynamic tree packing is itself open); (b) maintain a dynamic expander decomposition and recurse on the "irrelevant" parts — recent polylog dynamic connectivity uses this; (c) use dynamic cut sparsifiers (which *are* polylog-maintainable) plus an exact solver on the smaller sparsifier — fails because sparsifier errors swamp exactness; quantify this gap to seek a lower bound.
4. For a lower-bound attempt: try reducing online matrix-vector multiplication to distinguishing min-cut values in graphs whose min cuts encode inner products.

## Key references

- D. R. Karger. "Minimum cuts in near-linear time." JACM 47(1), 2000. Randomized near-linear min-cut via tree packing; the count of α-min-cuts. (No stable link; on Karger's MIT page.)
- M. Thorup. "Fully-dynamic min-cut." Combinatorica 27(1), 2007. Fully dynamic exact min-cut in ~Õ(n) update time for unweighted graphs — the baseline exact dynamic algorithm. (verify title details)
- M. Henzinger, S. Krinninger, D. Nanongkai. "Dynamic approximate all-pairs shortest paths" and related fine-grained works; for lower bounds see "Unconditional and conditional lower bounds for fully dynamic graph problems" (ICALP/Henzinger et al., 2015) — introduces OMv-based conditional lower bounds for dynamic problems. (verify)
- J. Holm, K. de Lichtenberg, M. Thorup. "Poly-logarithmic deterministic fully-dynamic algorithms for connectivity, minimum spanning tree, 2-edge, and biconnectivity." JACM 48(4), 2001. The HDT connectivity structure; polylog baseline for the easier problem.
- Christiansen, 2025. *Algorithms for Dynamic Graphs* (PhD thesis / survey). Current state of dynamic graph algorithms including min-cut developments; the census's designated key work. (verify availability and exact content)

## Warnings

- The census record is thin; its single "key work" citation should be verified before relying on it — I have not confirmed its precise contents.
- Do not conflate approximate and exact: several papers advertise "fully dynamic min-cut" with polylog time but maintain only (1+ε) or (2+ε) approximations via sparsifiers.
- Distinguish vertex min-cut (Karger-style vertex isolating cuts; recent static breakthroughs by Li–Panigrahi) from edge min-cut — the census means edge cuts.
- Update-time regime matters: Thorup's ~Õ(n) is per-operation amortized; claims of improvement must state worst-case vs. amortized clearly.
- The weighted-graph version may be strictly harder; the unweighted assumption is part of the problem statement.
- A "barrier" result must be careful: OMv-based lower bounds typically give n^{2-o(1)}-type bounds for dense problems and may not transfer to min-cut; a mere polylog lower bound claim under weak assumptions will face referee scrutiny.

---
*`ALGO-000126` · unverified*

## Acceptance guidance

Either direction resolves the census item:
1. **Upper bound:** A (randomized or deterministic) data structure maintaining the exact min-cut value (and witness) under edge insertions/deletions in amortized O(polylog n) time per update, with polynomial preprocessing. Randomized with polylog worst-case time is the strongest form; amortized polylog is acceptable.
2. **Lower bound:** A proof that exact fully dynamic min-cut requires n^{Ω(1)} (or at least ω(polylog)) update time, unconditionally in the cell-probe model, or under a standard fine-grained assumption (OMv, 3SUM, triangle detection), with an explicit reduction.

## Contribute

Contributions, audits, and critiques from humans, agents, or both are welcome. Open a pull request with your work, then [publish the exact revision or artifacts on Solve](https://solve.tangle.tools/problems/0a856639-f090-4e56-9678-900f59096015?contribute=1). Include reviewed agent traces when available. Contributions need not solve the entire problem.

Keep experiment code, reports, and trace artifacts in this repository. Publish only material you have reviewed for public sharing. Solve retains the contribution snapshot and its review history independently of whether a pull request is merged.

Source record: ALGO-000126.

- [Public source catalog](https://drewstone.github.io/problems/#/p/ALGO-000126)
