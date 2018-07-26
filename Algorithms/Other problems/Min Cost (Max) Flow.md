# Min Cost (Max) Flow

## Problem Description

The Min Cost Flow problem is the problem of finding the _cheapest_ possible way of sending a certain amount of (usually max) flow through a flow network. In this problem, every edge has two attributes: The flow capacity through this edge _and the unit cost_ for sending one unit flow through this edge. Some problem authors choose to simplify this problem by setting the edge capacity to a constant integer and only vary the edge cost.

![MCMF](https://i.imgur.com/7ZRPpjl.png)

In the left figure above, each edge has a uniform capacity of 10 units and a unit cost as shown in the edge label. We want to send 20 units of flow from A to D (note that the max flow of this flow graph is 30 units) which can be satisfied by sending 10 units of flow A -> D with cost 1 * 10 = 10 (middle figure); plus another 10 units of flow A -> B -> D with cost (3 + 4) * 10 = 70 (right figure). The total cost is 10 + 70 = 80 and this is the minimum. Note that if we choose to send the 20 units of flow via A -> D (10 units) and A -> C -> D instead, we incur a cost of 1 * 10 + (3 + 5) * 10 = 10 + 80 = 90. This is higher than the optimal cost of 80.

## Solution(s)

The Min Cost (Max) Flow, or in short MCMF, can be solved by replacing the O(E) BFS (to find the shortest -in terms of number of hops- augmenting path) in Edmond Karp's algorithm into the O(V * E) Bellman Ford's (to find the shortest/cheapest -in terms of the _path cost_- augmenting path). We need a shortest path algorithm that can handle negative edge weights, as such negative edge weights _may appear_ when we cancel a certain flow along a backward edge (as we have to _substract_ the cost taken by this augmenting path as cancenling flow means that we do not want to use that edge).

The needs to use shortest path algorithm like Bellman Ford's slows down the MCMF implementation to around O(V^2 * E^2), but this is usually compensated by the problem author of most MCMF problems by having smaller input graph constraints.

Following is an implementation for MCMF:

```cpp

// Running time, O(|V|^2) cost per augmentation
// max flow: O(|V|^3) augmentations
// min cost max flow: O(|V|^4 * MAX_EDGE_COST) augmentations
// INPUT:
// 	- graph, constructed using AddEdge()
// 	- source
// 	- sink
// OUTPUT:
// 	- (maximum flow value, minimum cost value)
// 	- To obtain the actual flow, look at positive values only.

typedef vector<int> VI
typedef vector<VI> VVI;
typedef long long L;
typedef vector<L> VL;
typedef vector<VL> VVL;
typedef pair<int, int> PII;
typedef vector<PII> VPII;
const L INF = numeric_limits<L>::max() / 4;
struct MinCostMaxFlow {
	int N;
	VVL cap, flow, cost;
	VI found;
	VL dist, pi, width;
	VPII dad;
	MinCostMaxFlow(int N) : N(N), cap(N, VL(N)), flow(N, VL(N)), cost(N, VL(N)), found(N), dist(N), pi(N), width(N), dad(N) {}
	void AddEdge(int from, int to, L cap, L cost) {
		this->cap[from][to] = cap;
		this->cost[from][to] = cost;}
	void Relax(int s, int k, L cap, L cost, int dir) {
		L val = dist[s] + pi[s] - pi[k] + cost;
		if (cap && val < dist[k]) {
			dist[k] = val;
			dad[k] = make_pair(s, dir);
			width[k] = min(cap, width[s]);}}
	L Dijkstra(int s, int t) {
		fill(found.begin(), found.end(), false);
		fill(dist.begin(), dist.end(), INF);
		fill(width.begin(), width.end(), 0);
		dist[s] = 0;
		width[s] = INF;
		while (s != -1) {
			int best = -1;
			found[s] = true;
			for (int k = 0; k < N; k++) {
				if (found[k]) continue;
				Relax(s, k, cap[s][k] - flow[s][k], cost[s][k], 1);
				Relax(s, k, flow[k][s], -cost[k][s], -1);
				if (best == -1 || dist[k] < dist[best])
					best = k;}
			s = best;}
		for (int k = 0; k < N; k++)
			pi[k] = min(pi[k] + dist[k], INF);
		return width[t];}
	pair<L, L> GetMaxFlow(int s, int t) {
		L totflow = 0, totcost = 0;
		while (L amt = Dijkstra(s, t)) {
			totflow += amt;
			for (int x = t; x != s; x = dad[x].first) {
				if (dad[x].second == 1) {
					flow[dad[x].first][x] += amt;
					totcost += amt * cost[dad[x].first][x];
				} else {
					flow[x][dad[x].first] -= amt;
					totcost -= amt * cost[x][dad[x].first];}}}
		return make_pair(totflow, totcost);}};

```
