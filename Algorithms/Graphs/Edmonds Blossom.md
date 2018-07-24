# Edmonds Blossom

The blossom algorithm can be used on any graph to construct a maximum matching. The blossom algorithm improves upon the Hungarian algorithm by shrinking cycles in the graph to reveal augmenting paths. Additionally, the Hungarian algorithm only works on weighted bipartite graphs, but the blossom algorithm will work on any graph. The blossom algorithm is a polynomial time maximum graph matching algorithm. This algorithm has many applications, for example, assignment problems such as the Hall's marriage theorem, and path problems such as the Hamiltonian path problem.

## Algorithm

The blossom algorithm takes a general graph G = (V, E) and finds a maximum matching M. The algorithm starts with an empty matching and then iteratively improves it by adding edges, one at a time, to build augmenting paths in the matching M. Adding to an augmenting path can grow a matching since every other edge in an augmenting path is an edge in the matching; as more edges are added to the augmenting path, more matching edges are discovered.

The blossom algorithm has three possible results after each iteration:
- The algorithm finds no augmenting paths, in which case it has found a maximum matching.
- An augmenting path can be found in order to improve the outcome.
- A blossom can be found in order to manipulate it and discover a new augmenting path.

### Augmenting Paths

An augmenting path is an odd length path that has unmatched nodes for endpoints. The edges in an augmenting path alternate: one segment will be in the matching, the next segment is not in the matching, the next segment is in the matching, and so on. The algorithm works by finding augmenting paths so that it can increase the size of the matching by one each iteration.

### Blossoms

The idea behind blossoms is that an odd-length cycle in the graph can be contracted to a single vertex so that the search can continue iteratively in the now contracted graph. In the following figure, these odd-length cycles are the blossoms. The algorithm can just go on through the graph and treat the tricky cycle as if it were only a single node — fooling the algorithm into thinking there is an even number of nodes in the graph (it has explored so far) so that it can run the Hungarian algorithm on it.

A blossom is a cycle in G consisting of 2k + 1 edges of which exactly k belong to M, and where one of the vertices v in the cycle (the base) is such that there exists an alternating path of an even length, called a stem, from v to an exposed vertex w. The advantage is given due to the ease at which the stem part can be toggled between edges in the blossom.

![Edmonds Blossom](https://ds055uzetaobb.cloudfront.net/image_optimizer/47535fd1ac93eaaf94af13174fa6648d42a45b19.png)

The goal of the algorithm is to shrinks a blossom into a single node in order to reveal augmenting paths. It works by running the Hungarian algorithm until it runs into a blossom, which it then shrinks down into a single vertex. Then, it begins the Hungarian algorithm again. If another blossom is found, it shrinks the blossom and starts the Hungarian algorithm, and so on.

![Edmonds Blossom](https://ds055uzetaobb.cloudfront.net/image_optimizer/2aae0151bc6976d02780e6187f5cadd1ebdd3b14.png)

Notice the toggling in edges to make another matched edge the stem of the blossom.

![Edmonds Blossom](https://ds055uzetaobb.cloudfront.net/image_optimizer/47213ea24e7978bca2464721c5a566a38442bc29.png)



## Complexity

The blossom algorithm has three outcomes contributing to the runtime complexity: it may find a maximum matching, an augmenting path, or a blossom. In the worst case, this algorithm has a total runtime of O(|E||V|^2), where each iteration takes O(|E|) as each edge is scanned in hopes of asserting a perfect matching. If an augmenting path is found, the matching can be reconfigured in O(|V|) time, taking O(|E||V|) time. Lastly, if a blossom is found, the smaller graph has to be recursed, which can occur repeatedly for a maximum of O(|V|) recursions, at which point either an augmenting path is found, or the algorithm terminates. Since each time a blossom is recursed, there is a new set of augmenting paths, the algorithm can take a maximum runtime of O(|E||V|^2).

Following is an implementation of the Edmonds Blossom algorithm:

```cpp

struct edge {
	int v, nx;
};

const int MAXN = 1000, MAXE = 2000;
edge graph[MAXE];
int last[MAXN], match[MAXN], px[MAXN], base[MAXN], N, edges;
bool used[MAXN], blossom[MAXN], lused[MAXN];

inline void add_edge(int u, int v) {
	graph[edges] = (edge) {v, last[u]};
	last[u] = edges++;
	graph[edges] = (edge) {u, last[v]};
	last[v] = edges++;
}

void mark_path(int v, int b, int children) {
	while (base[v] != b) {
		blossom[base[v]] = blossom[base[match[v]]] = true;
		px[v] = children;
		children = match[v];
		v = px[match[v]];
	}
}

int lca(int a, int b) {
	memset(lused, 0, N);
	while (1) {
		lused[a = base[a]] = true;
		if (match[a] == -1) break;
		a = px[match[a]];
	}
	while (1) {
		b = base[b];
		if (lused[b]) return b;
		b = px[match[b]];
	}
}

int find_path(int root) {
	memset(used, 0, N);
	memset(px, -1, sizeof(int) * N);
	for (int i = 0; i < N; ++i) {
		base[i] = i;
	}
	used[root] = true;
	queue<int> q;
	q.push(root);
	int v, e, to, i;
	while (!q.empty()) {
		v = q.front(); q.pop();
		for (e = last[v]; e >= 0; e = graph[e].nx) {
			to = graph[e].v;
			if (base[v] == base[to] || match[v] == to) continue;
			if (to == root || (match[to] != -1 && px[match[to]] != -1)) {
				int curbase = lca(v, to);
				memset(blossom, 0, N);
				mark_path(v, curbase, to);
				mark_path(to, curbase, v);
				for (i = 0; i < N; ++i) {
					if (blossom[base[i]]) {
						base[i] = curbase;
						if (!used[i]) {
							used[i] = true;
							q.push(i);
						}
					}
				}
			} else if (px[to] == -1) {
				px[to] = v;
				if (match[to] == -1) return to;
				to = match[to];
				used[to] = true;
				q.push(to);
			}
		}
	}
	return -1;
}

void build_pre_matching() {
	int u, e, v;
	for (u = 0; u < N; ++u) {
		if (match[u] == -1) {
			for (e = last[u]; e >= 0; e = graph[e].nx) {
				v = graph[e].v;
				if (match[v] == -1) {
					match[u] = v;
					match[v] = u;
					break;
				}
			}
		}
	}
}
				
void edmonds() {
	memset(match, 0xff, sizeof(int) * N);
	build_pre_matching();
	int i, v, pv, ppv;
	for (i = 0; i < N; ++i) {
		if (match[i] == -1) {
			v = find_path(i);
			while (v != -1) {
				pv = px[v], ppv = match[pv];
				match[v] = pv, match[pv] = v;
				v = ppv;
			}
		}
	}
}

```
