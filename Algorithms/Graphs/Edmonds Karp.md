# Edmonds Karp

Imagine a connected, weighted, and directed graph as a pipe network where the pipes are the edges and the splitting points are the vertices. Each pipe has a capacity equal to the weight of the edge. There are also two special vertices: source _s_ and sink _t_. What is the maximum flow from source _s_ to sink _t_ in this graph (imagine water flowing in the pipe network. We want to know the maximum volume of water that can be passed by this pipe network)? This problem is called the Max Flow problem. 

Following is an illustration of this problem.

![Edmonds Karp](https://i.imgur.com/OuicSgW.png)

One solution is the Ford Fulkerson’s method. Ford Fulkerson’s method implemented using DFS can run in O(|_f_|E) where _f_ is the max flow value. This is because we can have a graph where every path augmentation only decreases the edge capacity along the path by 1. This is going to be repeated _f_ times. Because DFS runs in O(E) in a flow graph, the overall time complexity is O(|_f_|E). We do not want this unpredictability in programming contests as the author can choose to give a very large |_f_| value.

A better implementation of Ford Fulkerson's method is to use BFS for finding the shortest path in terms of number of layers/hops between source _s_ and sink _t_. This algorithm runs in O(VE^2) as it can be proven that after O(VE) BFS iterations, all augmenting paths will already be exhausted. As BFS runs in O(E) in a flow graph, the overall time complexity is O(VE^2).

Following is an implementation of Edmonds Karp algorithm.

```cpp

/* Edmonds-Karp
Finds a the max flow from source to sink of a directed graph.
The weight of an edge denotes the capacity of the edge.
The negative weight edges are the edges with flow.
*/

int augment(MatrixGraph &g, int flow, vi &parent, int source, int cv, int minEdge) {
	if(cv == source)
		return minEdge;
	if(parent[cv] != -1) {
		flow = augment(g, flow, parent, source, parent[cv], min(minEdge, g.edges[parent[cv]][cv].weight));
		g.edges[parent[cv]][cv].weight -= flow;
		g.edges[cv][parent[cv]].weight += flow;
	}
	return flow;
}

int maxFlow(MatrixGraph &g, int source, int sink) {
	int mf = 0, flow = -1;
	while(flow) {
		vi distanceTo(g.V, INF);
		distanceTo[source] = 0;
		queue<int> q; q.push(source);
		vi parent(g.V, -1);
		while(!q.empty()) {
			int cv = q.front(); q.pop();
			if(cv == sink) break;
			FOR(i, 0, g.V)
				if(g.edges[cv][i].weight > 0 && distanceTo[i] == INF)
					distanceTo[i] = distanceTo[cv] + 1, q.push(i), parent[i] = cv;
		}
		mf += flow = augment(g, 0, parent, source, sink, INF);
	}
	return mf;
}

```
