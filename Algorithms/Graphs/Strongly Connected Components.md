# Strongly Connected Components

Yet another application of DFS is to find strongly connected components in a directed graph. This is a different problem to finding connected components of an undirected graph. A strongly connected component is a subset of a graph in which every vertex is reachable from every other vertex in the subset.

In the following example, we can see that although the graph looks like it has  one ‘connected’ component, it is actually not a 'strongly connected’ component. In directed graphs, we are more interested with the notion of ‘Strongly Connected Component (SCC)’. An SCC is defined as such: If we pick any pair of vertices _u_ and _v_ in the SCC, we can find a path from _u_ to _v_ and vice versa. There are actually three SCCs in this example, as highlighted: {0}, {1, 3, 2}, and {4, 5, 7, 6}.

![SCC](https://i.imgur.com/9KMt0Db.png)

There are at least two known algorithms to find SCCs: Kosaraju’s algorithm and Tarjan’s algorithm. In this case, we will adopt Tarjan’s version, as it extends naturally from the problem of finding Articulation Points and Bridges.

The basic idea of the algorithm is that SCCs form subtrees in the DFS spanning tree. On top of computing dfs_num(_u_) and dfs_low(_u_) for each vertex, we also append vertex _u_ to the back of a stack _S_ and keep track of the vertices that are currently explored via _visited_. The condition to update dfs_low(_u_) is that only vertices that currently have the _visited_ flag turned on can update dfs_low(_u_). Now, if we have vertex _u_ in this DFS spanning tree with dfs_low(_u_) = dfs_num(_u_), we can conclude that _u_ is the root (start) of an SCC and the members of those SCCs are identified by popping the current content of stack _S_ until we reach vertex _u_ (the root) of SCC again.

In this example, the content of _S_ is {0,1,2,3,4,5,6,7} when vertex 4 is identified as the root of an SCC (dfs_low(4) = dfs_num(4) = 4), so we pop elements in _S_ one by one until we reach vertex 4 and we have this SCC: {6,7,5,4}. Next, the content of _S_ is {0,1,3,2} when vertex 1 is identified as another root of another SCC (dfs_low(1) = dfs_num(1) = 1), so we pop elements in _S_ one by one until we reach vertex 1 and we have SCC: {2,3,1}. Finally, we have the last SCC with one member only: {0}.

Following is an implementation to find Strongly Connected Components in a graph that runs in O(V+E) time.

```cpp

vi low1, num1, components;
int counter1, SCCindex;
vector<bool> visited;
stack<int> S;

void dfs(Graph &g, int cv) {
	low1[cv] = num1[cv] = counter1++;
	S.push(cv);
	visited[cv] = true;
	FORC(g.edges[cv], edge) {
		if(num1[edge->to] == -1)
			dfs(g, edge->to);
		if(visited[edge->to])
			low1[cv] = min(low1[cv], low1[edge->to]);        
	}
	if(low1[cv] == num1[cv]) {
		int index = SCCindex++;
		while(true) {
			int v = S.top(); S.pop(); visited[v] = 0;
			components[v] = index;
			if (cv == v)
				break;
		}
	}
}

vi stronglyConnectedComponents(Graph &g) {
	counter1 = 0, SCCindex = 0;
	visited = vector<bool>(g.V, 0);
	num1 = vi(g.V, -1), low1 = vi(g.V, 0), components = vi(g.V, 0);
	S = stack<int>();
	FOR(i, 0, g.V)
		if(num1[i] == -1)
			dfs(g, i);
	return components;
}

```
