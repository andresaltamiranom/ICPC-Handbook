# Maximum Bipartite Matching

A matching in a Bipartite Graph is a set of the edges chosen in such a way that no two edges share an endpoint. A maximum matching is a matching of maximum size (maximum number of edges). In a maximum matching, if any edge is added to it, it is no longer a matching. There can be more than one maximum matchings for a given Bipartite Graph.

There are many real world problems that can be formed as Bipartite Matching.

**Example**

There are M job applicants and N jobs. Each applicant has a subset of jobs that he/she is interested in. Each job opening can only accept one applicant and a job applicant can be appointed for only one job. Find an assignment of jobs to applicants in such that as many applicants as possible get jobs.

![alt text](https://www.geeksforgeeks.org/wp-content/uploads/maximum_matching1.png)

A Maximum Bipartite Matching (MBP) problem can be solved by converting it into a flow network and using an algorithm such as Ford Fulkerson's algorithm. However, there is a better way to solve the problem in programming contests (in terms of implementation time) rather than going via the Max Flow route. We can use the specialized and easy to implement O(VE) _augmenting path_ algorithm.

An augmenting path is a path that starts from a _free (unmatched)_ vertex on the left set of the Bipartite Graph, alternate between a free edge (now on the right set), a matched edge (now on the left set again), ..., a free edge (now on the right set) until the path finally arrives on a _free vertex_ on the right set of the Bipartite Graph.

A lemma by Claude Berge in 1957 states that a matching _M_ in a graph _G_ is maximum (has the maximum possible number of edges) if and only if there is no more augmenting path in _G_. This augmenting path algorithm is a direct implementation of Berge's lemma: Find and then eliminate _augmenting paths_.

With its handy implementation, all the MBP problems can be easily solved. With this implementation, we can also solve the Min Vertex Cover in Bipartite Graph problem.

Following is an implementation of MBP.

_augment()_ is used in _maxMatching()_ to solve the MBP problem and _augment2()_ is used in _minCover()_ to solve the Min Vertex Cover in Bipartite Graph problem.

```cpp

struct MaxBipartiteMatching {
	int L, R;
	vvi edgesL;
	vi visitedL, matchR, matchL, inCoverL, inCoverR;
	MaxBipartiteMatching(int L, int R) : L(L), R(R) { edgesL.assign(L, vi()); }
	void addEdge(int l, int r) { edgesL[l].pb(r); }
	bool augment(int l) {
		if (visitedL[l]) return 0;
		visitedL[l] = 1;
		for (auto r: edgesL[l])
			if (matchR[r] == -1 || augment(matchR[r])) { matchR[r] = l; return 1; }
		return 0;
	}
	int maxMatching() {
		int ans = 0;
		matchR.assign(R, -1), matchL.assign(L, -1);
		for(int i = 0; i < L; i++)
			visitedL.assign(L, 0), ans += augment(i);
		for(int i = 0; i < R; i++) if (matchR[i] != -1) matchL[matchR[i]] = i;
		return ans;
	}
	void augment2(int l) {
		if (l == -1 || !inCoverL[l]) return;
		inCoverL[l] = 0;
		for (auto r: edgesL[l])
			if (!inCoverR[r]) inCoverR[r] = 1, augment2(matchR[r]);
	}
	void minCover() { // assuming matching found
		inCoverL.assign(L, 1), inCoverR.assign(R, 0);
		for(int i = 0; i < L; i++)
			if (matchL[i] == -1) augment2(i);
	}
};

```

