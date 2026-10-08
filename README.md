# Network Centrality: An Interactive Demo

An in-browser demo of network centrality measures and their distributions. It asks which node is the "center" of a network, compares three answers (degree, closeness and betweenness), and shows why the whole distribution of centrality matters more than any single top node.

Created by Claude Opus 5.5, based on the lecture slides by Sang Hoon Lee.

## What's inside

The page is a single self-contained `index.html`. It has no build step. Its only outside resources are two Google Fonts (Source Serif 4 and IBM Plex Sans) and KaTeX 0.16.9 from cdnjs, which typesets the formulas. Without them the page falls back to system fonts and shows the formulas as plain TeX. All networks are generated and measured live in the browser. It shares its look and chart code with the companion scale-free networks demo.

The demo has six parts:

1. **Which node is the center?** A live force-directed drawing of one of four networks: airline hubs and spokes, two groups joined by a broker, a Barabási–Albert scale-free network, or an Erdős–Rényi random network. You choose whether to size nodes by degree k, closeness g̃ or betweenness b̃. A side table lists the top five nodes by each measure, and selecting a node shows its rank under all three. The "two groups" network is the clearest case where the measures disagree.
2. **Closeness.** The seven-node network from the slides, with node 3 as the degree hub. Picking a node writes its distance l_ij inside every other node and works out Σ l, g = 1/Σ l and g̃ = (N − 1)/Σ l step by step. Picking a destination draws every shortest path to it, so you can see where each term of the sum comes from. A second experiment grows networks from N = 30 to 30,000 to show that raw closeness collapses like 1/N (an extensive quantity), while the rescaled g̃ stays comparable across sizes.
3. **Betweenness.** A network with a dense group, a wheel, a small tree and one broker, modeled on the bottleneck figure in the slides. Picking two nodes j and k highlights every shortest path between them and labels each node in between with its share σ_jk(i)/σ_jk. You can switch between node and edge betweenness; the highest-betweenness link is marked as the bottleneck. Two more modes break a whole sum down: pick a node or a link and step through, or play back, every pair (h, j) whose shortest paths run through it. Each step shows σ_hj, how many of those paths pass through, the share σ_hj(i)/σ_hj, and a running total that ends at the betweenness computed by Brandes' algorithm. A table lists every pair, and you can include the pairs that contribute nothing. Two small examples reproduce Fig. 3.1 of Menczer, Fortunato and Davis: k₃ = 4 with b₃ = 3.5, and k₃ = 2 with b₃ = 4 × 5 = 20.
4. **Degree and betweenness usually agree.** A log–log scatter of betweenness against degree for every node of a 2,000-node network, with the average at each degree and the Spearman rank correlation. A third network option, 40 tight groups joined by a few links, shows low-degree nodes reaching the top 1% of betweenness.
5. **Centrality distributions.** A histogram of the chosen measure for the network at the top, with counts n on the left axis and fractions f = n/N on the right; betweenness and closeness are binned. A toggle plots e^−k and k^−3 on linear, semi-log and log–log axes. Cumulative distributions P(k) and P(b) compare random and scale-free networks of 2,000 or 5,000 nodes, with the γ = 3 prediction P(k) ∝ k^−2 and an option to overlay the noisy p(k).
6. **How wide is the distribution?** The heterogeneity parameter κ = ⟨k²⟩/⟨k⟩² as N grows from 100 to 100,000 for random networks, BA networks and sampled power-law degree sequences with a γ you choose. A bar chart places your 10,000-node networks among the real networks of Table 3.1.

## Running it

Open `index.html` in any modern browser. To host it with GitHub Pages, push this repository and enable Pages for the branch that contains `index.html`.

## Models and methods

- **Degree** is the number of neighbors, k_i = `len(G.neighbors(i))`.
- **Closeness** is computed from breadth-first searches. The page reports g̃_i = (N − 1)/Σ_j l_ij. On a network that is not connected it is scaled by the share of nodes reachable (the Wasserman–Faust correction used by NetworkX). The size experiment averages over 40 randomly chosen nodes per network.
- **Betweenness** is computed exactly with Brandes' algorithm and counts each unordered pair of endpoints once, so b̃_i = 2b_i/((N − 1)(N − 2)) matches `nx.betweenness_centrality(G)`. Edge betweenness is accumulated in the same pass.
- **Hero networks** keep only their largest connected component. The airline network has 4 interlinked hubs and 72 spokes; the two-group network has groups of 18 and 20 nodes joined through one broker node with a short tree attached.
- **Random networks** use the G(N, M) model with M = N⟨k⟩/2 links placed uniformly at random. **Scale-free networks** use Barabási–Albert preferential attachment with m = 2, so ⟨k⟩ ≈ 4 and γ → 3.
- **Power-law samples** in the κ panel draw degrees k = ⌊2u^(−1/(γ−1))⌋, capped at N − 1. Each point is the median of 21 samples.

## References

- F. Menczer, S. Fortunato, and C. A. Davis, *A First Course in Network Science* (Cambridge University Press, 2020), Chapter 3.
- K.-I. Goh, E. Oh, B. Kahng, and D. Kim, "Betweenness centrality correlation in social networks," *Phys. Rev. E* 67, 017101 (2003).
- U. Brandes, "A faster algorithm for betweenness centrality," *J. Math. Sociol.* 25, 163 (2001).
- D. V. Schroeder, *An Introduction to Thermal Physics* (Addison-Wesley, 2000), Section 5.2, for extensive and intensive quantities.
