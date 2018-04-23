# Articulation Points and Bridges

```cpp

/* Articulation Points
O(V+E)
Finds all articulation points and bridges in a graph.
An articulation point is a vertex whose removal would disconnect the graph.
An bridge is a vertex whose removal disconnects the graph.
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
