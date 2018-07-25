# Push–relabel maximum flow algorithm

The push–relabel algorithm is considered one of the most efficient maximum flow algorithms. While the generic push–relabel algorithm has O(V^2 * E) time complexity, efficient implementations (like the one below) achieve O(V^3) or lower time complexity by enforcing appropriate rules in selecting applicable push and relabel operations. These all are asymptotically more efficient than the O(V * E^2) Edmonds–Karp algorithm. 

The name "push–relabel" comes from the two basic operations used in the algorithm. Throughout its execution, the algorithm maintains a "preflow" and gradually converts it into a maximum flow by moving flow locally between neighboring nodes using push operations under the guidance of an admissible network maintained by relabel operations. In comparison, the Ford–Fulkerson algorithm performs global augmentations that send flow following paths from the source all the way to the sink.

We are given a directed capacitated graph G = (V, E) with source s and sink t, and want to find a max s-t flow. Let's imagine all nodes are reachable from s, and can reach t. (All other nodes can be deleted.) For brevity, we denote the directed edges (u, v) belonging to a set E merely as uv.

### Preflows

The algorithm will send flow along edges. But in contrast to other algorithms, we will not maintain a flow at each point in time — we will only maintain a “preflow”. What’s a preflow? It's what we get when we don't have flow conservation at even the non-s-t nodes: in a preflow the flow entering the node is allowed to be more than the flow leaving it. (It is not allowed to be less.) So if f is a preflow, then for all v belonging to a set V \ {s, t}:

![Push-relabel](https://i.imgur.com/zHRkdRg.png)

Consider this s-t flow network:

![Push-relabel](https://i.imgur.com/e3jZkDm.png)

Here’s an example of a preflow:

![Push-relabel](https://i.imgur.com/5KD7NsX.png)

Every flow is trivially a preflow. And here is an example of something that is not a preflow, since node a has more flow leaving it than entering it:

![Push-relabel](https://i.imgur.com/a1aCeK4.png)

Given a preflow f, a node v has some excess value ef(v) ≥ 0, this is just the difference between the flow entering the node and the flow leaving it. Namely:

![Push-relabel](https://i.imgur.com/Px81us4.png)

A node v is active if v does not belong to {s, t} and ef(v) > 0. (In the preflow example above, the nodes a, c, d, e, are all active, with excesses of 2, 5, 1 and 2 respectively.)

Let Ef denote the edges of the residual graph with respect to the preflow (and let Gf denote the residual graph itself). As before, when you send flow f along an edge uv with capacity c, in the residual graph you get an edge uv with capacity c − f, and you also get the reverse edge vu with capacity f. If c = f then we drop the edge uv from Ef (since it has residual capacity c − f = 0), so Ef only contains the arcs with non-zero residual capacity. The residual graph corresponding to the preflow figure is this:

![Push-relabel](https://i.imgur.com/UjQuWWD.png)

### Labelings

Each node v is given a _label_ d(v) belonging to the set Z≥0 (Z refers to the integer numbers set). A labeling is valid for preflow f if for all edges uv in Ef, we have d(u) ≤ d(v) + 1. We will maintain the invariant that our current labeling is valid for the current preflow. Call an arc uv _admissible_ if uv belongs to Ef and d(u) ≥ d(v) + 1. Since we always have a valid labeling, we also have the inequality (d(u) ≤ d(v) + 1). Putting the two together, admissible edges uv in fact satisfy the _equality_ d(u) = d(v) + 1. These admissible edges will be the ones we will try to advance flow on.

Following is an implementation of the Push-Relabel Algorithm:

```cpp

// Significantly faster than straight Ford-Fulkerson.
// Solves random problems with 10000 vertices and 1000000 edges in a few seconds,
// though it is possible to construct test cases that achieve the worst case.
// Running time: O(|V|^3)
// INPUT:
// 	- graph, constructed using AddEdge()
// 	- source
// 	- sink
// OUTPUT:
// 	- maximum flow value
// 	- To obtain the actual flow values, look at all edges with capacity > 0 (zero capacity edges are residual edges).

typedef long long LL;
struct Edge {
	int from, to, cap, flow, index;
	Edge(int from, int to, int cap, int flow, int index): from(from), to(to), cap(cap), flow(flow), index (index) {}};
	
struct PushRelabel {
	int N;
	vector<vector<Edge> > G;
	vector<LL> excess;
	vector<int> dist, active, count;
	queue<int> Q;
	PushRelabel(int N) : N(N), G(N), excess(N), dist(N), active(N), count(2*N) {}
	void AddEdge(int from, int to, int cap) {
		G[from].push_back(Edge(from, to, cap, 0, G[to]. size()));
		if (from == to) G[from].back().index++;
		G[to].push_back(Edge(to, from, 0, 0, G[from].size() - 1));}
	void Enqueue(int v) {
		if (!active[v] && excess[v] > 0) { active[v] = true; Q.push(v); } }
	void Push(Edge &e) {
		int amt = int(min(excess[e.from], LL(e.cap - e.flow)));
		if (dist[e.from] <= dist[e.to] || amt == 0)
			return;
		e.flow += amt;
		G[e.to][e.index].flow -= amt; 
		excess[e.to] += amt; 
		excess[e.from] -= amt; 
		Enqueue(e.to);}
	void Gap(int k) {
		for (int v = 0; v < N; v++) {
			if (dist[v] < k) continue;
			count[dist[v]]--;
			dist[v] = max(dist[v], N+1);
			count[dist[v]]++;
			Enqueue(v);}}
	void Relabel(int v) { 
		count[dist[v]]--;
		dist[v] = 2*N;
		for (int i = 0; i < G[v].size(); i++)
			if (G[v][i].cap - G[v][i].flow > 0)
				dist[v] = min(dist[v], dist[G[v][i].to] + 1);
		count[dist[v]]++;
		Enqueue(v);}
	void Discharge(int v) {
		for (int i = 0; excess[v] > 0 && i < G[v].size(); i++)
			Push(G[v][i]);
		if (excess[v] > 0) {
			if (count[dist[v]] == 1)
				Gap(dist[v]);
			else
				Relabel(v);}}
	LL GetMaxFlow(int s, int t) {
		count[0] = N-1;
		count[N] = 1;
		dist[s] = N;
		active[s] = active[t] = true;
		for (int i = 0; i < G[s].size(); i++) {
			excess[s] += G[s][i].cap;
			Push(G[s][i]);}
		while (!Q.empty()) {
			int v = Q.front(); 
			Q.pop();
			active[v] = false;
			Discharge(v);}
		LL totflow = 0;
		for (int i = 0; i < G[s].size(); i++) totflow += G[s][i].flow;
		return totflow;}};
```
