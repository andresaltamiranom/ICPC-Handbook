# Articulation Points and Bridges

**Motivating problem**: Given a road map (undirected graph) with cost associated to all intersections (vertices) and roads (edges), sabotage either a single intersection or a single road that has minimum cost such that the road network breaks down. This is a problem of finding the least cost Articulation Point (intersection) or the least cost Bridge (road) in an undirected graph (road map).

An _Articulation Point_ is defined as a vertex in a graph G whose removal disconnects G. A graph without any articulation points is called _Biconnected_. Similarly, a _Bridge_ is defined as an edge in a graph G whose removal disconnects G. These two problems are usually defined for undirected graphs, although they are still well defined for directed graphs.

To solve this problem, we can run a O(V+E) DFS once to identify all the articulation points and bridges.

This algorithm maintains two numbers: dfs_num(_u_) and dfs_low(_u_). Here, dfs_num(_u_) stores the iteration counter when the vertex _u_ is visited **for the first time**. The other number dfs_low(_u_) stores the lowest dfs_num reachable from DFS spanning sub tree of _u_. Initially dfs_low(_u_) = dfs_num(_u_) when vertex _u_ is first visited. Then, dfs_low(_u_) can only be made smaller if there is a cycle (some back edges exist). Note that we do not update dfs_low(_u_) with back edge (_u_, _v_) if _v_ is a direct parent of _u_.

See the following example:

![APaB](https://i.imgur.com/1IB8iM3.png)

In these two sample graphs, we run _articulationPointsAndBridges()_ with 0 as the starting node. Suppose in the graph in the left side, the sequence of visitation is 0 (at iteration 0) → 1 (1) → 2 (2) (backtrack to 1) → 4 (3) → 3 (4) (backtrack to 4) → 5 (5). See that these iteration counters are shown correctly in dfs_num. Since there is no back edge in this graph, all dfs_low = dfs_num.

In the graph in the right side, the sequence of visitation is 0 (at iteration 0) → 1 (1) → 2 (2) (backtrack to 1) → 3 (3) (backtrack to 1) → 4 (4) → 5 (5). There is an important back edge that forms a cycle, i.e. edge 5-1 that is part of cycle 1-4-5-1. This causes vertices 1, 4, and 5 to be able to reach vertex 1 (with dfs_num 1). Thus dfs_low of {1, 4, 5} are all 1.

When we are in a vertex _u_, _v_ is a neighbor of _u_, and dfs_low(_v_) ≥ dfs_num(_u_), then _u_ is an articulation vertex. This is because the fact that dfs_low(_v_) is not smaller than dfs_num(_u_) implies that there is no back edge connected to vertex _v_ that can reach vertex _w_ with a lower dfs_num(_w_) (which further implies that _w_ is the parent of _u_ in the DFS spanning tree). Thus, to reach that parent of _u_ from _v_, one must pass through vertex _u_. This implies that removing the
vertex _u_ will disconnect the graph.

The process to find bridges is similar. When dfs_low(_v_) > dfs_num(_u_), then edge _u_-_v_ is a bridge. In the following example, almost all edges are bridges for the left and right graph. Only edges 1-4, 4-5, and 5-1 are not bridges on the right graph. This is because – for example – edge 4-5, dfs_low(5) ≤ dfs_num(4), i.e. even if this edge 4-5 is removed, we know for sure that vertex 5 can still reach vertex 1 via another path that bypass vertex 4 as dfs_low(5) = 1.

![APaB](https://i.imgur.com/0bW7oK9.png)

Following is an implementation of Articulation Points and Bridges.

```cpp

/* Articulation Points
O(V+E)
Finds all articulation points and bridges in a graph.
*/
vi low2, num2, parent, strongPoints;
int counter2, root, rootChildren;
void dfs1(Graph &g, int v) {
	low2[v] = num2[v] = counter2++;
	FORC(g.edges[v], edge) {
		if(num2[edge->to] == -1) {
			parent[edge->to] = v;
			if(v == root) rootChildren++;
			dfs1(g, edge->to);
			if(low2[edge->to] >= num2[v]) strongPoints[v] = true;
			if(low2[edge->to] > num2[v]) edge->strong = g.edges[edge->to][edge->backEdge].strong = true;
			low2[v] = min(low2[v], low2[edge->to]);
		} else if(edge->to != parent[v])
			low2[v] = min(low2[v], num2[edge->to]);
	}
}

vi articulationPointsAndBridges(Graph &g) {
	counter2 = 0;
	num2 = vi(g.V, -1), low2 = vi(g.V, 0), parent = vi(g.V, -1), strongPoints = vi(g.V, 0);
	FOR(i, 0, g.V)
		if(num2[i] == -1) {
			root = i, rootChildren = 0;
			dfs1(g, i);
			strongPoints[root] = rootChildren > 1;
		}
	return strongPoints;
}

```
