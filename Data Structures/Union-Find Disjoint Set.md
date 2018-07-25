# Union-Find Disjoint Set

The Union-Find Disjoint Set (UFDS) is a data structure used to model a collection of _disjoint sets_ with the ability to efficiently determine which set an item belongs to or to test whether two items belong to the same set and to unite two disjoint sets into one larger set. UFDS can be used to solve problems such as finding connected components in an undirected graph.

Two O(log(n)) time operations are supported:

1) **Unite**: Joins two sets.

2) **Find**: Finds the representative of the set that contains a given element.

In a union-find structure, one element in each set is the representative of the set, and there is a chain from any other element of the set to the representative.

**Example**

Assume that we have the sets {1,4,7}, {5}, and {2,3,6,8}:

![UFDS](https://i.imgur.com/GM4LddW.png)

In this case the representatives of the sets are 4, 5, and 2. We can find the representative of any element by following the chain that begins at the element. For example, the element 2 is the representative for the element 6, because we follow the chain 6 --> 3 --> 2. Two elements belong to the same set exactly when their representatives are the same.

Two sets can be joined by connecting the representative of one set to the representative of the other set. For example, the sets {1,4,7} and {2,3,6,8} can be joined as follows:

![UFDS](https://i.imgur.com/9gz5Gio.png)

The resulting set contains the elements {1,2,3,4,6,7,8}. From this on, the element 2 is the representative for the entire set and the old representative 4 points to the element 2.

The efficiency of the union-find structure depends on how the sets are joined. It turns out that we can follow a simple strategy: always connect the representative of the smaller set to the representative of the larger set (or if the sets are of equal size, we can make an arbitrary choice). Using this strategy, the length of any chain will be O(log(n)), so we can find the representative of any element efficiently by following the corresponding chain.

Following are two implementations of a UFDS. The first one is a regular implementation of UFDS. The second one has additional functionality to help us keep track of how many sets we have (_N_) as well as the size of each set (_sizes_).

```cpp

struct UnionFindDS {
	vi tree;
	UnionFindDS(int n) { FOR(i, 0, n) tree.pb(i); }
	int root(int i) { return tree[i] == i ? i : tree[i] = root(tree[i]); }
	bool connected(int i, int j) {return root(i) == root(j);}
	void connect(int i, int j) { tree[root(i)] = tree[root(j)]; }
};

struct UnionFindDS2 {
	vi tree, sizes;
	int N;
	UnionFindDS2(int n) : N(n) {
		tree.reserve(n);
		FOR(i, 0, n) tree[i] = i;
		sizes.assign(n, 1);
	}
	int root(int i) { return (tree[i] == i) ? i : (tree[i] = root(tree[i]));}
	int countSets() { return N;}
	int getSize(int i) { return sizes[root(i)];}
	bool connected(int i, int j) { return root(i) == root(j);}
	void connect(int i, int j) {
		int ri = root(i), rj = root(j);
		if(ri != rj) {
			N--;
			sizes[rj] += sizes[ri];
			tree[ri] = rj;
		}
	}
};

```

