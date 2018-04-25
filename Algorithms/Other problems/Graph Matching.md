# Graph Matching

## Problem Description

Graph matching consists of selecting a subset of edges M of a graph G(V, E) so that no two edges share the same vertex. Most of the time, we are interested in the Maximum Cardinality matching, i.e. we want to know the maximum number of edges that we can match in graph G. Another common request is the Perfect matching where we have both Maximum Cardinality matching and no vertex is left unmatched. Note that if V is odd, it is impossible to have a Perfect matching. Perfect matching can be solved by simply looking for Maximum Cardinality and then checking if all vertices are matched.

There are two important attributes of graph matching problems in programming contests that can significantly alter the level of difficulty: Whether the input graph is bipartite (harder otherwise) and whether the input graph is unweighted (harder otherwise). These two characteristics create four variants as outlined below as well as in the figure.

1) **Unweighted Maximum Cardinality Bipartite Matching (Unweighted MCBM)**: This is the easiest and the most common variant.
2) **Weighted Maximum Cardinality Bipartite Matching (Weighted MCBM)**: This is a similar problem as above, but now the edges in G have weights. We usually want the MCBM with the minimum total weight.
3) **Unweighted Maximum Cardinality Matching (Unweighted MCM)**: The graph is not guaranteed to be bipartite.
4) **Weighted Maximum Cardinality Matching (Weighted MCM)**: This is the hardest variant.

![alt text](https://i.imgur.com/NmpJbDb.png)

## Solution(s)

### Solutions for Unweighted MCBM

This variant is the easiest. The list below summarizes three possible solutions for the Unweighted MCBM problems:

1) Reducing the Unweighted MCBM problem into a Max Flow Problem. The time complexity depends on the chosen Max Flow algorithm.
2) O(V^2 + VE) Augmenting Path Algorithm for Unweighted MCBM. This is good enough for various contest problems involving Unweighted MCBM.
3) O(√V E) Hopcroft Karp’s Algorithm for Unweighted MCBM.

### Solutions for Weighted MCBM

When the edges in the bipartite graph are weighted, not all possible MCBMs are optimal. We need to pick one (not necessarily unique) MCBM that has the minimum overall total weight. One possible solution is to reduce the Weighted MCBM problem into a Min Cost Max Flow (MCMF) problem.

For example, in the following figure, we have a MCBM problem on a complete bipartite graph K(n,m), but each edge has an associated cost. We add edges from source _s_ to vertices of the left set with capacity 1 and cost 0. We also add edges from vertices of the right set to the sink _t_ also with capacity 1 and cost 0. The directed edges from the left set to the right set has capacity 1 and a cost. After having this weighted flow graph, we can run the MCMF algorithm to get the required answer: Flow 1 = 0 → 2 → 4 → 8 with cost 5, Flow 2 = 0 → 1 → 4 → 2 (cancel flow 2-4) → 6 → 8 with cost 15, and Flow 3 = 0 → 3 → 5 → 8 with cost 20. The minimum total cost is 5 + 15 + 20 = 40.

![alt text](https://i.imgur.com/pY4q5u3.png)

### Solutions for Unweighted MCM

While the graph matching problem is easy on bipartite graphs, it is hard on general graphs. In the past, computer scientists thought that this variant was another NP-Complete problem until Jack Edmonds published a polynomial algorithm for solving this problem in his 1965 paper titled “Paths, trees, and flowers”.

The main issue is that on general graphs, we may encounter odd-length augmenting cycles. Edmonds calls such a cycle a _blossom_. The key idea of Edmonds Matching algorithm is to repeatedly shrink these _blossoms_ (potentially in recursive fashion) so that finding augmenting paths returns back to the easy case as in bipartite graph. Then, Edmonds matching algorithm readjusts the matchings when these _blossoms_ are re-expanded (lifted).

The implementation of Edmonds Matching algorithm is not straightforward. Therefore, to make this graph matching variant more manageable, many problem authors limit the size of their unweighted general graphs to be small enough, i.e. V ≤ 18 so that an O(V × 2^V) DP with bitmask algorithm can be used to solve it.

### Solution for Weighted MCM

This is potentially the **hardest** variant. The given graph is a general graph and the edges have associated weights. In typical programming contest environments, the most likely solution is the DP with bitmask as the problem authors usually set the problem on a **small general graph** only.
