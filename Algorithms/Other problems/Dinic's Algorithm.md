# Dinic's Algorithm

We have seen the potentially unpredictable O(|_f_|E) Ford Fulkerson’s method and the preferred O(V * E^2) Edmonds Karp’s algorithm (finding augmenting paths with BFS) for solving the Max Flow problem. As of year 2013, most (if not all) Max Flow problems are solvable using Edmonds Karp’s.

There are several other Max Flow algorithms that have theoretically better performance than Edmonds Karp’s. One of them is Dinic’s algorithm which runs in O(V^2 * E). Since a typical flow graph usually has V < E and E << V^2, Dinic’s worst case time complexity is theoretically better than Edmonds Karp’s. Although there's rarely a case where Edmonds Karp’s receives TLE verdict and Dinic’s receiveds AC verdict on the same flow graph, it may be beneficial to use Dinic’s algorithm in programming contests just to be on the safer side.

Dinic’s algorithm uses a similar idea as Edmonds Karp’s as it also finds augmenting paths iteratively. However, Dinic’s algorithm uses the concept of ‘blocking flows’ to find the augmenting paths. Understanding this concept is the key to extend the easier-to-understand Edmonds Karp’s algorithm into Dinic’s algorithm.

Let’s define _dist[v]_ to be the length of the shortest path from the source vertex _s_ to _v_ in the residual graph. Then the level graph of the residual graph is L where edge (_u_, _v_) in the residual graph is included in the level graph L iff dist[_v_] = dist[_u_] + 1. Then, a ‘blocking flow’ is an _s − t_ flow _f_ such that after sending through flow _f_ from _s_ to _t_, the level graph L contains no _s − t_ augmenting path anymore.

It has been proven that the number of edges in each blocking flow increases by at least one per iteration. There are at most V − 1 blocking flows in the algorithm because there can only be at most V − 1 edges along the ‘longest’ simple path from _s_ to _t_. The level graph can be constructed by a BFS in O(E) time and a blocking flow in each level graph can be found in O(V * E) time. Hence, the worst case time complexity of Dinic’s algorithm is O(V * (E + V * E)) = O(V^2 * E).

Dinic’s implementation can be made similar with Edmonds Karp’s implementation. In Edmonds Karp’s, we run a BFS —which already generates for us the level graph L— but we just use it to find one augmenting path by calling the _augment(t, INF)_ function. In Dinic’s algorithm, we need to use the information produced by BFS in a slightly different manner. The key change is this: Instead of finding a blocking flow by running DFS on the level graph L, we can simulate the process by running the augment procedure from each vertex _v_ that is directly connected to the sink vertex _t_ in the level graph, i.e. _edge(v, t)_ exists in level graph L and we call _augment(v, INF)_. This will find many (but not always all) the required augmenting paths that make up the blocking flow of level graph L for us.

Following is an implementation of Dinic's algorithm:

```cpp

// Running time: O(|V|ˆ2 |E|)
// OUTPUT: maximum flow value;
// To obtain the actual flow values, look at all edges with
// capacity > 0 (zero capacity edges are residual edges).

struct Edge {
	int from, to, cap, flow, index;
	Edge(int from, int to, int cap, int flow, int index) : from(from), to(to), cap(cap), flow(flow), index(index) {}};
struct Dinic {
	int N;
	vector<vector<Edge> > G;
	vector<Edge *> dad;
	vector<int> Q;
	Dinic(int N) : N(N), G(N), dad(N), Q(N) {}
	void AddEdge(int from, int to, int cap) {
		G[from].push_back(Edge(from, to, cap, 0, G[to]. size()));
		if (from == to) G[from].back().index++;
		G[to].push_back(Edge(to, from, 0, 0, G[from].size() - 1));}
	long long BlockingFlow(int s, int t) {
		fill(dad.begin(), dad.end(), (Edge *) NULL);
		dad[s] = &G[0][0] - 1;
		int head = 0, tail = 0;
		Q[tail++] = s;
		while (head < tail) {
			int x = Q[head++];
			for (int i = 0; i < G[x].size(); i++) {
				Edge &e = G[x][i];
				if (!dad[e.to] && e.cap - e.flow > 0) {
					dad[e.to] = &G[x][i];
					Q[tail++] = e.to;}}}
		if (!dad[t]) return 0;
		long long totflow = 0;
		for (int i = 0; i < G[t].size(); i++) {
			Edge *start = &G[G[t][i].to][G[t][i].index];
			int amt = INF;
			for (Edge *e = start; amt && e != dad[s]; e = dad[e->from]) {
				if(!e){amt=0;break;}
				amt = min(amt, e->cap - e->flow);}
			if (amt == 0) continue;
			for (Edge *e = start; amt && e != dad[s]; e = dad[e->from]) {
				e->flow += amt;
				G[e->to][e->index].flow -= amt;}
			totflow += amt;}
		return totflow;}
	long long GetMaxFlow(int s, int t) {
		long long totflow = 0;
		while (long long flow = BlockingFlow(s, t))
			totflow += flow;
		return totflow;}};
		
```
