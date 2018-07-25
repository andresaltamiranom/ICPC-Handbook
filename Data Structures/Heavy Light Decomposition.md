# Heavy Light Decomposition

Heavy Light Decomposition (HLD), also called Heavy Path Decomposition, is a technique for decomposing a rooted tree into a set of paths. The importance of decomposing the tree is that each decomposed path will give us asymptotic time bounds for certain problems involving trees.

The utility of the HLD lies in that problems involving paths between nodes can often be solved more efficiently by skipping over heavy paths rather than considering each edge in the path individually. This is because long paths in trees that are very poorly balanced tend to consist mostly of heavy edges.

To apply HLD, we divide the rooted tree into vertex-disjoint chains (meaning that no two chains have a node in common) in such a way that to move from any node in the tree to the root node, we will have to change at most log(N) chains. The path from any node to root can be broken into pieces such that each piece belongs only to one chain.

Now, the path from any node _A_ to any node _B_ can be  broken into two paths: _A_ to _LCA(A, B)_ and _B_ to _LCA(A, B)_. So at this point we only need to worry about paths of the following format: Start at some node and go up the tree because _A_ to _LCA(A, B)_ and _B_ to _LCA(A, B)_ are both such paths.

So we know the following regarding HLD:

1) We can break the tree into chains such that we will have to change at most log(N) chains to move from any node up the tree to any other node.

2) Any path can be broken into two paths such that both paths start at some node and move up the tree.

3) Queries in each chain can be answered with O(log(N)) complexity and there are at most log(N) chains we need to consider per path. So, in the end, we will have a O(log^2(N)) complexity solution to process queries on the tree.

Let's try to solve an example to better understand how HLD is used.

**Example Problem**

You are given a tree (an acyclic undirected connected graph) with N nodes, and edges numbered 1, 2, 3...N-1.

We will ask you to perform some instructions of the following form:

1) CHANGE _i_ _ti_: Change the cost of the _i_-th edge to _ti_.

2) QUERY _a_ _b_: Ask for the maximum edge cost on the path from node _a_ to node _b_.

To solve this, Heavy Light Decomposition will allow us to respond to these queries in a quick manner.

Suppose we are given the following tree:

![alt text](https://i.imgur.com/QmRnrn1.png)

After applying Heavy Light Decomposition, it will look something like this:

![alt text](https://i.imgur.com/HAPMEVu.png)

Each edge has a number associated with it. Given 2 nodes _A_ and _B_, we need to find the edge on path from _A_ to _B_ with the maximum value. We can break the path into _A_ to _LCA(A, B)_ and _B_ to _LCA(A, B)_, calculate the answer for each of them, and take the maximum of both.

Here is how a query would be processed on a path from U to V.

![alt text](https://i.imgur.com/QNoyMhq.png)

Following is an implementation of HLD.

```cpp

struct HeavyLightDecomposition {
	vector<vi> lists;
	vi values, listIndex, posIndex, parent, treeSizes;
	vector<SparseTable> sts;
	LCA *lca;
	HeavyLightDecomposition(Graph &g, vi values) : values(values) {
		lca = new LCA(g, 0);
		listIndex = posIndex = parent = treeSizes = vi(g.V, -1);
		getTreeSizes(g, 0);
		makeLists(g, 0, -1);
		FORC(lists, list) {
			vi v;
			FORC(*list, it) v.pb(values[*it]);
			sts.pb(SparseTable(v));
		}
	}
	~HeavyLightDecomposition() { delete lca; }
	int getTreeSizes(Graph &g, int cv) {
		treeSizes[cv] = 1;
		FORC(g.edges[cv], edge) if(edge->to != parent[cv])
				parent[edge->to] = cv, treeSizes[cv] += getTreeSizes(g, edge->to);  
		return treeSizes[cv];
	}
	void makeLists(Graph &g, int cv, int listNum) {
		if(listNum == -1)
			listNum = lists.size(), lists.pb(vi());
		listIndex[cv] = listNum;
		posIndex[cv] = lists[listNum].size();
		lists[listNum].pb(cv);
		int MAX = -1;
		FORC(g.edges[cv], edge) if(edge->to != parent[cv])
			if(MAX == -1 || treeSizes[edge->to] > treeSizes[MAX]) MAX = edge->to;
		FORC(g.edges[cv], edge) if(edge->to != parent[cv])
			makeLists(g, edge->to, edge->to == MAX ? listNum : -1);	
	}
	int query(int from, int to) {
		int anc = lca->query(from, to), posLeft, posRight;
		int result = min(queryToAncestor(from, anc, posLeft), queryToAncestor(to, anc, posRight));
		if(posLeft < posRight) swap(posLeft, posRight);
		result = min(result, values[lists[listIndex[anc]][sts[listIndex[anc]].query(posIndex[anc], posRight)]]);
		if(posRight != posLeft)
			result = min(result, values[lists[listIndex[anc]][sts[listIndex[anc]].query(posRight+1, posLeft)]]);
		return result;
	}
	int queryToAncestor(int from, int anc, int &posInAncestorList) {
		int result = INF, left = from;
		while(listIndex[left] != listIndex[anc]) {
			result = min(result, values[lists[listIndex[left]][sts[listIndex[left]].query(0, posIndex[left])]]);
			left = parent[lists[listIndex[left]][0]];
		}
		posInAncestorList = posIndex[left];
		return result;
	}
};

```
