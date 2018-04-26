# Min Path Cover on DAG

## Problem Description

The Min Path Cover (MPC) problem on DAG is described as the problem of finding the minimum number of paths to cover _each vertex_ on DAG _G = (V,E)_. A path _v0,v1,...,vk_ is said to cover all vertices along its path.

### Motivating problem

Imagine that the vertices in the figure _A_ below are passengers, and we draw an edge between two vertices _u − v_ if one taxi can serve passenger _u_ and then passenger _v_ on time. The question is: What is the minimum number of taxis that must be deployed to serve **all** passengers?

The answer is two taxis. In figure _D_, we see one possible optimal solution. One taxi (dotted line) serves passenger 1, passenger 2, and then passenger 4. Another taxi (dashed line) serves passenger 3 and passenger 5. All passengers are served with just two taxis. Notice that there is one more optimal solution: 1 → 3 → 5 and 2 → 4.

![alt text](https://i.imgur.com/nrja999.png)

## Solution(s)

This problem has a polynomial solution: Construct a _bipartite graph_ G′ = (Vout ∪ Vin, E′) from G, where Vout = {_v_ ∈ V : _v_ has positive out-degree}, Vin = {_v_ ∈ V : _v_ has positive in-degree}, and E′ = {(_u_, _v_) ∈ (Vout, Vin) : (_u_, _v_) ∈ E}. This G′ is a bipartite graph. A matching on bipartite graph G′ forces us to select at most one outgoing edge from every _u_ ∈ Vout (and similarly at most one incoming edge for _v_ ∈ Vin). DAG G initially has _n_ vertices, which can be covered with _n_ paths of length 0 (the vertices themselves). One matching between vertex _a_ and vertex _b_ using edge (_a_, _b_) says that we can use one less path as edge (_a_, _b_) ∈ E′ can cover both vertices in _a_ ∈ Vout and _b_ ∈ Vin. Thus if the MCBM in G′ has size _m_, then we just need _n − m_ paths to cover each vertex in G.

The MCBM in G′ that is needed to solve the MPC in G can be solved via several polynomial solutions, e.g. maximum flow solution, augmenting paths algorithm, or Hopcroft Karp’s algorithm. As the solution for bipartite matching runs in polynomial time, the solution for the MPC in DAG also runs in polynomial time. Note that MPC in general graph is NP-hard.

