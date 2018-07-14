# Minimum Cut

In a flow network, an s-t cut is a cut that requires the source ‘s’ and the sink ‘t’ to be in different subsets, and it consists of edges going from the source’s side to the sink’s side. The capacity of an s-t cut is defined by the sum of the capacity of each edge in the cut-set. The Minimum Cut problem, often abbreviated as just Min Cut, is to find minimum capacity s-t cut of the given network. Expected output is all edges of the minimum cut. This problem is more general than finding bridges, i.e. in this case we can cut more than just one edge and we want to do so in the least cost way. As with bridges, Min Cut has applications in ‘sabotaging’ networks.

There are two ways of solving this problem: using Maximum Flow algorithm and using Stoer–Wagner algorithm.

## Using Maximum Flow Algorithm

Let’s define an s-t cut C = (S-component, T-component) as a partition of V ∈ G such that source s ∈ S-component and sink t ∈ T-component.  Let’s also define a _cut-set_ of C to be the set {(u, v) ∈ E | u ∈ S-component, v ∈ T-component} such that if all edges in the cut-set of C are removed, the Max Flow from s to t is 0 (i.e. s and t are disconnected). The cost
of an s-t cut C is defined by the sum of the capacities of the edges in the cut-set of C.

The solution is simple: The by-product of computing Max Flow is Min Cut!

Let’s see the following figure, which is the result to a Max Flow problem:

![Min Cut](https://i.imgur.com/GBF9QFh.png)

After Max Flow algorithm stops, we run graph traversal (DFS/BFS) from source s again. All reachable vertices from source s using positive weighted edges in the residual graph belong to the S-component (i.e. vertex 0 and 2). All other unreachable vertices belong to the T-component (i.e. vertex 1 and 3). All edges connecting the S-component to the T-component belong to the cut-set of C (edge 0-3 (capacity 30/flow 30/residual 0), 2-3 (5/5/0) and 2-1 (25/25/0) in this case). The Min Cut value is 30+5+25 = 60 = Max Flow value mf. This is the minimum over all possible s-t cuts value.

Using Edmonds–Karp maximal flow algorithm, the overall complexity is O(V^2 * E^2).

## Using Stoer–Wagner Algorithm

The Stoer–Wagner algorithm is an algorithm used to solve the minimum cut problem in undirected weighted graphs with non-negative weights. The essential idea of this algorithm is to shrink the graph by merging the most intensive vertices, until the graph only contains two combined vertex sets. At each phase, the algorithm finds the minimum s-t cut for two vertices s and t chosen as its will. Then the algorithm shrinks the edge between s and t to search for non s-t cuts. The minimum cut found in all phases will be the minimum weighted cut of the graph.

Following is an implementation of Stoer-Wagner Algorithm. This algorithm runs in O(V^3) time complexity.

```cpp

// Adjacency matrix implementation of Stoer-Wagner min cut algorithm.
// Running time: O(|V|^3)
// INPUT:
// 	- graph, constructed using AddEdge()
// OUTPUT:
// 	- (min cut value, nodes in half of min cut)

typedef vector<int> VI;
typedef vector<VI> VVI;
const int INF = 1000000000;

pair<int, VI> GetMinCut(VVI &weights) {
	int N = weights.size();
	VI used(N), cut, best_cut;
	int best_weight = -1;
	for (int phase = N - 1; phase >= 0; phase--) {
		VI w = weights[0];
		VI added = used;
		int prev, last = 0;
		for (int i = 0; i < phase; i++) {
			prev = last;
			last = -1;
			for(int j= 1; j < N; j++)
				if (!added[j] && (last == -1 || w[j] > w[last])) last=j;
			if (i == phase - 1) {
				for (int j = 0; j < N; j++) weights[prev][j] += weights[last][j];
				for (int j = 0; j < N; j++) weights[j][prev] = weights[prev][j];
				used[last] = true;
				cut.push_back(last);
				if (best_weight == -1 || w[last] < best_weight) {
					best_cut = cut;
					best_weight = w[last];
				}
			} else {
				for (int j = 0; j < N; j++) w[j] += weights[last][j];
				added[last] = true;
			}
		}
	}
	return make_pair(best_weight, best_cut);
}

```

